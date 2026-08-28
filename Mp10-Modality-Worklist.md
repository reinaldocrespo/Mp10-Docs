---
title: "Mp10 Modality Worklist"
subtitle: "Installing, running and troubleshooting the DICOM worklist server"
author: "Structured Systems · www.structuredsystems.com"
lang: en
toc: true
toc-depth: 3
numbersections: true
geometry: margin=1in
colorlinks: true
linkcolor: blue
urlcolor: blue
header-includes: |
  \usepackage{titlesec}
  \newcommand{\sectionbreak}{\clearpage}
  \usepackage{newunicodechar}
  \newunicodechar{→}{\ensuremath{\rightarrow}}
  \newunicodechar{−}{\ensuremath{-}}
---

# About this guide

This guide is for whoever looks after the server: installing the Mp10 modality
worklist, connecting imaging equipment to it, and working out what is wrong
when a technologist says the worklist is empty.

A **modality worklist** is how a scanner finds out who its next patient is.
Instead of a technologist typing the patient's name, date of birth and
accession number at the console, the machine asks Mp10 and fills its own screen
in. That is worth more than the keystrokes it saves: a study labelled by hand
is a study that can be labelled wrongly, and a mistyped accession number is
what puts an image in the wrong patient's file.

Two audiences, one document. The chapters on *what it is* and *day-to-day* are
readable by anyone; the install and troubleshooting chapters assume you are
administering the server.

# How it fits together

A scanner does not speak HTTP. It speaks **DICOM**, over a TCP port, and asks a
question called **C-FIND**. So the worklist is two pieces that meet in a
directory on disk:

```
   Mp10 orders (ADS)
        |
        |  AutoTasks service, every ~2 minutes
        v
   the worklist directory        e.g. C:\Mp10Mwl\worklists
   one small file per study      OR26-00041471.wl
        |
        |  DICOM worklist server (Orthanc), reading that directory
        v
   C-FIND  <--------------------  the CT / ultrasound / X-ray room
```

| Piece | What it is | Where it comes from |
|---|---|---|
| **The generator** | Part of the **AutoTasks** service. Decides which studies belong on the worklist and writes one file each | Installed with Mp10; switched on in the System Registry |
| **The worklist directory** | An ordinary folder. AutoTasks owns it completely | Created by the installer |
| **The DICOM server** | **Orthanc** with its worklist plugin, running as a Windows service | Installed once, from the Mp10 Modality Worklist bundle |
| **The console** | The Worklist page in Mp10 Web — read-only, for looking at what is scheduled | Part of Mp10 Web |

**The directory is the entire interface between the two halves.** AutoTasks
writes into it; Orthanc reads out of it; neither knows the other exists. That
is what makes the failure described on page one of the troubleshooting chapter
so common and so quiet — point them at different directories and each half
works perfectly while the scanner shows nothing.

## What is *not* here

This worklist replaces the worklist only. Images, the diagnostic viewer,
dictation and report distribution are unchanged, and the HL7 orders Mp10
already sends to your imaging system keep flowing exactly as before.

The worklist server stores no images. It is configured to refuse them
outright — it must never quietly become a second archive.

> **The worklist files themselves are patient data.** Each one carries a name,
> a date of birth and a medical record number. Keep the directory on local
> disk, off any share, and out of anything that copies folders to a cloud
> service. Orthanc's own web port is bound to the server's loopback address for
> the same reason: it is a health check, not a viewer.

# Installing

## Before you start

You need three answers. Get them from whoever configures the imaging equipment,
because changing them afterwards means visiting every scanner:

| | Bench default | What it is |
|---|---|---|
| **AE title** | `MP10MWL` | The name scanners call this server by. Up to 16 characters, no spaces |
| **DICOM port** | `4242` | The TCP port. Clinical sites usually want `104` or `11112` |
| **Worklist directory** | `C:\Mp10Mwl\worklists` | Where the files live. Local disk, not a network share |

You also need, for each scanner: its **AE title**, its **IP address**, and —
this one is easy to forget — **which fields it filters its query by**. Most
filter by modality and date. Some also filter by *Scheduled Station AE Title*
— "only my work" — which Mp10 supports and which is how you stop three CTs
showing each other's patients, but which needs the mapping in *Sending each
scanner its own work* filled in first. Find that out during the first scanner
test, not during a clinic.

## Install the DICOM server

On the server that will host the worklist, as **Administrator**:

```
powershell -NoProfile -ExecutionPolicy Bypass -File .\Install-Mwl.ps1
```

or, for a clinical install with the port and the scanners known:

```
.\Install-Mwl.ps1 -DicomPort 11112 `
                  -Modality "CT01,192.168.1.50" `
                  -Modality "MR01,192.168.1.51"
```

The installer needs no internet access: the Orthanc installer travels inside
the bundle. It installs Orthanc, creates the worklist tree, installs **only**
the worklist plugin, writes a locked-down configuration, opens the firewall
port, restarts the service and then proves the server is answering before it
says it is done.

With `-Modality` given, only those machines may query and their source address
is checked. With none given, any host on the network may read the worklist —
acceptable on a bench, worth tightening before go-live.

> **After any Orthanc upgrade, re-run `Install-Mwl.ps1`.** An upgrade restores
> the vendor's own configuration files, which turns worklist serving **off**.
> Nothing fails loudly when that happens: the service runs, the port answers,
> and every query returns nothing. `Test-Mwl.ps1` reports it in one line.

## Switch on the Mp10 half

In **Admin10 → Global System Registry**, in this order:

**1. Create the table.**

| Section | Entry | Set to |
|---|---|---|
| `GENERAL` | `MODALITY-WORKLIST` | `YES` |

Then run a **file check** in Admin10. The table is created during that check
and at no other time — setting this *after* a check has run does nothing until
the next one.

Station routing needs **data dictionary 10.72 or later**. The same file check
creates `mwlstations` and grants rights on it; until it has run, the generator
logs `station routing is OFF` and publishes every study unrouted — safe, but
Imaging Stations will not appear in Admin10.

**2. Turn the generator on.**

| Section | Entry | Set to |
|---|---|---|
| `AUTOTASKS` | `EXPORT_MWL` | `YES` |
| `AUTOTASKS` | `MWL_SPOOL_PATH` | the worklist directory, e.g. `C:\Mp10Mwl\worklists` |
| `AUTOTASKS` | `MWL_WINDOW_DAYS` | `1` unless you have a reason |

No restart is needed. **AutoTasks re-reads every setting at the start of each
cycle**, so a change takes effect within a couple of minutes. (If you are
watching for it, `AutoTasks.exe --mwl-run` applies it immediately.)

> These entries usually exist already, set to `NO` or blank. That is Mp10
> writing down a question it asked, not somebody else's change. Your value
> always wins once you set it.

`MWL_SPOOL_PATH` must be **the same directory** the installer reported. Not a
share pointing at it, not a different capitalisation of a different folder —
the same directory.

## Fill in the modality mapping

A worklist entry has to say whether the study is a CT, an ultrasound or an
X-ray. Mp10 takes that from the **revenue code** on the order: each revenue
code carries the kind of imaging it represents.

Run the starter mapping once per practice:

```
Services\docs\seed-revcodes-modality.sql
```

It fills the standard revenue codes and deliberately leaves non-imaging ones
blank. Then correct any local codes it did not cover.

**Studies with no modality are not published at all.** That is deliberate:
DICOM has no way to say "kind of study unknown", and a blank one would match no
scanner's query anyway. It is also the filter that keeps laboratory work and
office visits off the imaging worklist. AutoTasks counts the unmapped ones
every cycle, with examples — drive that number down to the point where what
remains is genuinely non-imaging.

## Prove it, in the right order

Test the DICOM half **before** any patient data is involved. Each step tells
you something the next one cannot.

**1. Is the server healthy?**

```
powershell -File .\Test-Mwl.ps1 -ExpectedSpoolPath "C:\Mp10Mwl\worklists"
```

Everything green means the service is up, the configuration is Mp10's, the two
halves agree on the directory, the port is listening and the firewall is open.

**2. Can a machine see a study we publish?**

```
AutoTasks.exe --mwl-selftest "C:\Mp10Mwl\worklists"
```

That writes one obviously-fake study — patient **MP10 SELFTEST / DO NOT SCAN**,
a CT, scheduled today — using exactly the same machinery a real study goes
through, with no database involved. Query the worklist from a scanner. If it
appears, the whole DICOM path works: file format, server, port, firewall,
scanner configuration.

The test file disappears on the next generator cycle, because AutoTasks owns
that directory. Delete it by hand if you want it gone sooner.

**3. Do real studies come through?**

```
AutoTasks.exe --mwl-run
```

One worklist cycle and nothing else — no backups, no remittances, no e-mail.
Safe to run in the middle of a clinic. It prints what it found and what it
wrote. Then query from the scanner again.

If step 2 works and step 3 does not, the problem is in the **data**: nothing
scheduled in the window, or revenue codes not mapped. If step 2 fails too, the
problem is in the **DICOM half**, and step 1 says which part.

# Sending each scanner its own work

With one CT, every CT study can go to every CT and nothing is wrong. With three,
a technologist at one site should not be looking at another site's patients to
find today's list.

DICOM's answer is **Scheduled Station AE Title**: the worklist entry says which
station the study is booked on, and each scanner asks only for its own. Mp10
has no station field on an order — nobody chooses a scanner when the study is
ordered — so the station is **derived**:

```
   the encounter's location   (admit type: "Site A", "Site B" ...)
                +
   the study's modality       (from the revenue code: CT, US, DX ...)
                |
                v
   Tables -> Imaging Stations  ->  AE title the scanner calls itself
```

Nothing is typed at order entry, and adding a scanner is one row in the
**Imaging Stations** screen, not a code change.

## Where the screen is

**Admin10 → the "Tables" panel on the left → Imaging Stations**, between
*Referring Phys* and *Fee Schedule*. It is a long list; the entry is about
two-thirds of the way down.

> **If it is not there**, the feature is switched off in this dictionary. The
> link only appears where `GENERAL / MODALITY-WORKLIST` is `YES` — the same
> setting that decides whether the table exists at all, so a link to a missing
> table can never appear. Set it, run **Update Data Dictionary**, and restart
> Admin10: the panel is built once, when the program starts.

The screen also needs the dictionary at **version 10.72 or later**. Opening it
against an older one says so rather than failing quietly.

## The rows

| Field | What it is |
|---|---|
| **Location** | Must match the encounter type used for that site. Spelling matters, capitalisation does not |
| **Modality** | `CT`, `MR`, `DX`, `US`, `MG` — the same value the revenue code maps to |
| **AE Title** | What the scanner calls itself when it asks for its worklist. This is the field that does the routing |
| **Station Name** | Free label, shown on some consoles |
| **Notes** | For people, not for DICOM — room, vendor, whoever to call |
| **Inactive** | Tick to stop routing to it without deleting the row |

One row per location and modality. **Two X-ray rooms in the same building share
a worklist** — that is Phase 1 as designed, not a fault. Splitting them needs a
finer key than location and modality (body part is the intended next step).

### Where each value comes from

| | |
|---|---|
| **Location** | The encounter types you already use. If encounters are opened as `Site A`, `Site B` and `Site C`, those are the three values — typed exactly as they appear on the encounter, not abbreviated |
| **Modality** | Whatever the revenue codes map to. If no revenue code maps to `MR`, a station for `MR` will never be used, and the screen says so when you save |
| **AE Title** | **From the scanner, not from you.** It is the name the device calls *itself* in its worklist query — read it off the machine's DICOM configuration, or ask whoever installed it. Up to 16 characters, no spaces. Getting this wrong is invisible: the row saves, looks right, and routes nothing |

### A worked example

Three buildings, one CT and one X-ray room in each:

| Location | Modality | AE Title | Station Name | Notes |
|---|---|---|---|---|
| Site A | CT | `SITEA_CT` | Site A CT | Siemens, room 2 |
| Site A | DX | `SITEA_DX` | Site A X-ray | |
| Site B | CT | `SITEB_CT` | Site B CT | |
| Site B | DX | `SITEB_DX` | Site B X-ray | |
| Site C | CT | `SITEC_CT` | Site C CT | |
| Site C | DX | `SITEC_DX` | Site C X-ray | |

Six rows, and a CT study opened at Site B now goes to `SITEB_CT` and to no
other machine. Substitute your own encounter types and the AE titles the
devices actually use.

### What the screen checks

It **refuses** a row with no Location, no Modality or no AE Title (a row with no
AE title routes nothing — it exists only to be misread later), an AE title
containing a space, and a second row for a location and modality that already
has one.

It **asks** — rather than refuses — when the Location matches no encounter type
or the Modality matches no revenue code, because a site may map a scanner before
its first study goes through it. A typo produces exactly that question, so read
it rather than clicking past it.

Location and Modality **cannot be edited afterwards**: changing either is
indistinguishable from adding the station somewhere else, and would leave the
old pairing routed to nothing. Delete the row and add it again.

## Fill it in before you switch a scanner to station filtering

The order matters, for the reason in the troubleshooting chapter: a scanner that
filters by station sees nothing for any location and modality you have not
mapped.

1. Add a row for every (location, modality) that scanner will ask for.
2. Watch `AutoTasks.out` — `published with NO station` should not name that
   scanner's pairs any more. `AutoTasks.exe --mwl-run` gives the answer in
   seconds rather than at the next cycle.
3. Prove it before real work depends on it:
   ```
   AutoTasks.exe --mwl-selftest c:\Mp10Mwl\worklists CT SITEB_CT
   ```
   That publishes one obviously-fake study booked on `SITEB_CT`. The scanner
   configured as `SITEB_CT` should see it; the others should not.
4. Then switch the scanner to filter by station AE title.

Reverse the order and the symptom is an empty worklist on a machine that was
working ten minutes ago.

## What it does not change

A scanner that filters by **modality and date** is unaffected by any of this.
It sees every study of its modality, mapped or not, exactly as before. You can
run a mixed estate — some devices station-filtered, some not — and the only
rule is the one above: any device that filters by station needs its pairs
mapped.

# Day to day

## Where things are

| | |
|---|---|
| Imaging stations | Admin10 -> Tables -> **Imaging Stations** |
| Worklist files | `C:\Mp10Mwl\worklists\<order number>.wl` |
| AutoTasks log | `AutoTasks.out`, beside `AutoTasks.exe` |
| Orthanc logs | `C:\Program Files\Orthanc Server\Logs` |
| Orthanc configuration | `C:\Program Files\Orthanc Server\Configuration\orthanc.json` and `worklists.json` |
| Health check | `http://127.0.0.1:8042/system` on the server itself |
| What is published right now | `http://127.0.0.1:8042/worklists` — see *Seeing exactly what a scanner would get* |

## Which studies appear

An order is on the worklist when **all** of these are true:

- its encounter is **not** inactive,
- its encounter is **not** closed,
- its start date is within `MWL_WINDOW_DAYS` days either side of today (the
  encounter's admission date is used when the order has no start date of its
  own), and
- its revenue code maps to a modality.

Nothing is ticked per order. An order that meets those tests is on the
worklist; one that stops meeting them comes off it, and its file is deleted
within a couple of minutes.

Whether a given scanner then *sees* it is a second question, answered by
*Sending each scanner its own work*.

## A finished study stays on the list

Nothing tells Mp10 that a scan has been performed — the equipment reports that
to the PACS, not to the worklist — so a study that has already been done
remains on the worklist until its date falls outside the window, usually the
next day. That is normal, and every modality worklist behaves this way;
technologists work from the list and ignore what they have already done.

If it is a nuisance, the answer is a narrower `MWL_WINDOW_DAYS` (`0` = today
only), not a change of procedure.

## Cancelling is not deleting

When a study falls off the worklist its file is removed but its **entry is kept
and marked cancelled**. Re-open the encounter, or move the date back into the
window, and the *same* entry returns with the *same* study identifier — so a
study is never split into two half-studies in your image archive.

Deleting the order itself is the one thing that removes the entry outright,
which is as it should be.

## What a healthy log looks like

In `AutoTasks.out`, every couple of minutes:

```
WriteWorklistFiles: active set size      488
WriteWorklistFiles: inserted 0  re-activated 0  unchanged 488
WriteWorklistFiles: rows with a rev_code but no revcodes.Modality mapped 56  example rev_codes 0320,0350
WriteWorklistFiles: cancelled 0
EmitWorklistFiles: written 0  unchanged 432  purged 0  no modality (not emitted) 56  write failures 0
```

Read it as: 488 orders qualify, nothing changed this cycle, 432 studies are
published to the machines, and 56 are held back for want of a modality —
that last number is the one with work behind it.

`written` is high on the first cycle after a change and `0` the rest of the
time. `write failures` should always be `0`.

Two more lines appear once station routing is in play:

```
EmitWorklistFiles: published with NO station -- invisible to any scanner that
filters by station AE title   135   example location/modality   SiteC/US SiteA/DX
EmitWorklistFiles: station routing is OFF -- no mwlstations, or it cannot be read
```

The first counts studies that no Imaging Stations row matched, and names
examples — those are the rows to add. The second means the table is absent or
unreadable: expected before the 10.72 file check has run, worth investigating
after it.

## Turning it off

Setting `AUTOTASKS / EXPORT_MWL` to `NO` stops the generator — and **leaves the
files it has already written**. Orthanc goes on serving them, so the scanners
keep seeing a worklist that has quietly stopped being true, which is worse than
no worklist at all.

To stop publishing properly:

1. set `EXPORT_MWL` to `NO`,
2. wait one cycle, then **delete the `.wl` files** in the worklist directory.

To stop one *site or modality* rather than everything, tick **Inactive** on its
Imaging Stations row instead; the generator keeps publishing, and those studies
simply stop carrying a station.

## Seeing exactly what a scanner would get

The log says how many studies were published. This says **what is in them**,
decoded, without a scanner or any DICOM tooling. On the server itself:

| | |
|---|---|
| `http://127.0.0.1:8042/worklists` | every published entry |
| `http://127.0.0.1:8042/worklists/<order number>` | just that one, e.g. `.../worklists/OR26-00046110` |

The entry id **is** the Mp10 order number, so a question that starts "is order
OR26-00046110 on the worklist, and does it say the right thing?" is one URL.

```json
{
   "ID" : "OR26-00046110",
   "Tags" : {
      "AccessionNumber" : "EN26-00281843",
      "PatientID" : "00100947",
      "PatientName" : "PEREZ ALVIRA^INGRID",
      "ReferringPhysicianName" : "SALGUEIRO^JESUS M",
      "ScheduledProcedureStepSequence" : [ {
         "Modality" : "CT",
         "ScheduledProcedureStepStartDate" : "20260826",
         "ScheduledStationAETitle" : "SITEB_CT",
         "ScheduledStationName" : "Site B CT"
      } ],
      "StudyInstanceUID" : "2.25.1553995747295557493394282228217242855"
   }
}
```

What to look at, in the order it usually matters:

- **`ScheduledStationAETitle`** — the routing answer. **Absent** means no
  Imaging Stations row matched, so a station-filtering scanner will not see
  this study.
- **`Modality`** — must be exactly what the scanner asks for. A study with no
  modality is never published at all, so if it is here, it has one.
- **`ScheduledProcedureStepStartDate`** — the day the scanner has to ask for.
- **`PatientName`** and **`AccessionNumber`** — what the technologist will see,
  and what ties the images back to the encounter.

Three things to know about this page:

> It is bound to the server's **loopback address**, so it only opens on the
> server itself — not from a workstation, by design. It shows **patient data**,
> so treat the browser window as you would any clinical screen. And it is
> **not** the worklist users are meant to read: that is the Worklist page in
> Mp10 Web.

If it answers **HTTP 400** instead of a list, something in the worklist
directory is not readable as DICOM — see the troubleshooting chapter. Scanners
are unaffected in that state; only this listing breaks.

## Older equipment that "needs its own worklist server"

Some legacy devices — older ultrasound systems in particular — are said to need
a worklist server of their own. Usually they do not. What they have is a fixed
setting you cannot change on the device, and the mismatch shows up at the
console as *cannot connect*, which reads like an incompatibility rather than a
configuration difference.

Three fixed settings account for most of it:

| The device insists on | What to do |
|---|---|
| **A particular Called AE title** it will not let you edit | Install with `-AcceptAnyCalledAet`. The server then answers whatever a device calls it. Verified against devices calling `PHILIPS_MWL`, `WORKLIST` and `HDI5000` |
| **Port 104**, the standard DICOM port | `-DicomPort 104`. Nothing else changes |
| **Querying with almost no filters** — no date, sometimes not even a modality | Already handled. This server answers a query with an empty Scheduled Procedure Step, or none at all, by returning everything |

```
.\Install-Mwl.ps1 -DicomPort 104 -AcceptAnyCalledAet
```

> `-AcceptAnyCalledAet` is a real loosening: any host that can reach the port is
> answered, whatever it calls itself. Pair it with `-Modality`, which restricts
> by source address, and prefer changing the device where the device allows it.

**Before assuming a device cannot do this at all**, two checks settle it in
minutes:

1. **Point it at this server and read the log.** Set Orthanc to verbose
   (`Invoke-RestMethod http://127.0.0.1:8042/tools/log-level -Method Put -Body
   verbose`), have the device query, then read the newest file in
   `C:\Program Files\Orthanc Server\Logs`. It shows the association — what
   the device called itself and called us — and the query it sent. Put the
   level back to `default` afterwards.
2. **Read the vendor's DICOM conformance statement** for that model and
   software version. It states plainly whether *Modality Worklist Information
   Model — FIND* is supported as an SCU, and which keys the device sends. If
   worklist is absent from that document, the device genuinely cannot do it and
   no server-side change will help.

If it turns out the device does not speak worklist at all, the only routes are
a software option or upgrade from the vendor, or the interface box the site is
presumably already running — and that is worth knowing definitely rather than
by inference.

## Adding a scanner

1. On the scanner, configure the worklist source: **called AE title**, **host**
   and **port** exactly as `Test-Mwl.ps1` reports them. Its own AE title can be
   anything, but write it down.
2. If the server was installed with `-Modality`, re-run `Install-Mwl.ps1` with
   the new scanner added to the list — otherwise it will be refused at
   association, which the scanner usually reports as "cannot connect".
3. Use `--mwl-selftest` (above) for the first query rather than a real patient.

# Troubleshooting

Almost every worklist problem is one of two questions, and telling them apart
is most of the work:

> **Is nothing being published, or is nothing getting through?**

`Test-Mwl.ps1` answers the second. `AutoTasks.exe --mwl-run` answers the first.
Run both before changing anything.

## The scanner's worklist is empty

Work down this list; each step rules something out.

| # | Check | How | If it fails |
|---|---|---|---|
| 1 | Are files being written? | Look in the worklist directory | Go to *Nothing is being published* |
| 2 | Do the two halves agree on the directory? | `Test-Mwl.ps1 -ExpectedSpoolPath <the Mp10 setting>` | Fix `MWL_SPOOL_PATH`, or re-run `Install-Mwl.ps1` with `-SpoolPath` |
| 3 | Is Orthanc actually serving? | `Test-Mwl.ps1` — "Worklist plugin is serving *n* entries", or open `http://127.0.0.1:8042/worklists` and read one | Go to *The server is not answering* |
| 4 | Does the scanner reach it? | `--mwl-selftest`, then query from the scanner | Go to *The scanner cannot connect* |
| 5 | Does the scanner filter by something we do not publish? | Ask it to query with no filters, or by date only | See *The scanner filters by station* |

## Nothing is being published

| Symptom in `AutoTasks.out` | Cause |
|---|---|
| No `WriteWorklistFiles` lines at all | `AUTOTASKS / EXPORT_MWL` is not `YES`, or the service was not restarted after it was set |
| `no .wl written -- AUTOTASKS/MWL_SPOOL_PATH is empty` | The path setting is blank |
| `ABORTED -- AUTOTASKS/MWL_SPOOL_PATH does not exist` | The path is wrong, or the directory was deleted. AutoTasks will not create it: a mistyped path that created itself would look like it was working |
| `active set size 0` with orders on today's schedule | The encounters are closed or inactive, or the dates fall outside `MWL_WINDOW_DAYS` |
| `no modality (not emitted)` accounts for everything | The revenue-code mapping is not filled in. Nothing at all can be published until at least one code maps |
| `write failures` is not `0` | Permissions on the worklist directory, or the disk is full |

If the table itself is missing, `GENERAL / MODALITY-WORKLIST` was set to `YES`
after the last file check rather than before. Run the check again.

> **A file check does not complain when a table cannot be created.** If
> `modalitywl` is still missing after a check that appeared to finish normally,
> report it rather than re-running it repeatedly.

## The server is not answering

| Symptom | Cause |
|---|---|
| `Test-Mwl.ps1` says orthanc.json is **not** the Mp10 configuration | Orthanc was upgraded. Re-run `Install-Mwl.ps1` |
| The service is "Running" but the HTTP port does not answer | Orthanc itself died at start-up while its service wrapper stayed up. The reason is in the last lines of the newest file in `C:\Program Files\Orthanc Server\Logs` |
| The log says a setting is *"defined in 2 different configuration files"* | An extra `.json` was added to Orthanc's configuration directory. Orthanc refuses to start rather than guess. Remove it and re-run `Install-Mwl.ps1` |
| `Worklist server is disabled by the configuration file` | `worklists.json` has `Enable: false` — again, usually an upgrade |
| Service start type is not Automatic | It will not come back after a reboot. `Install-Mwl.ps1` sets this |
| `Test-Mwl.ps1` warns **Orthanc cannot list the directory (HTTP 400)** | Something in the worklist directory is not readable as DICOM — usually a zero-length file, or something the generator did not write. **Scanners are unaffected**: a C-FIND skips the bad file and answers normally. Find it and delete it |
| `Test-Mwl.ps1` says *n* files are **not being served** | Same cause, seen from the other side: Orthanc parsed fewer files than are on disk. The warning prints the two commands that turn on verbose logging, which names the file |

## The scanner cannot connect

| Symptom at the scanner | Cause |
|---|---|
| *Called AE Title Not Recognized* | The scanner is calling the wrong AE title. It must match exactly, including case |
| Association rejected, or refused | The server was installed with `-Modality` and this scanner is not on the list, or it is querying from a different IP than the one recorded |
| Times out with no response | Firewall, or the wrong port. `Test-Mwl.ps1` confirms the port is listening and the rule is enabled — from there it is the network between the two |
| Connects, returns nothing, no error | It is reaching the server. This is now a data or matching problem, not a connection one |
| Returns nothing, and the device's clock is wrong | The scanner asks for *its* today. If its date differs from the server's, the dates do not match and nothing comes back. Check the clock on the device before looking anywhere else |

## A station-filtering scanner sees nothing

Some equipment queries by **Scheduled Station AE Title** — "only show me work
booked for *this* machine". That works, and it is how you keep three CTs from
all showing each other's patients (see *Sending each scanner its own work*).
It has one sharp edge:

> **A study with no station is invisible to a scanner that filters by station.**
> Not "shown anyway" — invisible. Measured: 65 X-ray studies scheduled for one
> day answered a modality-and-date query and returned **zero** to an X-ray
> scanner asking by its own AE title, because no station was mapped for that
> location and modality.

So the mapping and the scanner configuration are one job, not two. Either:

- every location and modality that scanner needs has a row in **Imaging
  Stations**, or
- that scanner filters by **modality and date** only.

`AutoTasks.out` counts the gap every cycle:

```
EmitWorklistFiles: published with NO station -- invisible to any scanner that
filters by station AE title   135   example location/modality   SiteC/US SiteA/DX ...
```

Those example pairs are exactly the rows missing from Imaging Stations.

## A study is on the schedule but not in the worklist

In order of likelihood:

1. **Its revenue code has no modality.** By far the most common. The
   `no modality (not emitted)` count in the log is these.
2. **Its encounter is closed or inactive.** Both remove it deliberately.
3. **Its date is outside the window.** `MWL_WINDOW_DAYS` is one day either side of today by default.
4. **The scanner is filtering by a different modality** than the one the
   revenue code maps to — an ultrasound-guided procedure coded as a procedure
   rather than as ultrasound, for instance.

`AutoTasks.exe --mwl-run` settles 1-3 in a few seconds.

## Things that look like faults and are not

| What you see | Why it is correct |
|---|---|
| Files vanish from the worklist directory | AutoTasks owns that directory and sweeps it every cycle. That sweep is what withdraws cancelled studies |
| A file you copied in by hand disappears | Same sweep. Use `--mwl-selftest`, which is meant for this |
| `unchanged` is large and `written` is `0` | Correct on a quiet cycle. Files are rewritten only when something changed |
| Laboratory and office-visit orders are counted as "no modality" | They are not imaging, and belong in that count. It is not a number that must reach zero — only the imaging codes in it matter |
| The worklist directory contains a `.tmp` file for a moment | Files are written under a temporary name and renamed, so a scanner can never read a half-written one |
| Orthanc refuses a C-STORE | Deliberate. This server serves worklists and must never become an image archive |
| The worklist directory was emptied by hand, or restored from backup | Nothing to do. The generator compares what is on disk, not only what it remembers writing, so the files come back on the next cycle |
| A study that has already been scanned is still on the list | Expected — see *A finished study stays on the list* |

# Reference

## Mp10 settings

| Section | Entry | Default | What it does |
|---|---|---|---|
| `GENERAL` | `MODALITY-WORKLIST` | `NO` | Whether the `modalitywl` table exists. Read during a file check |
| `AUTOTASKS` | `EXPORT_MWL` | `NO` | Runs the generator every cycle |
| `AUTOTASKS` | `MWL_SPOOL_PATH` | *(blank)* | The worklist directory. Blank publishes nothing |
| `AUTOTASKS` | `MWL_WINDOW_DAYS` | `1` | Today +/- this many days |
| `AUTOTASKS` | `MWL_UID_ROOT` | *(blank)* | Leave blank unless your organisation owns a registered DICOM root. Never invent one |

## Commands

| Command | What it does |
|---|---|
| `AutoTasks.exe --mwl-run` | One worklist cycle, printed to the screen. Nothing else runs |
| `AutoTasks.exe --mwl-selftest <dir> [modality] [station AE]` | Publishes one fake study, with no database connection. Give it a station AE title to prove a station-filtering scanner |
| `Test-Mwl.ps1` | Reads both halves and reports what is wrong. Changes nothing |
| `Test-Mwl.ps1 -ExpectedSpoolPath <dir>` | Also confirms both halves use the same directory |
| `Install-Mwl.ps1` | Installs or repairs the DICOM server. Safe to re-run, including while it is running |
| `Install-Mwl.ps1 -AcceptAnyCalledAet` | Answers whatever Called AE title a device uses — for legacy equipment whose worklist AE title is fixed |

## What each worklist entry carries

| The machine shows | Mp10 source |
|---|---|
| Patient name | Patient record, as `Family^Given` |
| Patient ID | Medical record number, padded to 8 digits — the same one your imaging system already knows |
| Date of birth, sex | Patient record |
| Accession number | The **encounter** number |
| Requested procedure ID | The **order** number — one study per order |
| Description | The item's description |
| Modality | The revenue code's modality |
| Scheduled date and time | The order's start date, else the encounter's admission date and time |
| Referring physician | The order's requesting physician, else the encounter's doctor |
| Scheduled station AE title | The scanner it is booked on, from Imaging Stations. Absent when that (location, modality) is not mapped |
| Station name and location | The same row's label and the encounter's site, for display |
| Study identifier | Issued once by Mp10 and never changed |

The study identifier is worth understanding: it is issued when the study first
appears on the worklist, stored, and reused for the life of that order. Cancel
and reinstate a study and it keeps the same identifier, which is why images
taken before and after never split into two studies.
