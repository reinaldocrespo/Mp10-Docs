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

**2. Turn the generator on.**

| Section | Entry | Set to |
|---|---|---|
| `AUTOTASKS` | `EXPORT_MWL` | `YES` |
| `AUTOTASKS` | `MWL_SPOOL_PATH` | the worklist directory, e.g. `C:\Mp10Mwl\worklists` |
| `AUTOTASKS` | `MWL_WINDOW_DAYS` | `1` unless you have a reason |

Then **restart the AutoTasks service** — settings are read at start-up.

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
   the encounter's location   (admit type: "San Francisco", "Mariolga" ...)
                +
   the study's modality       (from the revenue code: CT, US, DX ...)
                |
                v
   Tables -> Imaging Stations  ->  AE title the scanner calls itself
```

Nothing is typed at order entry, and adding a scanner is one row in **Admin10 →
Tables → Imaging Stations**, not a code change.

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
   AutoTasks.exe --mwl-selftest c:\Mp10Mwl\worklists CT SFCT01
   ```
   That publishes one obviously-fake study booked on `SFCT01`. The scanner
   configured as `SFCT01` should see it; the others should not.
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
| What is published right now | `http://127.0.0.1:8042/worklists` |

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
| 3 | Is Orthanc actually serving? | `Test-Mwl.ps1` — "Worklist plugin is serving *n* entries" | Go to *The server is not answering* |
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
filters by station AE title   135   example location/modality   Degetau/US San Francisco/DX ...
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
| `Install-Mwl.ps1` | Installs or repairs the DICOM server. Safe to re-run |

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
