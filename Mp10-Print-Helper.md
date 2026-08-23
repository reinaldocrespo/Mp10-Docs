---
title: "Mp10 Web — Printing"
subtitle: "Encounter forms and labels from the browser, and the helper behind them"
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
  \newunicodechar{…}{\ldots}
---

# About this guide

Mp10 Web prints encounter forms and labels on the **printer attached to the
workstation you are sitting at**, using the same templates as the desktop
application — so a label printed from the browser and a label printed from
Mp10 come out identical.

It does that through a small companion program, **MpPrintSrv**, installed on
each workstation that prints. This guide has three parts:

- **Using it** — for the person at the front desk. No setup.
- **Installing it** — for whoever looks after the computers. Marked **IT
  task**.
- **When something is wrong** — a fault-finding table, and the one trap that
  catches most installations.

## Why there is a separate program at all

Three reasons, and they are worth knowing because they explain every other
decision in this guide:

1. **The printer is chosen by name from the workstation's own printer list.**
   Label printers sit at the desk, so the printing has to be started at the
   desk. A web server in a rack cannot see them.
2. **The report templates bind to the database in a way only Mp10's own
   toolchain can reproduce.** MpPrintSrv is built from the same code as the
   desktop application, which is what guarantees the output matches.
3. **The templates live in the database**, not in files on disk, so there is
   nothing to copy to each workstation and nothing that can drift out of date.

# Using it

## Printing an encounter form or a label

On either Encounters grid — the standalone **Encounters** list, or a patient's
own **Encounters** tab — select the encounter, then choose:

- **Print Enc** — the encounter form, to the workstation's `FORMS`
  printer.
- **Label** — the encounter label, to the workstation's `LABELS`
  printer.

The printer is chosen automatically. Nothing asks you which one.

## Preview — checking before you spend a label

**Preview** produces exactly what would have been printed, as a PDF in a new
browser tab, without sending anything to a printer.

Use it when you want to check a form looks right, when a label came out wrong
and you want to see whether the problem is the data or the printer, or when
somebody just wants to read what is on the encounter.

It is the same template and the same data as printing — not an approximation.

> If nothing opens, your browser has blocked the new tab. Allow pop-ups for
> this site; Mp10 Web tells you when this has happened rather than appearing
> to do nothing.

## Printing a result

The helper prints results as well as forms and labels — the **Results** window
on an encounter offers **Print** and **View**, and both come through here.

Two things about results differ from forms and labels, and both surprise people:

- **A result always goes to the FORMS printer.** There is no separate result
  printer, and no `RESULTS` name in `Mp10.ini`.
- **Roughly a third of results are stored scans rather than typed reports.**
  Those are sent exactly as they were stored, so a scanned result printed here
  is the original file — and **View** on one opens whatever that file is,
  commonly a TIFF, which some browsers download instead of displaying. That is
  the file, not a fault.

A typed result is rendered from a template chosen by the encounter's type and
revenue code; when no template matches, a general one is used rather than the
job failing.

## Printing claims

The **Print** button on the Claims dialog is wired up but not finished: it
reports that claim printing is still to be built. Print claims from the
desktop application in the meantime.

## What it does not do

**Designing and editing the report templates stays in the desktop
application.** That is deliberate, not an oversight — a template is shared by
everyone, and editing one is a deskbound job. There is no way to reach it from
a browser.

# When something is wrong

Every failure is reported in the page with a message that names the cause.
Nothing is ever printed silently or half-printed.

| What it says | What it means | Who fixes it |
|---|---|---|
| No printer whose name contains **LABELS** *(or FORMS)* is installed on this workstation | Mp10 Web now offers the printer list instead of stopping here — pick one. If the printer you want is **not in that list**, it is missing, or the helper is a service that cannot see it: see *The session 0 trap* | Operator, then **IT** if the printer is absent |
| The print helper is not answering on this workstation | MpPrintSrv is not running here, **or** its allowed-origins setting does not list this site | **IT task** |
| Encounter not found | The encounter number did not match a record | Check the encounter |
| The print helper is restarting itself to clear it, and should accept work again within a minute. Choose a different printer when it does. | A job wedged the helper. **Installed as a service, it now restarts itself** — nobody has to press anything. | Wait about a minute, then print again, choosing a different printer. |
| The print job did not finish within 60 seconds, and will not. End MpPrintSrv.exe on this workstation and start it again. | The same wedge, on a copy that is **not** a service and so cannot restart itself. | End `MpPrintSrv.exe` in Task Manager and start it again. **Admin → Print helper**'s Restart button cannot help this copy** — it only works for a service. |
| Claim printing is not built yet | Expected — see above | — |
| Report design is a desktop function | Expected — see above | — |
| *An update was applied but nothing changed* | The service is still running the old program | **Restart** it, then check the **Build** line |

The message *"The print helper is not answering"* covers two different
problems on purpose: **a browser cannot tell them apart.** When a page is
refused by the helper's origin list, the browser blocks the reply before Mp10
Web can read the explanation, and it looks exactly like nothing listening.
Check both.

**IT: work through *7. Where to look when it misbehaves*.** It is a step ladder
that separates those two, and the four other causes that present the same way,
starting from a single command run at the workstation.

## A printer the helper will refuse

Some printers cannot be used by a **service** at all, and choosing one produces
a refusal rather than a print:

> '`<name>`' cannot be used: it is not on a printer port — it writes to a file,
> and asks where to put it. Nothing can answer that question while the helper
> runs as a Windows service…

That covers Microsoft Print to PDF, Foxit and anything else whose port is
`FILE:` or `PORTPROMPT:`. It is not a fault to fix at the printer — pick a real
printer, or run the helper under a user account (see *The session 0 trap*).

## Admin → Print helper

Users with the `system.printhelper` permission get an **Admin → Print helper**
page. It reports on the helper running on **whichever workstation has the
browser open** — sitting at a different desk shows you that desk's helper.

It shows:

- **State** — whether it is answering, **which build it is running**, whether
  it is installed as a Windows service, which account it runs as, and when it
  started.
- **Jobs** — how many have printed, how many failed, and the last error.
- **Printers this helper can see** — which printer it would pick for `FORMS`
  and for `LABELS`, **which rule chose it**, and the full list. **This is the page that solves most
  printing problems.**
- **Allowed origins** — the addresses it will accept requests from.

And a **Restart** button.

### When to use Restart

Use it when the page reports that a job **stopped responding**. The helper
deliberately refuses further jobs after that rather than queueing them behind
a job that is never going to finish, and a restart is what clears it.

The button only works when MpPrintSrv was installed as a Windows service. A
copy that somebody started by hand can only be stopped, and the page says so
rather than leaving you waiting for a comeback that is not coming.

A restart is usually quick, but if a print job is stuck it can take up to a
minute — Windows has to give up on the stuck job first. **Do not press it
again in the meantime.** The page waits for a genuinely new instance
before it tells you it worked — not merely for something to answer, because
the old copy keeps replying for a moment after being asked to stop.

# Installing it — IT task

## The quick way: the workstation bundle

There is an installer for this. It puts **both** desk helpers in place —
MpPrintSrv for printing and MpSigSrv for the signature pad and scanning — so
the manual steps below are the fallback, and the explanation of what the
installer is doing.

At each desk, from an **elevated** PowerShell:

```powershell
Expand-Archive Mp10Web-Workstation-Install.zip -DestinationPath C:\Mp10Helpers
C:\Mp10Helpers\Install-Workstation.ps1
```

It asks for the Mp10 program folder and the **origin** — the address staff
open Mp10 Web at — and then:

- copies `MpPrintSrv.exe`, `MpSigSrv.exe`, `FrSystH.dll` and `EZTW32.DLL`
  into that folder;
- writes `PORT` and `ORIGIN` under `[PRINT]` and `[SIGN]` in `Mp10.ini`,
  **editing only those lines** — comments, ordering and every other section
  are left byte for byte as they were. The one exception: a file with no
  `[RDD]` section at all gets one added, containing two **commented-out**
  template lines and no live keys, because this installer has no way to know
  a site's dictionary path and must not guess it;
- registers MpPrintSrv as a service (`-SkipService` if you would rather not);
- **prints** the MpSigSrv startup instructions rather than doing it — see
  *The session 0 trap* below for why it cannot be a service, and the
  signature-helper guide for whose profile a Startup shortcut would land in;
- checks both helpers over loopback and reports the origins each accepts.

**Every answer defaults to what the desk already says.** The installer reads
the existing `Mp10.ini` first, so a re-run — or an update after the site
changes address — is a matter of pressing Enter. An unattended run still
needs `-ProgramDir` (the installer refuses to choose a folder to write into
with nobody to confirm it), but a desk that already has an origin on file
needs no `-Origin`. That
matters across twenty desks: re-typing an origin nineteen times is how one of
them ends up subtly different, and a wrong origin fails in the one way that
looks identical to *"the helper is not running"*.

Two things it deliberately does **not** install, because it cannot:

| | Why | What to do instead |
|---|---|---|
| `ace32.dll`, `axcws32.dll` | SAP's 32-bit ACE client. Redistribution is governed by your own ADS licence, and every PC running the Mp10 desktop already has it. | Install the Mp10 desktop applications there. The installer checks for the files and names them if they are absent. |
| `SigPlus.ocx` | Topaz's control is **COM-registered** by Topaz's own SigPlus installer — copying the file would not work. | Run Topaz's SigPlus installer. The installer checks the registration and reports it. |

## What to copy

From the Mp10 installation folder, onto each workstation that prints — **four
files**:

| File | What it is |
|---|---|
| `MpPrintSrv.exe` | the helper itself |
| `FrSystH.dll` | the report engine that renders the templates |
| `ace32.dll` and `axcws32.dll` | the Advantage database client (both files) |
| `Mp10.ini` | settings — copy `Mp10.ini.example` and edit it |

`ace32.dll` is the one that gets forgotten, because on a PC that already runs
the Mp10 desktop applications it is there already and nobody notices. On a
workstation that only ever uses the browser it is **not**, and without it
`MpPrintSrv.exe` will not start at all.

## 1. Settings

Two settings matter, both in `Mp10.ini` next to the program:

```ini
[RDD]
RDD-VERSION=REMOTE
PATH=\\adsserver\adsdata\sfi\

[PRINT]
PORT=6265
ORIGIN=https://mp10web.example.org
```

- **`[RDD] PATH`** — the folder holding `mp.add`, the same value the desktop
  applications use.
- **`[RDD] user` / `[RDD] password`** — optional, and only needed where the
  dictionary requires a login other than the default the helpers use.
- **`[PRINT] ORIGIN`** — **the address staff open Mp10 Web at**, exactly as the
  browser sends it: scheme and host, no trailing slash, and the port too when
  it is not 80 or 443.
- **`[PRINT] FORMS` / `[PRINT] LABELS`** — optional, and normally absent. A
  printer name that overrides the naming convention, written here by the helper
  itself when an operator picks a printer and asks to be remembered. See
  *Name the printers — or let the operator pick*.

`ORIGIN` is not a formality. It is the whole of the access control: a page
served from any other address is stopped by the browser before its request is
ever sent. Get it wrong and **nothing prints**, with the message *"The print
helper is not answering"*. Several may be listed, comma separated, which is
what a site reachable over both `http` and `https` needs.

**`PORT` and `ORIGIN` are read once, when the helper starts.** Editing either
by hand changes nothing until it is restarted — which is the usual reason a
corrected `ORIGIN` still refuses everything. (`FORMS` and `LABELS` are re-read
for every job, so those take effect immediately.)

`PORT` is 6265 on both sides. Changing it here means changing it in Mp10 Web's
build as well, so leave it alone unless something else on the workstation
already wants that port.

### If this workstation also captures signatures

A desk with a signature pad runs a **second** helper, **MpSigSrv**, and the two
share this same `Mp10.ini`. They coexist and do not interact, but three things
are worth knowing before you edit that file:

- **Different sections.** Printing reads `[PRINT]`, signatures read `[SIGN]`.
  Both read the same `[RDD]`. Adding one must not replace the other.
- **Different ports.** 6265 for printing, 6266 for signatures.
- **MpSigSrv is NOT a Windows service and cannot be one.** It drives an ActiveX
  control and opens a window, and a service in session 0 has neither. It runs
  in the signed-in operator's own session, started at logon. Installing it the
  way you install this one will not work.

See the *Mp10 Signature Helper* guide for that half.

## 2. Name the printers — or let the operator pick

The printer is found **by name**. This is the same convention the desktop
application already uses:

- the label printer's Windows name must contain **`LABELS`**
- the form printer's Windows name must contain **`FORMS`**

Case does not matter. `Zebra ZD410 LABELS` and `labels-front-desk` both work.

**Where the printers cannot be renamed, nothing has to be set up here.** The
first time somebody prints, the helper answers that it found no matching
printer, and Mp10 Web shows the list of printers this workstation has and asks
which to use. The job then goes to that printer. If the operator ticks *always
use this printer*, the choice is written to `[PRINT]` in this workstation's
`Mp10.ini` and nobody is asked again:

```
[PRINT]
FORMS=HP LaserJet 4200 (Front Desk)
LABELS=Zebra ZD410
```

Full resolution order, for both `FORMS` and `LABELS`:

1. a printer chosen for that one job, from the picker
2. `[PRINT] FORMS=` / `[PRINT] LABELS=` in `Mp10.ini`
3. a printer whose name contains `FORMS` / `LABELS`
4. nothing — and the operator is offered the list

A name in `Mp10.ini` that is **not installed** is skipped rather than used, and
the next rule applies. That matters when a printer is later renamed or removed:
the job falls back to the convention instead of failing. **Only some of that
reaches the log:** a configured name that *is* installed but cannot be used by
a service is written to `MpPrintSrv.log`; one that is not installed at all is
skipped silently. So a renamed or removed printer leaves no trace there — if a
desk has quietly started printing somewhere unexpected, check the two names in
`Mp10.ini` against the printers that actually exist.

**To undo a remembered choice**, delete the `FORMS=` or `LABELS=` line from
`[PRINT]`. The next print picks the change up — the helper re-reads `Mp10.ini`
for every job, so it does not need restarting. There is deliberately no button
for this in the browser: it is a setting for the whole workstation, not for the
session that made it.

*Admin → Print helper* shows which printer would be used **and which of these
rules chose it**, so a remembered choice and a name match are told apart.

## 3. Install the service

From an **elevated** command prompt, in the folder holding `MpPrintSrv.exe`:

```
MpPrintSrv.exe -i
```

It starts immediately, and again at every boot. The other verbs:

| Command | What it does |
|---|---|
| `MpPrintSrv.exe -i` | Install and start. In `services.msc` it appears as **Structured Systems MpWeb Print Helper**, not as `MpPrintSrv` — that is the service *name*, which is what `sc` and `Get-Service` want. |
| `MpPrintSrv.exe -u` | Stop and remove |
| `MpPrintSrv.exe -start` | Start |
| `MpPrintSrv.exe -stop` | Stop |
| `MpPrintSrv.exe -restart` | Stop and start |
| `MpPrintSrv.exe -status` | Report the service state |
| `MpPrintSrv.exe -listen` | Run in the foreground instead, without installing |

Install also configures Windows to restart the service automatically if it
ever crashes.

> All of these need an elevated prompt. Without one, `-status` reports
> *"unknown (needs an elevated prompt to ask)"* rather than guessing.

## 4. The session 0 trap

**Read this one even if everything appears to work.**

A Windows service runs in what Windows calls *session 0*, and **session 0
cannot see printers that a signed-in user added**. Since MpPrintSrv picks its
printer by name out of that list, the result is:

> No printer whose name contains LABELS is installed on this workstation

which sounds like a naming mistake and is not. The printer is installed. The
service simply cannot see it.

Two fixes, either is fine:

- **Install the printers for the whole machine** rather than for one user; or
- **Run the service as a user** who has them:

  ```
  MpPrintSrv.exe -u
  MpPrintSrv.exe -i -account=DOMAIN\user -svcpwd=secret
  ```

To tell which situation you are in, open **Admin → Print helper** on that
workstation. It names the account the service runs as and lists every printer
that account can see. If the account is `SYSTEM` and the list is missing the
label printer, this is your problem.

## 5. Check it

Without a browser, from the folder holding the program:

```
MpPrintSrv.exe -enc=<an encounter number> -what=label -mode=pdf -out=c:\temp\t.pdf
```

A PDF means the database connection, the template and the data are all
working, and **only the printer is left to sort out**. That single command
separates half the possible faults from the other half.

**It does not test the listener.** That command is a one-shot job and never
opens a port, so a workstation where it succeeds perfectly can still be unable
to answer the browser. Prove the other half separately:

```
curl.exe http://127.0.0.1:6265/status
```

`origins` in the reply must contain the address staff type into the browser.
See *7. Where to look when it misbehaves*.

Then, from a browser **on that workstation**, select an encounter and choose
**Preview**, then **Label**.

## 6. Updating a workstation later

**Stop the service before copying, and start it afterwards.**

Copying a new `MpPrintSrv.exe` over a running service *looks* like it worked —
the copy succeeds, with no error and no warning — but the service goes on
running the **old** program until it is restarted. Measured: with one build
running and a newer one already on disk, the service still reported the older
one, and only a restart changed it.

```
MpPrintSrv.exe -stop
   ... copy the new files ...
MpPrintSrv.exe -start
```

Then open **Admin → Print helper** on that workstation and check the **Build**
line. It reports the build the helper is *actually running*, not the one
sitting in the folder — which is exactly the question worth asking after an
update, and the only way to catch a workstation that was missed.

## 7. Where to look when it misbehaves

`MpPrintSrv.log`, beside the program, records every job and every refusal with
a timestamp — what was printed, which template, and what went wrong. It is the
first thing to read and usually the last thing you need.

When the browser says *"The print helper is not answering"*, the rest of this
section is the order to work in. It is written as a ladder because each step
eliminates a whole class of fault, and because the steps are not
interchangeable — doing them out of order is how an afternoon disappears.

### First, be at the right computer

**MpPrintSrv must run on the PC that has the browser open**, because the page
fetches `http://127.0.0.1:6265` — an address that means *this* computer and
nothing else. It does not matter which machine serves Mp10 Web.

A site with the web server on one machine and the operator on another is the
normal case, and it is easy to end up testing the wrong one:

| Machine | Needs MpPrintSrv? |
|---|---|
| The one serving Mp10 Web | **No** — unless somebody also uses a browser there |
| The one with the browser and the printer | **Yes** |

**Admin → Print helper** reports on whichever desk has the browser open, so
opening that page from your own PC tells you about *your* PC. Sitting at a
different desk shows you that desk's helper. If it says "not answering",
confirm first that you are looking at the machine you think you are.

### Step 1 — ask the helper what it believes

From a command prompt **on the workstation with the browser**:

```
curl.exe http://127.0.0.1:6265/status
```

This one command answers most of the question, and it answers it better than
`Mp10.ini` does. `Mp10.ini` says what the helper *will* read next time it
starts. `/status` says what it is **actually running with right now**, and when
those two disagree, the helper wins.

A healthy reply, wrapped here for reading:

```json
{"ok":true,"app":"MpPrintSrv","build":"2026-08-20 12:05:08",
 "started":"08/22/26 19:25:07","service":"running","account":"SYSTEM",
 "port":6265,"origins":["http://server02:8081"],
 "printers":{...},"printed":12,"failed":0,"wedged":false}
```

Read three things before anything else:

- **`origins`** — must contain the address staff type into the browser. Step 3.
- **`started`** — if `Mp10.ini` was edited after this time, the edit has not
  taken effect. Step 3.
- **`account`**, and `printers.forms` / `printers.labels`. Step 4.

### Step 2 — if `/status` does not answer

Nothing is listening. Find out which of four things it is:

```
sc qc MpPrintSrv
Get-Service MpPrintSrv
```

| What you find | What it means | Fix |
|---|---|---|
| No such service | `-i` never ran, or ran without elevation | Elevated prompt, `MpPrintSrv.exe -i` |
| Service exists, but `BINARY_PATH_NAME` names **a different folder** than the one you installed into | A service was already registered on this desk. `-i` refuses outright when a service by that name exists, and the workstation installer never re-points one — so it is still running the **old** exe, beside the **old** `Mp10.ini` | `-stop` then `-u` using the **old** exe, then `-i` from the new folder |
| Service exists, state **Stopped** | It started and died, or was never started | Read the log, below |
| Service **Running**, still nothing on the port | The bind failed — something else holds 6265 | The log names it |

For a service that dies at startup the log is decisive. A healthy start writes
three lines:

```
Service starting
MpPrintSrv build 2026-08-20 12:05:09
Listening on http://127.0.0.1:6265  for ["http://server02:8081"]
```

**If the third line is absent, it never served.** The commonest reason is the
database: startup opens the dictionary *before* it binds the port, and a failed
connection stops the service there. Check `[RDD] PATH`, and check that
`ace32.dll` and `axcws32.dll` are beside the exe.

> **A successful `-enc` test does not prove the listener works.** That command
> is a one-shot job — it connects, renders, prints and exits, and never opens a
> port at all. It proves the database, the template and the report engine, which
> is exactly why it is worth running (see *5. Check it*). It says nothing
> whatever about whether anything is listening on 6265, and a workstation where
> it succeeds can still be completely unable to serve the browser.

Note also that `-enc` runs as **you**, while the service runs as **LocalSystem**
by default. Same computer, different account, different view of the network and
of the printers.

### Step 3 — if `/status` answers but the browser still says "not answering"

Compare `origins` against the address in the browser's address bar.

This is the most common fault at a new site, and it is disguised: **a browser
cannot tell "nothing is listening" apart from "the listener refused me".** When
the helper refuses an origin, the browser blocks the reply before Mp10 Web can
read the explanation, so both arrive as the same failure. The message in the
page covers both cases on purpose.

Two ways `origins` goes wrong:

**1. The ini was edited and the helper was not restarted.** `PORT` and `ORIGIN`
are read **once, at startup**. Editing them changes nothing until the helper
restarts — and `started` in `/status` tells you exactly when it last read them.
This is the usual reason a *corrected* `ORIGIN` still refuses everything.

```
MpPrintSrv.exe -restart
curl.exe http://127.0.0.1:6265/status
```

`origins` must now show the new value. If it does not, you edited a different
file — go back to `sc qc MpPrintSrv` and check which folder the service
actually runs from.

**2. The value itself is wrong.** It must be exactly what the browser sends:
scheme and host, **no trailing slash and no path**, and the port too when it is
not 80 or 443. `http://server02:8081` is right. `http://server02:8081/mpweb/`
is not, and neither is the helper's own `http://127.0.0.1:6265`.

Capitalisation does **not** matter — the comparison lowercases both sides, so
`http://Server02:8081` in the ini matches a browser sending
`http://server02:8081`. Do not spend time there.

List several, comma separated, for a site reachable more than one way:

```ini
ORIGIN=http://server02:8081,https://mp10.example.org
```

### Step 4 — it answers, the origin is right, and printing still fails

The connection is now proven and only the printer is left. Look at the
`printers` block in `/status`, or the same thing formatted in **Admin → Print
helper**:

```json
"printers":{"account":"SYSTEM","session0":true,
            "all":["Microsoft XPS Document Writer","Microsoft Print to PDF"],
            "forms":"","labels":"","formsWhy":"none","labelsWhy":"none",
            "unusable":[...]}
```

`forms` and `labels` empty with `session0: true` is *The session 0 trap* above —
go back and read it. `unusable` names every printer that was rejected and why;
a printer on a `FILE:` or `PORTPROMPT:` port (Print to PDF, XPS, and most
"virtual" printers) cannot be used by a service at all.

### The whole ladder, in one table

| `/status` | Then | Cause |
|---|---|---|
| No answer | `sc qc` shows another folder | Old service still registered, running the old exe and old ini |
| No answer | Service Stopped, log ends at `Service starting` | Dictionary unreachable, or ACE DLLs missing |
| No answer | No such service | Never installed, or `-i` ran unelevated |
| Answers, wrong `origins` | `started` is **older** than the ini | Ini edited, helper not restarted |
| Answers, wrong `origins` | `started` is **newer** than the ini | Wrong file edited — check the service's folder |
| Answers, right `origins` | `forms` / `labels` empty | Session 0 trap — printers, not networking |

# Notes for the curious

## Is it safe to have a program listening on the workstation?

It listens on `127.0.0.1` only — the loopback address — so nothing else on the
network can reach it, only software already running on that PC. On top of that
it accepts requests only from the web addresses listed in `[PRINT] ORIGIN`;
your browser enforces that before a request is ever sent.

The signature helper, **MpSigSrv**, works the same way on the same
workstations — loopback plus an allowed-origins list. It is a different
program on a different port, not Topaz's SigWeb, which this practice does not
use. See the *Mp10 Signature Helper* guide.

## What happens if a print job hangs

The helper is built so that a stuck job cannot take the rest of it down with
it. Printing runs separately from the part that answers Mp10 Web, so
**Admin → Print helper** keeps working and keeps reporting — which is what
makes the **Restart** button useful at the moment you actually need it.

After sixty seconds a job is declared stuck: whoever was waiting is told, and
further printing is refused with a message naming the time it happened, rather
than silently queueing work that will never come out.

## Does an https site really reach a http://127.0.0.1 helper?

Yes. Browsers treat the loopback address as trustworthy and exempt it from the
usual rule against mixed content. The signature helper depends on the same
behaviour.
