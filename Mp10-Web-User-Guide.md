---
title: "Mp10 Web — User Guide"
subtitle: "Finding patients, working encounters, and everything the browser can do"
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
  \newunicodechar{←}{\ensuremath{\leftarrow}}
  \newunicodechar{−}{\ensuremath{-}}
  \newunicodechar{…}{\ldots}
---

# Mp10 Web — User Guide

This guide is for clinic staff using Mp10 Web in a browser. It does not cover
how the system is installed or built — that is the **Mp10 Web — Installation**
guide.

Two companion guides cover things this one only points at: **Mp10 Web —
Printing** for the print helper, and **Mp10 Signature Pads** for the Topaz pad.

## Signing in

Go to the Mp10 Web address your office gives you. Enter your username and
password and select **Sign In**.

What you can see and do depends on your account. If a screen or button
described below is missing, your account doesn't have permission for it —
ask your administrator.

**Changing your own password.** At the bottom of the menu, beside the sign-out
button, is a **key** (🔑). It asks for your current password and the new one
twice. The new password must be at least 8 characters.

Changing it **signs you out of Mp10 Web everywhere**, including the window you
are in — that is deliberate, so a password you have just replaced cannot still
be in use somewhere you have forgotten about. You are taken back to the sign-in
page to sign in with the new one.

Your Mp10 Web password is separate from your Mp10 desktop password. Changing it
here does not change the one you use in the desktop applications.

## Getting around

After signing in you'll see a menu down the left. It only lists the areas you
have permission to use, so two people may see different menus on the same
system. The main areas are **Dashboard**, **Patients**, **Encounters** and
**Modality worklist**.

The menu can be collapsed to icons with the control at its top, to give the
grids more room. Mp10 Web remembers whether you left it collapsed.

### What the grids remember

The three big screens — Patients, Encounters and Modality worklist — all
behave the same way, and both kinds of memory below are per person, so two
people sharing a computer keep their own.

**Column widths and order are remembered for good.** Drag a column edge to
resize it, or drag a heading to move it, and it stays that way the next time
you sign in — on that computer. If you get into a mess, **Reset layout** in
the button bar puts the columns back to how they ship. It only affects
columns; it does not clear your search or filters.

**Filters are remembered until you close your browser.** Search a name on
Patients, go to Encounters, come back — your search and filters are still
there, and the grid is showing what it was showing. Close the browser and
they are gone; this is meant to save you retyping during a shift, not to
follow you into next week.

The **Modality worklist**'s date range follows a rule of its own, because that
box is filled in for you rather than starting empty. **A date range you set
yourself is remembered** like any other filter. One you never touched is not —
it is reset to today each time, so a browser left open overnight opens on
today's work rather than yesterday's. **Clear** puts it back to following the
calendar.

## The Dashboard

After signing in, the Dashboard is what you land on first. It has three cards:

- **Today** — a quick count of today's encounters, split out by site. If
  today shows nothing yet — first thing in the morning, or on a weekend or
  holiday when the office is closed — that's normal, not a fault: the card
  names the day it's showing and also tells you the last day that actually
  had encounters, so you can tell "quiet today" from "something's wrong"
  at a glance.
- **Needs attention** — encounters from the last several days that still
  need a signature. Use it to catch anything that was missed before it gets
  old.
- **System health** — three quick status rows:
  - **Print helper (this workstation)** — whether the print helper program
    is answering on **the computer you're sitting at right now**, not on the
    server. It only knows about itself, so if you check this at the front
    desk and a colleague checks it at their own desk, you can each see a
    different answer — that's expected, not a bug. If it shows a job stuck
    ("stopped responding"), see **When printing does not work**.
  - **HL7 interface** — when the last message came in from the lab/imaging
    interface, and how many are still waiting to be processed. This only
    turns red when messages have gone quiet for longer than expected — a
    large "unprocessed" count on its own is normal for a busy practice and
    does not turn the row red by itself.
  - **Modality worklist** — how many imaging orders are queued. Seeing
    "nothing scheduled" here is a normal, everyday answer, not an error —
    it just means nothing is queued right now.

## Finding a patient

The patient list shows every patient in a scrollable grid, with search,
filtering, sorting, and paging so you can find one quickly among the full
list.

- **Search** — type a last name, first name, Social Security number, or
  record number, and press Enter. Partial names work.
- **Filter** — narrow the list by sex using the box above the grid, and by
  city or date of birth using the small funnel in those column headings. The
  City funnel takes more than one city at once, and has its own **Find…** box
  because the city list is long.
- **Sort** — select a column heading to sort by it; select again to reverse
  the order. Not every heading sorts — **Sex**, **Phone**, **Mobile** and
  **St** have no arrow, because sorting a list this size on those columns
  would be slow enough to be worse than useless.
- **Paging** — use the controls at the bottom to move through pages of
  results. The count beside them is the number of patients your search
  matched, not the number on screen.

**Single-click selects a row; double-click opens it.** Selecting is what
turns on the buttons in the bar at the bottom — most of them act on the row
you have chosen. The patient then opens **in a window on top of the list**,
so closing it puts you back exactly where you were: same search, same page,
same row highlighted.

**On patients who share a surname.** The list is ordered by last name, and
within one surname the order is not alphabetical by first name — with over
two thousand patients called RODRIGUEZ, sorting them all every time you
opened the screen cost more than it was worth. Use the search box, which
looks at first names too, rather than paging through a common surname.

### The Created column

The last column, **Created**, is the date the patient record was first made.
It sorts, so it is the quickest way to see who was registered recently.

## The patient record

A patient's record has four tabs across the top:

- **Demographics** — name, address, contact details, and the HIPAA signature.
- **Additional** — the further identifying details that do not fit on
  Demographics.
- **Insurance** — the patient's insurance plans.
- **Encounters** — the patient's visits.

Select a tab to switch to it.

The record normally opens **as a window on top of the patients list**, and
**Close** returns you to the list untouched. It can also be opened as a
full page of its own — a bookmark or a link someone sent you does that — and
in that case there is a **Back to list** button instead, and the tab you are
on is part of the address, so refreshing or sharing the link reopens on the
same tab.

### Demographics

This tab shows the patient's personal information: name, address, phone,
date of birth, and similar details.

- Select **Edit** to make changes, then **Save**.
- **Name and identity**: last name and first name are on one line, with date
  of birth ("DOB") and Social Security number on the next, and sex and
  marital status below that. The patient ID card photo sits beside these
  fields (see "Capturing images" below).
- **Language is no longer editable** on this screen. It is not shown or
  changed here.
- **Zip code**: if you enter a zip code, the city and state fill in
  automatically — but only if those boxes were empty. If you already typed a
  city or state, autofill will not overwrite what you entered.
- **ID card photo**: you can attach a photo of the patient's ID (see
  "Capturing images" below).
- **Mailing address** sits beside the **HIPAA signature**, with the two
  address lines stacked and the zip, city and state on the row underneath.

### The HIPAA signature

The card to the right of the address shows the signature certifying that this
**person** received the HIPAA notices. It is not the same thing as the
per-visit signature on an encounter — that one is a receipt for a particular
visit, this one is about the patient.

Underneath the signature it tells you **when it was given**. That is the
point of the card: a signature on file is not much use if nobody can say when
it was signed.

Three things you may see:

- a signature and a date — the normal case;
- **"No HIPAA signature on file for this patient"** — nothing has been
  captured;
- a signature and **"Signed on an unrecorded date"** — the signature is real,
  but it predates the system recording signing times. It is not an error and
  there is nothing to fix.

**Capture signature** (or **Re-capture signature**) takes a new one from a
signature pad. It is pad-only by design — a consent cannot be uploaded from a
file, it has to be signed. See **Patient signatures from a signature pad**.

### Insurance

This tab shows a list of the patient's insurance plans, plus a form below
the list with details for whichever plan you select.

- Select **New** to add a plan, fill in the form, and select **Save**.
- Select an existing plan in the list to view or edit it.
- Select **Delete** to remove a plan.
- Select **Cancel** to discard unsaved changes in the form.
- Each plan can have a photo of the insurance card attached (see "Capturing
  images" below).

**Primary, Secondary, and Historical** — every plan is marked as one of
these three. Primary and Secondary are the patient's active coverage;
Historical means the plan is on file but no longer active. There is no
separate "active" checkbox — this Primary/Secondary/Historical setting is
what determines whether a plan counts as active.

**If you see "This record changed since you loaded it. Reload and try
again."** — this means someone else (on the web or on a desktop screen)
saved a change to this same insurance record while you had it open. This is
expected behavior, not an error in the system. **Do not simply try to save
again.** Reload the patient's insurance tab first, so you're looking at the
current version, then make your change again. Saving blindly on top of
someone else's change can create a duplicate record instead of updating the
one you meant to.

### Encounters

This tab lists the patient's visits (encounters), newest first.

- Filter by date range, insurance, or whether the encounter is still open.
- Select **New** to add an encounter, or select an existing one to edit it.
  Either one opens in a dialog on top of the patient record, so you never
  leave the patient's page just to work on an encounter.
- The action bar at the bottom of the grid (see "The grid action bar" below)
  has an **Inactivate / Reactivate** button. Inactivating an encounter does
  not delete or hide it — it stays in the list, shown in grey, and you can
  select it again later and reactivate it.

**Why "newest" means highest encounter number.** Encounter lists are ordered
by encounter number rather than by admission date. A small number of
encounters brought in from older systems years ago carry admission dates that
are plainly wrong — a year like 9201 or 4208 — and ordering by date put those
at the very top, so the first thing you saw was a screen of bad data.
Encounter numbers are issued in order, so they give the same "newest first"
without that. The odd dates are still in those old records; they are simply no
longer what greets you.

You can still sort by **Admitted** by selecting that heading, which is
occasionally what you want — just expect the strange ones at the end.

## The grid action bar

The patients list and both encounters grids (the standalone Encounters list
and the patient's own Encounters tab) each have a row of buttons in the
bottom-left of the grid, next to the paging controls. Most buttons only work
once you've selected a row — select a row first, then choose the action.

**On the Patients list**, the buttons are:

- **New** — start a new patient record. Works without selecting a row.
- **Edit** — open the selected patient's record.
- **Encounters** — jump straight to the selected patient's Encounters tab.
- **Claims** — open a read-only list of the patient's claims.
- **Notes** — open the patient's notes. Unlike Claims, Audit log, and
  Appointments, you can add a new note here (type it in the box and select
  **Add note**) — you cannot edit or delete an existing note.
- **Audit log** — a read-only history of changes made to this patient's
  record, on the web or on the desktop.
- **Appointments** — a read-only list of the patient's appointments.

Each of these opens in its own dialog on top of the patients list, so
searching or paging the list underneath is undisturbed.

**On an Encounters grid** (the standalone list and the patient's Encounters
tab), the buttons are:

- **New** — start a new encounter. Works without selecting a row.
- **Edit** — open the selected encounter.
- **Orders** — record what the patient is in for. See **Orders**.
- **Inactivate / Reactivate** — flips the selected encounter's status. You'll
  be asked to confirm. An inactivated encounter is greyed out in the list,
  not removed, and can be reactivated the same way.
- **Capture signature** — see **Patient signatures from a signature pad**.
- **Print encounter**, **Print label** and **Preview** — see **Printing encounter forms and labels**.

Every one of these opens **in a window on top of the grid**, and the grid is
untouched behind it — same filters, same page, same selected row when you
close. That applies to patients and encounters alike, from either grid.

If you reach an encounter as a full page instead — a bookmark, or a link
somebody sent you — the **Back to list** button returns you to wherever you
came from rather than always to the same place.

**Reset layout**, in the same bar, restores the grid's columns to their
original widths and order. It leaves your search and filters alone.

### Working an encounter

Two fields on the encounter form check what you type against the practice's
own lists rather than accepting anything.

**Doctor.** Type an id, or a surname, or a first name, and a short list of
matches drops down — choose one with the mouse, or the arrow keys and Enter.
The field beside it then shows **who that doctor is**, read-only. With nearly
ten thousand doctors on file, an id on its own tells you very little, and
this is how you confirm you picked the right one. Upper or lower case makes
no difference to the search.

If you type an id that does not exist, the box turns red. Nothing stops you
saving — it is a warning, not a barrier.

**Admitting code.** The same control, against the ICD-10-CM list. Type a code
like `E11` to see the codes under it, or type words like `diabetes` to search
the descriptions. The **Admitting description** beside it is filled in from
whichever code you choose and cannot be typed into — a description that
disagreed with its own code would be worse than none.

**Leaving it blank is perfectly valid**, and normal: this field has not been
used before, so almost every existing encounter has it empty. It is checked
only if you put something in it.

Only codes that can actually be billed are offered. The ICD-10 list contains
group headings and parent codes as well — `E11` is a heading, `E11.00` is a
real code — and offering the headings would invite choosing something that
cannot be billed.

## Orders — what the patient is in for

An encounter can carry one or more **orders**: the items from your practice's
catalogue that the patient is being seen for. You can open Orders from three
places, and all three lead to the same screen:

- the **Orders** button on the patient's Encounters tab,
- the **Orders** button on the standalone Encounters list,
- the **Manage orders** button on the encounter itself.

Orders are only available once an encounter has been saved. On a brand-new
encounter, save it first and the Orders card appears.

The screen has two lists side by side. On the **left** is your catalogue — every
item you can order. On the **right** is what this encounter has already been
ordered. The arrows between them move items across:

- select an item on the left and choose **→** to add it,
- select an order on the right and choose **←** to take it off again.

Nothing is saved until you choose **Save orders**. Until then you are only
building up the list, and **Close** discards the lot. After saving you'll see a
short summary of what happened — for example "Saved: 2 added, 1 removed".

To find something in a long catalogue, type part of the description or the CPT
code in the search box above the left-hand list and select **Search**. Clearing
the box and searching again brings the whole catalogue back.

**An order that has been transcribed cannot be removed.** Once a transcription
has been written against an order, that order is part of the patient's clinical
record, and the system will not let you take it off the encounter. These orders
are shown greyed out and in italics in the right-hand list, and choosing **←**
on one tells you why instead of removing it. If such an order really is wrong,
the transcription has to be dealt with first — the order can then be removed
normally. You can still add and remove other orders on the same encounter as
usual.

If you ever see an order described as **"(catalogue entry missing)"**, the item
it was ordered from has since been deleted from the catalogue. The order is
still real and still on the encounter — it is shown this way deliberately,
rather than hidden, so nothing disappears from the record without you knowing.

## The Modality worklist

**Modality worklist** in the menu shows the imaging orders queued for the
scanners — the same queue the imaging equipment itself reads when it asks
"what am I scanning today". It is a read-only view: nothing here is edited
from the browser.

Each row is one scheduled procedure, with the patient's name, the accession
number, the modality (CR, CT, US and so on), the procedure, and when it is
scheduled for.

- **Search** by patient name or accession number. Case does not matter — type
  `rivera` or `RIVERA`, whichever is quicker.
- **Status** is **Scheduled** by default. **Cancelled** shows what has been
  called off, and **All** shows both.
- **Modality** is a funnel in that column heading, and takes more than one at
  once — CT and MR together, say.
- **Dates** default to today, plus and minus a day, so a procedure scheduled
  late last night or early tomorrow is still in view. **Change them and your
  window is remembered**, like every other filter, until you close the
  browser. Leave them alone and they follow the calendar instead of freezing
  — a browser left open overnight opens on today, not on yesterday. **Clear**
  returns them to following the calendar.
- **Auto-refresh** keeps the list current without you doing anything. It is
  on by default; the tick box turns it off.

Auto-refresh deliberately holds back in two situations, so it cannot pull the
screen out from under you: while you have a row selected, and while the
browser tab is in the background. Select nothing and it resumes.

### When the list is empty

An empty worklist is usually an ordinary answer, and the message says which
kind it is rather than making you guess:

- **nothing scheduled in this window** — the normal quiet case;
- **nothing matches your filters** — you have narrowed it to nothing, so
  widen the dates or clear the search;
- **nothing in the table at all** — this one is worth reporting, because it
  can mean the process that fills the worklist has stopped rather than that
  there is genuinely no work.

The **Mp10 AutoTasks** guide covers what fills this table and how it is
turned on, which is an administrator's job rather than a desk one.

## Capturing images

Both the patient ID card (Demographics tab) and insurance cards (Insurance
tab) let you attach a photo.

- **On a desktop computer**: selecting "Capture / Upload" opens a file
  picker. Choose an image you've already scanned into a folder (the same
  scan-to-folder process your office already uses).
- **On a phone or tablet**: the same button can open your camera directly, so
  you can photograph the card on the spot.
- Accepted formats are JPEG and PNG, up to 4 MB. Large photos are
  automatically shrunk before they're uploaded, so a full-size phone photo is
  fine — you don't need to resize it yourself.
- Select **Replace** to swap out an existing image, or **Delete** to remove
  it.

## Patient signatures from a signature pad

If a **Topaz signature pad** is attached to your computer, you can take the
patient's signature straight from it.

Select the encounter on either Encounters list and choose **Capture signature**.

If your practice has set up a consent text, the window shows it first, one page
at a time — **Continue** stays greyed out until the patient has reached the last
page. If a choice was set up, they pick one of the options offered; it is one
choice, not a tick-list. Then the signing area appears: the patient signs on the
pad and their signature appears on screen as they write. **Clear** starts them
over, **Save signature** stores it.

**If the patient changes their choice after signing, the signature is cleared**
and they are asked to sign again — the signature has to belong to the choice
they actually made. The window says so when it happens.

With no consent text set up, the window goes straight to the signing area.

**The button only works for an encounter that has no signature yet.** When it is
greyed out, hover over it and it will say *"This encounter already has a
signature"*. To replace one that is already there, open the encounter and use
the **Signature** panel on the encounter form.

Two things it deliberately refuses:

- **Saving before anyone has signed.** It says so instead of storing a blank
  image — an encounter that looks signed but is not would be worse than one
  that is plainly unsigned.
- **Working on a Mac, tablet or phone.** The pad software is Windows-only, and
  the window says so rather than appearing to work. Use the **Signature** panel
  on the encounter form instead — that works everywhere.

If it reports that something is missing (`SigWebTablet.js`, or SigWeb not
running), that is a setup task for whoever looks after the computers, not
something you can fix at the desk. Nothing is saved when it happens, so the
encounter simply stays unsigned. The full details are in the **Mp10 Signature
Pads** guide.

## Printing encounter forms and labels

Printing happens on **the printer attached to the computer you are sitting
at**, using the same templates as the desktop application — so a label printed
here and a label printed from Mp10 come out identical.

Select the encounter on either Encounters grid, then:

- **Print encounter** — the encounter form, to this computer's `FORMS`
  printer.
- **Print label** — the encounter label, to this computer's `LABELS` printer.
- **Preview** — exactly what would have been printed, opened as a PDF in a new
  browser tab, with nothing sent to a printer.

The printer is chosen automatically; nothing asks you which one.

**Preview is worth knowing about.** Use it to check a form looks right before
committing a sheet, or when a label came out wrong and you want to see whether
the problem is the data or the printer. It is the same template and the same
data as printing, not an approximation. If nothing opens, your browser has
blocked the new tab — allow pop-ups for this site; the app tells you when that
has happened rather than appearing to do nothing.

**The Print button on the Claims dialog is not finished.** It reports that
claim printing is still to be built. Print claims from the desktop application
in the meantime.

**Designing and editing the report templates stays in the desktop
application**, deliberately: a template is shared by everyone in the practice,
and editing one is a deskbound job.

### When printing does not work

Nothing is ever half-printed: when it fails, nothing was sent, and the message
on screen names the cause. Most of them come down to the printer not being
found by name, or the print helper not running on that computer — both setup
tasks rather than something to fix at the desk.

If a print job stops responding, Mp10 Web refuses further printing from that
computer until the helper is restarted, which is better than queueing work
that is never going to come out. Somebody with administrator access can do
that from **Admin → Print helper** in the menu, without leaving the browser;
that page also lists which printers the helper can actually see, which is what
solves most problems.

**The full account — what each message means, how the helper is installed and
configured, and how to work through a fault — is in the Mp10 Web — Printing
guide.** It is not repeated here, so that there is only ever one version of it
to keep up to date.

## Who made a change

Every change made through Mp10 Web is recorded under the web user's own
operator code, the same way desktop changes are recorded under a desktop
operator code. This means a colleague using the desktop application can see
that a change came from you, from the web app, just like they would for any
other change.
