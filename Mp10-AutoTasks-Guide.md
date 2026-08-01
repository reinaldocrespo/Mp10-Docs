---
title: "Mp10 AutoTasks — Administrator's Guide"
subtitle: "Backups, electronic remittances, and automated reports"
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

Mp10 includes a background service called **AutoTasks**. It runs unattended on
your server and takes care of work that would otherwise have to be done by hand
every day:

- taking a **backup** of your data on a schedule you choose,
- reading **electronic remittances** (payment advices) that you download from
  your insurers, and posting them into Mp10, and
- running **reports** on a schedule and e-mailing them to whoever needs them.

This guide is for the person who administers Mp10 at the practice. It does not
assume you are a programmer. It does assume you can open the **Admin10** module
and that you know where your practice keeps its shared folders.

Anything in this guide that must be done by whoever installed Mp10 (your IT
person or your reseller) is marked **IT task**.

> **A note on wording.** Throughout this guide, *setting* means an entry in the
> Mp10 System Registry — not the Windows registry. They are unrelated. Nothing
> in this guide requires you to open Windows' own registry editor.

# How AutoTasks works

AutoTasks runs as a Windows service on your Mp10 server. Once started it wakes
up at a fixed interval, checks whether there is anything to do, does it, and
goes back to sleep.

| Setting | Section | Default | What it does |
|---|---|---|---|
| `RUN_EVERY_N_SECONDS` | `AUTOTASKS` | `120` | How often the service wakes up, in seconds |

Two consequences worth understanding, because they explain most "why hasn't it
happened yet?" questions:

- **Nothing happens instantly.** If you drop a file into a folder, AutoTasks
  picks it up on its next wake-up — up to two minutes later by default.
- **Scheduled work fires on the first wake-up at or after the time you set**,
  not exactly on the second.

## The log file

AutoTasks writes everything it does to a log file sitting next to the service
program, named after it with a `.out` extension. On a typical install:

```
C:\Mp10\AutoTasks.out
```

This file is the single most useful thing to look at when something has not
happened. It is plain text — open it with Notepad. Every line is stamped with
the date and time.

The log is capped so it cannot fill the disk:

| Setting | Section | Default | What it does |
|---|---|---|---|
| `MAXIMUM_LOG_FILE_SIZE` | `GENERAL` | `8192` | Largest log size in KB before it is trimmed |

# Where settings live: the System Registry

All three capabilities in this guide are switched on and configured in one
place: **Admin10 → Global System Registry**.

![The Global System Registry in Admin10](images/system-registry.png)

Every setting has four parts:

| Column | Meaning |
|---|---|
| **Section** | The group a setting belongs to, e.g. `BACKUPS`, `SCANNING` |
| **Entry** | The name of the setting, e.g. `BACKUP_PATH` |
| **Value** | What you are setting it to |
| **Description** | A free-text note for whoever reads it next |

## Adding or changing a setting

1. Open **Admin10** and go to the **Global System Registry**.
2. To find an existing setting, type into **Filter only Section** (for example
   `BACKUPS`) or use **Incremental Search**.
3. Press **New** to add a setting, or select a row and press **Change** to edit
   one.
4. Fill in **Section**, **Entry** and **Value** in the *Entries* tab at the
   bottom.
5. Press **Save**.

> **Type the Section and Entry names exactly as shown in this guide.** They are
> matched by name. A misspelled entry is not an error — it is simply a setting
> nobody reads, so the feature stays switched off with no warning.

Settings are read when AutoTasks starts. **After changing anything in this
guide, restart the AutoTasks service** so it picks up the new values. *(IT
task.)*

# Backups

AutoTasks can take an **Advantage Database Server hot backup** — a consistent
copy of your live data taken while people are still working in Mp10 — and
optionally compress it into a dated zip file, keeping only the most recent few.

## Turning backups on

Backups run only when **all three** of these are set. Leave any one of them
blank and no backup will ever run.

| Entry | Example | What it means |
|---|---|---|
| `BACKUP_PATH` | `\\adsserver\backup\mp10\` | Where the backup image is written |
| `BACKUP_TIMES` | `02:00:00` | Earliest time of day the backup may start |
| `BACKUP_DAYS` | `2345` | Which days of the week it may run |

All three go in section **`BACKUPS`**.

### BACKUP_PATH — read this carefully

This is the one setting people most often get wrong.

The backup is written by the **Advantage Database Server**, not by Mp10 and not
by your workstation. The path must therefore make sense *to the database server
process*:

- **UNC paths work** — `\\server\share\folder\`
- **Local drive letters on the database server work** — `D:\Backups\`
- **Mapped network drives do NOT work** — `Z:\Backups\` will fail, even if `Z:`
  is perfectly visible from your own PC

The account the Advantage service runs under must have permission to write
there. *(IT task.)*

### BACKUP_DAYS — the day codes

Days are given as a string of digits run together, one digit per allowed day:

| Digit | Day |
|---|---|
| 1 | Sunday |
| 2 | Monday |
| 3 | Tuesday |
| 4 | Wednesday |
| 5 | Thursday |
| 6 | Friday |
| 7 | Saturday |

So:

| Value | Meaning |
|---|---|
| `2345` | Monday through Thursday |
| `234567` | Monday through Saturday |
| `1234567` | Every day |
| `7` | Saturdays only |

### BACKUP_TIMES

A 24-hour time, `HH:MM:SS`. `02:00:00` means "any time from 2 a.m. onward".

Pick a time when the practice is quiet. A hot backup does not lock anybody out,
but it does work the server hard.

**A backup runs at most once per calendar day.** If one has already been taken
today, AutoTasks will not take another, however many times it wakes up.

## Optional: zipping and retention

If you also set a zip path, AutoTasks compresses each backup into a dated
archive and prunes old ones.

| Entry | Example | Default | What it means |
|---|---|---|---|
| `BACKUP_ZIP_PATH` | `D:\Backups\Zips\` | *(blank)* | Where dated zips are written. Blank = do not zip |
| `MAX_BACKUPS_TO_KEEP` | `20` | `20` | How many zips to keep. Oldest beyond this are deleted |
| `FILES_TO_SKIP` | `oldtable.adt;temp.adt` | *(blank)* | Data files to leave out of the backup, separated by semicolons |

Zip files are named by date, so they sort chronologically:

```
Mp10Data-2026-07-31.zip
```

Unlike `BACKUP_PATH`, the zip path is used by AutoTasks itself, so an ordinary
path reachable from the Mp10 server is fine.

> **`MAX_BACKUPS_TO_KEEP` deletes files.** Once the number of zips in
> `BACKUP_ZIP_PATH` exceeds this number, the oldest are erased automatically.
> Keep that folder for backups only, and set the number to match the disk space
> you have.

## Being told it worked

Backups run at night, when nobody is watching. AutoTasks can e-mail a short
report each time one finishes, so you find out it is still working without
having to go and look.

| Entry | Example | Default | What it means |
|---|---|---|---|
| `NOTIFY_EMAIL` | `support@structuredsystems.com` | `support@structuredsystems.com` | Who to notify. Blank = do not send |

You get **two** messages per night:

1. when the database backup finishes, and
2. when the zip archive finishes — this one also lists any older archives that
   were deleted to stay within `MAX_BACKUPS_TO_KEEP`.

The second message is the more useful of the two, because rotation is the only
part of the process that destroys anything, and this is the only record you get
of it.

```
The Mp10 backup archive was created successfully.

Data dictionary : \\adsserver\data\mp10\mp10.add
Archive         : D:\Backups\Zips\Mp10Data-2026-08-01.zip
Size            : 3072.0 MB (3,221,225,472 bytes)
Files archived  : 428
Started         : 2026-08-01 04:37:11
Finished        : 2026-08-01 04:52:44
Duration        : 00:15:33

Retention       : keeping the most recent 30 archives
Removed         : 2 older archive(s)
                  Mp10Data-2026-06-28.zip
                  Mp10Data-2026-06-29.zip
```

### Sending to more than one person

`NOTIFY_EMAIL` takes as many addresses as you like. Separate them with commas
or semicolons — you can mix the two, and spaces around them do not matter:

```
support@structuredsystems.com; manager@yourpractice.com, it@yourpractice.com
```

Anything that is not an address is ignored rather than sent, so one typo in the
list does not stop the others receiving their copy. Look in `AutoTasks.out` for
`Backup notice sent to` to see how many recipients it actually used.

> **These messages tell you a backup *succeeded*. They do not tell you one
> failed.** If a backup fails, or the service is not running at all, you get
> silence in both cases. Treat a night with no message as something to look
> into — the log will say which it was.

Delivery uses the same `eMail` settings as automated reports (see *Making sure
e-mail actually goes out*). If those are not configured, backups still run
normally; only the notification is lost, and the log records that it could not
be sent.

## A working example

| Section | Entry | Value |
|---|---|---|
| `BACKUPS` | `BACKUP_PATH` | `\\adsserver\backup\mp10\` |
| `BACKUPS` | `BACKUP_TIMES` | `02:00:00` |
| `BACKUPS` | `BACKUP_DAYS` | `234567` |
| `BACKUPS` | `BACKUP_ZIP_PATH` | `D:\Backups\Zips\` |
| `BACKUPS` | `MAX_BACKUPS_TO_KEEP` | `30` |
| `BACKUPS` | `NOTIFY_EMAIL` | `support@structuredsystems.com; manager@yourpractice.com` |

This takes a backup Monday through Saturday, any time after 2 a.m., zips it to
a local disk, keeps the last 30 zips, and e-mails two people when it is done.

## Backups are not a disaster recovery plan on their own

A backup that has never been restored is a hope, not a backup. Copy the zips
off-site — or at minimum onto a different physical machine — and test a restore
periodically. AutoTasks writes the file; making sure it survives a fire, a theft
or a ransomware event is a decision only the practice can make.

# Electronic remittances (EOB / ERA PDFs)

Insurers publish payment advices — remittance notices, ERAs, EOBs — to their
provider portals as PDF files. AutoTasks can read those PDFs and post the
payments into Mp10 automatically, so nobody has to key them in.

## What you have to do

1. **Choose a folder** where remittance PDFs will be saved.
2. **Declare that folder** in the System Registry.
3. **Save PDFs into it** as you download them.

That is the whole workflow. Everything after step 3 is automatic.

## Step 1 — Choose the folder

Any folder the Mp10 server can reach. A share on your file server is the usual
choice, so that billing staff can drop files in from their own workstations.

Requirements:

- The AutoTasks service account needs permission to **read, write and delete**
  in it. It does not merely read the PDFs — it moves them when it is done.
- If more than one person will save files there, give them all write access.
- Do not point it at a folder used for anything else. AutoTasks processes and
  then moves *every* PDF it finds there.

## Step 2 — Declare the folder

| Section | Entry | Example value |
|---|---|---|
| `SCANNING` | `EOBIMAGESPATH` | `\\192.168.0.202\data\mp8\data\x12files\scannedEOBs\` |

The screenshot in *Where settings live* shows exactly this entry being edited.

> **Setting this value is the on/off switch.** There is no separate "enable"
> setting. If `EOBIMAGESPATH` has a value, remittance processing is on; if it is
> blank, it is off. Include the trailing backslash.

Restart the AutoTasks service after setting it. *(IT task.)*

## Step 3 — Save your remittance PDFs there

Download the remittance PDF from your insurer's portal as you normally would,
and save it into the folder. Nothing else is required — you do not need to
rename the file or record it anywhere.

Guidelines:

- **One remittance per PDF.** If the portal offers a combined download of many
  checks, prefer separate files.
- **Let the download finish before AutoTasks wakes up.** Saving directly into
  the watched folder over a slow connection can expose a half-written file. If
  that is a risk, save to your desktop and move the finished file in.
- **Do not put anything else in the folder.** Non-remittance PDFs will be read,
  found to contain nothing usable, and left behind.

## What happens next

On its next wake-up, for each PDF in the folder, AutoTasks:

1. **Reads the PDF.** It first tries to read the text directly. If the PDF is a
   scan with no text in it, it falls back to optical character recognition.
2. **Writes a `..._eob.json` file** beside the PDF holding what it found — the
   check number and date, the payee, and every claim and service line.
3. **Checks the remittance belongs to your practice** (see below).
4. **Posts the payments** into the remittances file, ready to be applied.
5. **Moves the PDF** into a `history` sub-folder so it is not processed twice:

```
...\scannedEOBs\history\C6472004.pdf
```

If you want to know what happened to a particular file, search `AutoTasks.out`
for its name.

## If your server serves more than one practice

A single AutoTasks server can serve several practices, each with its own data
dictionary, all watching the same folder.

AutoTasks reads the **payee** — the provider the cheque was made out to — from
each remittance and compares it against the NPIs and provider numbers registered
for the practice it is currently connected to.

- If they **match**, the remittance is posted and the PDF is moved to `history`.
- If they **do not match**, the file is **left exactly where it is**, untouched,
  so the run connected to the practice that owns it can pick it up.

You do not need to sort remittances by practice, and nothing is lost if a file
is downloaded into the wrong practice's queue.

> If a remittance carries no identifiable payee at all, it is treated as yours
> and processed. Most payer layouts do print one; some do not.

## What must be installed on the server

*(IT task — but if remittances are not being read, this is the first thing to
check.)*

| Component | Purpose | Registry setting |
|---|---|---|
| Python 3 | Runs the extractor | `GENERAL` / `PYTHONPATH` — folder holding `python.exe`, with a trailing backslash |
| `extract_eob.py` | The extractor itself | Must sit in the same folder as the AutoTasks program |
| Poppler | Renders scanned PDFs to images | Installed at `C:\Mp10\poppler\Library\bin` |
| Tesseract OCR | Reads text out of scans | Installed at `C:\Program Files\Tesseract-OCR` |

Poppler and Tesseract are needed **only for scanned remittances**. If your
insurers all publish normal text PDFs, remittances will process without them.

A second log, `python_debug.log`, is written next to the AutoTasks program and
holds the extractor's own output. It is the place to look if a specific PDF
fails.

# Automated reports

Any Mp10 report can be scheduled to run by itself and be e-mailed out. You set
this up from the report itself, not from a central screen.

## Setting up a report

Open the report you want as you normally would, set its parameters, then go to
the **Automate Report** tab.

![The Automate Report tab, here on the Unbilled Report](images/automated-reports.png)

### Who receives it

In **Automated Report Users**, press the green **+** and pick a user from the
**RepUser** drop-down. Their e-mail address fills in beside them. Add as many
recipients as you need; the red **−** removes the selected one.

> The address comes from the user's Mp10 record. If it is wrong or missing, fix
> it on the user, not here.

### When it runs

Tick one of the **Frequency** options:

| Option | Runs |
|---|---|
| **Daily** | Every day |
| **Weekly** | Once a week, on the day you select on the right |
| **First day of month** | On the 1st |
| **Last day of month** | On the last day of the month |
| **End of Each Period** | On your billing period's closing day |
| **Suspend** | Not at all — see below |

**Scheduled Date** runs the report once, on that specific date, instead of
repeating.

**End of Each Period** uses the day-of-month set in
`BILLING` / `DAYS_INTO_MONTH_BEFORE_CLOSING`.

### What time it runs

**Scheduled Time** sets the time of day. If you leave it blank the system
default is used:

| Setting | Section | Default |
|---|---|---|
| `AUTO_REPORTS_TIME` | `AUTOTASKS` | `22:00:00` |

> **A report is only queued for a time that has not yet arrived today.** If it
> is 3 p.m. and you schedule a daily report for 2 p.m., it will not run this
> afternoon — it starts tomorrow. When setting up a report you want to see
> today, pick a time that is still ahead of you, or use **Run** to produce it
> immediately.

### What format it arrives in

Tick one or more of **Send as PDF**, **Send as XLS**, **Send as Fp3**, **Send as
HTML**. PDF is the usual choice. XLS is worth ticking when the recipient will
want to sort or total the figures themselves.

**Report Pswrd** puts a password on the attachment. Use it for anything leaving
the practice.

### Naming and saving

Give it a clear **Report Name** — this is what identifies the scheduled job and
what recipients will see. `Daily Unbilled Report` is a better name than
`report1`.

Press **Save Automated Report**. Press **Run** to test it right away without
waiting for the schedule.

## Stopping a report

Tick **Suspend** and save. The job stays configured, with its recipients and
settings, but stops running. Untick it to resume.

This is preferable to deleting a report you may want back.

## Making sure e-mail actually goes out

Scheduled reports are e-mailed through the practice's outgoing mail server. If
that is not configured, reports will run and produce nothing anybody sees.

These live in section **`eMail`** *(IT task)*:

| Entry | Example | What it is |
|---|---|---|
| `SMTP-ServerName` | `smtp.gmail.com` | Outgoing mail server |
| `SMTP-Port` | `465` | Its port |
| `SMTP-User` | `reports@yourpractice.com` | Account used to send |
| `SMTP-Pass` | *(password)* | Its password |
| `Address` | `reports@yourpractice.com` | Address messages come from |
| `SMTP-Auth` | `YES` | Whether the server requires a login |
| `SENDMETHOD` | `CDO` or `SMTP` | How mail is sent |

> **If you use Gmail or Microsoft 365**, an ordinary account password will not
> work. You need an app-specific password, and the account must permit it. This
> is the most common reason scheduled reports stop arriving after an otherwise
> unrelated IT change.

## Keeping a copy on disk

To have every automated report also written to a folder — useful as an audit
trail, or to feed a shared drive:

| Section | Entry | Example |
|---|---|---|
| `AUTOREPORTS` | `COPY_TO_PATH` | `\\fileserver\mp10\reports\` |

Leave it blank to skip.

# Troubleshooting

Start with `AutoTasks.out`. Search it for the date you expected something to
happen. Almost every question below is answered by a line in that file.

## Nothing at all is happening

| Check | How |
|---|---|
| Is the service running? | Windows **Services**, look for the Mp10 AutoTasks service *(IT task)* |
| Is it writing to the log? | Check the timestamp on `AutoTasks.out` — it should update every couple of minutes |
| Were settings changed and the service not restarted? | Restart it; settings are read at start-up |

## The backup never runs

| Symptom | Likely cause |
|---|---|
| Nothing in the log about a backup | One of `BACKUP_PATH`, `BACKUP_TIMES`, `BACKUP_DAYS` is blank — all three are required |
| "Backup started" then an error | The path is not reachable *by the database server*. Check for a mapped drive letter; use UNC instead |
| Runs some days but not others | Check the digits in `BACKUP_DAYS` against the day-code table — 1 is Sunday, not Monday |
| Ran once, never again the same day | Working as designed — one backup per calendar day |
| Zips are disappearing | `MAX_BACKUPS_TO_KEEP` is pruning them. Raise it or move zips off elsewhere |

## No backup e-mail arrived

Check the log first — it distinguishes "the backup did not run" from "it ran but
the e-mail did not go out", and those need different fixes.

| In `AutoTasks.out` | Meaning |
|---|---|
| `Backup notice sent to N recipient(s)` | It was sent. Look in the recipients' spam folder |
| `Backup notice FAILED to send to` | The mail server rejected it — check the `eMail` settings |
| `Backup notice could not be sent (mail error)` | The mail server could not be reached at all |
| `notifications are switched off` | `NOTIFY_EMAIL` is blank |
| `holds no usable address` | Nothing in `NOTIFY_EMAIL` looks like an address — check for a typo |
| Nothing at all about a notice | The backup itself never completed. See *The backup never runs* |

| Symptom | Likely cause |
|---|---|
| Only one message instead of two | The second comes from the zip step. If `BACKUP_ZIP_PATH` is blank there is nothing to archive, and the first message says so |
| Some recipients got it, others did not | An entry in the list is not a valid address and was skipped. The log line gives the count actually used |
| No message at all, on a night you expected one | The backup did not succeed. Silence is not confirmation — check the log |

## Remittance PDFs are not being picked up

| Symptom | Likely cause |
|---|---|
| Files sit in the folder, nothing in the log | `SCANNING` / `EOBIMAGESPATH` is blank or misspelled, or the service was not restarted |
| "belongs to another practice - left in place" | Working as designed — the cheque is payable to a different practice on this server |
| The file was read but no payments appeared | The claims on it could not be matched. The log names each claim it could not resolve |
| Log mentions the extractor failing | Look in `python_debug.log`. Usually Python or the extractor script is missing from the server |
| Only scanned remittances fail | Poppler or Tesseract is not installed |

Once processed, the PDF is in the `history` sub-folder — it has not been lost.

## A scheduled report did not arrive

| Symptom | Likely cause |
|---|---|
| It never ran at all | Check **Suspend** is not ticked; check the frequency matches today |
| Set up today, did not run today | The scheduled time had already passed when you saved it — it starts tomorrow |
| Ran, but nobody received it | The `eMail` settings, most often an expired or non-app-specific password |
| One person did not get it | Their address on their user record, not on the report |
| Arrived empty | The report's own parameters — run it manually with **Run** and see |

## Getting help

When contacting support, having these ready will save a round trip:

- the relevant section of `AutoTasks.out`, covering the time in question
- `python_debug.log`, for remittance problems
- the exact Section and Entry values you have set
- for a remittance problem, the PDF itself

Structured Systems · <reinaldocrespo@structuredsystems.com> · 954-744-0286
