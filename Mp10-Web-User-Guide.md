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

Mp10 Web gains screens as they are built, and this guide is revised with them.
If a screen here is missing from your menu, either your account has no
permission for it or your site has not turned that feature on; if a screen in
your menu is missing here, your site is running a newer version than this
guide. Last revised **September 2026**, covering the Dashboard, Patients,
Encounters, Claims, Remittances, Reports, the Modality worklist, attachments,
signatures and printing.

Three companion guides cover things this one only points at: **Mp10 Web —
Printing** for the print helper, **Mp10 — Signature Helper** for the program
that drives both the pad **and the scanner** at your desk, and **Mp10 Web —
Signature Pads** for the pad itself.

## Signing in

Go to the Mp10 Web address your office gives you. Enter your username and
password and select **Sign in**.

What you can see and do depends on your account. If a screen or button
described below is missing, your account doesn't have permission for it —
ask your administrator.

**Changing your own password.** At the bottom of the menu, beside the sign-out
button, is a **key** (🔑). It asks for your current password and the new one
twice. The new password must be at least 8 characters, and your site may require
more — the box tells you which. It also has to differ from the one you are
replacing, and the two new entries have to match.

Changing it **signs you out of Mp10 Web everywhere**, including the window you
are in — that is deliberate, so a password you have just replaced cannot still
be in use somewhere you have forgotten about. You are taken back to the sign-in
page to sign in with the new one.

Your Mp10 Web password is separate from your Mp10 desktop password. Changing it
here does not change the one you use in the desktop applications.

## Getting around

After signing in you'll see a menu down the left. It only lists the areas you
have permission to use, so two people may see different menus on the same
system. The main areas are **Dashboard**, then **Patients**, **Encounters**,
**Claims**, **Remittances** and **Modality worklist** grouped under a
**Records** heading, and **Reports** under its own. There is also an **Admin**
heading, and **it starts collapsed** — select it to find **Print helper** and
**Signature helper**, which are referred to later in this guide.

**Two entries appear only where the site uses them.** **Modality worklist** is
there when the site runs the imaging worklist; where it was never turned on the
menu entry is absent, because there is nothing behind it. The **HL7 interface**
row on the Dashboard behaves the same way. Neither is a fault or a permission
problem — it is the difference between a feature your site does not use and one
your account may not see.

**Two ways a button can be unavailable, and they mean different things.** A
button your account may not use is not shown at all. A button you may use, but
not on the row you have selected, is shown **greyed** — and hovering it says
why, such as *"Select a row first"* or *"This patient already has a HIPAA
signature"*. So a greyed button is worth hovering before asking anyone.

Permissions are re-read every time the page loads, so if an administrator
grants you something, reloading the page is enough — you do not have to sign
out and back in.

The menu can be collapsed to icons with the control at its top, to give the
grids more room. Mp10 Web remembers whether you left it collapsed.

### What the grids remember

The four big screens — Patients, Encounters, Claims and Modality worklist —
all behave the same way, and both kinds of memory below are per person, so two
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

After signing in, the Dashboard is what you land on first. What you see depends
on what you are allowed to see, so a colleague's may have fewer cards than
yours:

- **Today** — a quick count of today's encounters, split out by site. If
  today shows nothing yet — first thing in the morning, or on a weekend or
  holiday when the office is closed — that's normal, not a fault: the card
  names the day it's showing and also tells you the last day that actually
  had encounters, so you can tell "quiet today" from "something's wrong"
  at a glance.
- **Needs attention** — encounters from the last several days that still
  need a signature. Use it to catch anything that was missed before it gets
  old.
- **System health** — a few quick status rows (the Modality worklist row is
  there only if your account may see the worklist):
  - **Print helper (this workstation)** — whether the print helper program
    is answering on **the computer you're sitting at right now**, not on the
    server. It only knows about itself, so if you check this at the front
    desk and a colleague checks it at their own desk, you can each see a
    different answer — that's expected, not a bug. If it shows a job stuck
    ("stopped responding"), see **When printing does not work**.
  - **Signature helper (this workstation)** — the same thing for the program
    that drives the signature pad at your desk. Also workstation-local, and
    also normal to differ from desk to desk: a computer with no pad attached
    has no reason to be running it.
  - **HL7 interface** — shown only where the site runs an interface. It gives
    when the last message came in from the lab or imaging interface and how
    many are still waiting to be processed. It turns red when the interface
    program itself stops reporting in, and when messages have gone quiet for
    longer than expected. A large "unprocessed" count on its own is normal for
    a busy practice and does not turn the row red by itself.
  - **Modality worklist** — how many imaging orders are queued. Seeing
    "nothing scheduled" here is a normal, everyday answer, not an error —
    it just means nothing is queued right now.
- **Billing** — four counts over the last 90 days: encounters with no claim
  ("unbilled"), then that window's claims that were rejected, paid, and have no
  status yet. The first is a count of **encounters**, the other three of
  **claims**, so an encounter with two claims counts twice in those. Under them
  the card names the oldest of each, which is the one worth chasing.

  **Encounters dated ahead of today are never counted as unbilled.** An
  encounter created for an upcoming appointment carries no charge yet, and the
  window the card counts ends today, so it falls outside. Where such encounters
  exist the card says so on its own line — *"16 upcoming encounters
  pre-created, next 2026-10-09"* — and where there are none the line is absent
  rather than showing a zero.
- **Print helper** and **Signature helper** — one card each, when you have
  permission to see them, giving the fuller version of those two health rows:
  whether the program is answering at this desk, how many jobs it has done and
  how many failed. Both link through to their own page. Like the rows above,
  they describe **this computer** — sitting at another desk shows that desk's
  answer.

## Finding a patient

The patient list shows every patient in a scrollable grid, with search,
filtering, sorting, and paging so you can find one quickly among the full
list.

- **Search** — type a last name, first name, Social Security number, record
  number or e-mail address, and press Enter. Partial names work. There are
  **Search** and **Clear** buttons beside the box if you prefer them.
- **Filter** — narrow the list by sex using the box above the grid, and by
  city or date of birth using the small funnel in those column headings. The
  City funnel takes more than one city at once. Every funnel has a **Find…**
  box and **Apply** / **Clear** buttons; on a long list like City the Find box
  is the quick way in.
- **Sort** — select a column heading to sort by it; select again to reverse
  the order. Not every heading sorts — **Sex**, **Phone**, **Mobile** and
  **St** and **Signature** have no arrow, because sorting a list this size on
  those columns would be slow enough to be worse than useless.
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

- The fields are ready to type in as soon as the record opens — there is no
  Edit button to press first. Select **Save** when you are done.
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

- Select **New** to add an encounter, or select an existing one to edit it.
  Either one opens in a dialog on top of the patient record, so you never
  leave the patient's page just to work on an encounter.
- The action bar at the bottom of the grid (see "The grid action bar" below)
  has an **Active on/off** button. Inactivating an encounter does
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

### The Flags column

Both encounters grids carry a narrow **Flags** column. It shows small marks
telling you, at a glance, what an encounter already has:

| Mark | Means |
|---|---|
| a pen nib | a **signature** is on file for this encounter |
| a lined document | this encounter has **results** |

**An encounter with neither shows nothing at all.** That is deliberate: with no
placeholders to read past, the rows that do carry something are the ones your
eye lands on when you run down the column. An encounter can show both marks,
one, or none.

Hover over a mark and it will tell you what it is, so you do not have to
remember which shape is which.

The column does not sort. Both marks are worked out after the page of
encounters has already been fetched, so a sort arrow there could not do
anything, and one that silently did nothing would be worse than none.

## Claims

**Claims**, under Records between Encounters and the Modality worklist,
lists every claim raised on any patient. It opens showing claims with a
**service date in the last 30 days**; change the From and To dates to look
further back, or press **Today**. **Clear** puts the 30-day window back.

**Finding one claim.** The search box takes a claim number (`CL26-…`), an
encounter number (`EN26-…`) or a patient's record number, and finds it
regardless of the dates. It does not search by name — for that, find the
patient and open their record. If you type something it does not recognise
it says so above the grid rather than showing an empty list.

**Filtering by payer.** The Insurance heading has a small funnel; select it
to tick one or more payers. The heading is highlighted while a filter is
on; Clear removes it.

### The Status column

The first column shows a coloured flag:

| Flag | Means |
|---|---|
| green | paid in full — a payment is on file and the insurance balance is zero |
| yellow | partially paid — a payment is on file and a balance is still owed |
| black | overpaid — the insurance balance is below zero |
| red | rejected — no payment, and the payer's latest answer is a rejection |
| white | no payment yet, but the payer has answered something else |
| blue, orange, pink, purple | no payment, and the payer's latest answer is one of the four statuses your site defines for itself |
| none | no payment and no status yet |

**Money outranks status.** The first three colours are worked out from the
claim's CPT lines and its payments, the same figures the balance columns show,
so a flag and its row always agree. A claim that was paid in part and later
rejected stays yellow, because the balance beside it is real. Only where
nothing has been paid does the payer's latest answer set the colour.

The four site-defined colours are the statuses your practice names itself —
they read with your own wording, which the desktop Admin module sets, so
"Blue" may be "Appealed" at one site and something else at another.

Hover over a flag to read its meaning. **Click a flag** to see the claim's
status history — every answer the payer has sent, newest first. The first
column is **Recorded**: the date *and time* the answer was written down, since
several answers often arrive on one day and the order matters; hover it to see
the payer's own date, which carries no time. Where the payer's file named no
insurance, the claim's own is shown rather than a dash.

The Status heading has a funnel like Insurance's: tick one or more colours
("No flag" is a choice too) and the list narrows to claims in that state
across the whole window, not only the page on screen. Filtering by colour
takes a moment longer than the other filters — about a second for a month
of claims — because each claim's payments are added up to answer it.

**Payer status** shows the newest entry in the claim's status history and,
on hover, the date it was received.

### What you can do with a claim

Select a row and use the action bar: **Open** shows the claim (and lets you
edit it if you may edit encounters), **Preview** shows the CMS-1500 as a
PDF, **Print** sends it to the FORMS printer, and **837** shows the
electronic claim exactly as it was transmitted — one entry per send, newest
first, with the segments the payer received. A claim that has never been
transmitted says so.

**Two boxes fill themselves on a new claim.** Contract starts as the member
number on the patient's insurance card for the payer being billed, and
follows the payer if you change it before saving; on a saved claim it is
left alone. Performing physician is picked from the staff list rather than
typed: the staff set up for the claim's payer come first, then everyone
else, each shown as number, name, NPI and payer.

Opening, printing and raising a claim need the billing permission, which
the desktop Admin module grants to the **BillingUsers** group; a button you
lack the permission for explains that on hover. The Claims page itself
appears only for that group. Admissions staff still see an encounter's
claims list from Encounters, read-only. Raising a new claim is done from
the encounter, not from here — see "Working an encounter". Adjustments (a
non-zero Ins adjust or Pat adjust on a payment line, or a negative amount in
any of the four money columns) additionally need the AdjustmentAuthUsers
group. That group grants only the adjustment permission, so an operator who
posts adjustments needs BillingUsers as well — AdjustmentAuthUsers on its own
does not open the Claims page.

### Payments

Select a claim and press **Payments**. The top grid is every payment posted
to the claim, newest first, with a total under each money column. Below it,
if you are in the BillingUsers group, is a grid to post new payments:

- **One line per CPT.** Pick a CPT from the claim's own lines or type one
  (a payer sometimes pays under a different code; the page warns, it does
  not refuse).
- **Insurance** may be blank — that is a patient payment.
- **Date paid** starts as today and can be changed per line.
- A line with every amount zero must carry a **check #** — it records a
  rejection. A line with no amounts and no check is not saved.
- **Ins adjust / Pat adjust** need a **Reason**, and posting them needs the
  AdjustmentAuthUsers group. Without it the cell is marked and Save stays off.
- A **negative amount in any column** — a reversal of a payment posted in
  error, a refund — is an adjustment too: it needs the same Reason and the
  same AdjustmentAuthUsers group.

The two balance panels show the insurance and patient balances now and what
they would be after Save. Save posts the lines, brings the claim's stored
totals and status in line, and the Claims page behind reloads — the Status
flag may change.

Co-insurance assignment and the complementary claim are still posted on the
desktop.

## Remittances

A remittance is one payer's check and everything it pays: several claims,
each with its own CPT lines. Most arrive as an electronic 835 file that
AutoTasks loads overnight; some are read out of a PDF the payer sent; and one
can be keyed here by hand. This page is where you look at them, correct them,
and post them to the claims.

It needs the billing permission to open, and posting needs one more — see
**Who may do what**, below.

### The list

The top half is one row per remittance, over a date range that works like the
other screens: the last 30 days to begin with, changeable, with **Today** and
**Clear**. The search box finds a **check number**; unlike the Claims search,
it keeps the date range, because check numbers repeat between payers and years.

Two funnels sit in the headings: **Payer** and **Posted**.

The **Posted** column is a dot, so the state is readable at a glance:

| Dot | Means |
|---|---|
| green | every line of the check has been posted |
| red, "Unposted" | none of it has been posted |
| red, "3 unposted" | some of it posted and some did not — the count is how many lines are still out |

The line under the grid gives the window's totals: how many remittances, and
what they paid, adjusted, took as deductible and assigned.

### The check, and its claims

Select a remittance and the strip below fills in with the check's facts —
payer, check number, check date, received, deposit, check total — then **Lines
paid** and **Difference**. The difference is the check total less what the
lines add up to; it is red when it is not zero, which is the number to settle
before posting.

While a remittance is unposted, the received date, deposit date and check total
can be edited in the strip. Once any line is posted the whole check becomes
read-only, and the strip says who posted it.

Under the strip is the check's claims. Each row is one claim: patient, ICN, the
money it was paid, and its flag. Expand a claim to see its CPT lines. The
**Find claim or patient** box filters this tree as you type, by claim number or
patient name, which is how you find one claim on a check that pays four hundred.

The flags here are the payer's own answer for that claim, and the legend above
the tree names them: balanced, rejected, pending, duplicate, claim not found,
reversal, zero paid. **Claim not found** matters most: it means the claim the
payer names is not in this dictionary, so posting will skip it — fix the claim
number first or remove the line.

**Click a claim number** to open that claim's own record, read-only, without
leaving the remittance.

### Correcting a remittance

Everything here is possible only while the remittance is unposted, and only
with the billing permission.

- **Add claim** finds a claim by number, encounter or record number, shows what
  it was billed, and gives you a line per CPT to fill in.
- **A line can be edited where it sits**: click a Paid, Adjusted, Deductible,
  Assigned or Reason cell in the tree and type. The claim's totals and the
  strip's difference follow as you type. **Save** writes them.
- **Delete claim / Delete line** removes what is highlighted, after asking. A
  claim takes its lines with it; the last line of a claim takes the claim; the
  last claim takes the remittance. Only this remittance's rows are removed —
  the claim record itself is never touched.
- **Delete remittance** removes the whole check, and only while nothing on it
  is posted.

**New remittance** keys one from scratch: fill in the payer, check number and
dates, add the claims, and Save.

### Posting

**Post** writes the remittance to the claims: each line becomes a payment, the
claims' totals and status are brought in line, and the lines are marked posted.
It asks first, and the confirmation is worth reading:

- It shows the check total against the lines paid. **If they differ, posting
  needs you to tick "post anyway"** — that is the safety catch for a check that
  was keyed short.
- **Post deductibles** is on by default and matches the desktop.
- It names how many claims will post and how many are **not found**. Those
  stay unposted, which is why the check then shows a red dot with a count
  rather than a green one. Fix them and post again — the second pass only
  touches what is still out.

Posting cannot be undone from the browser, and a posted remittance cannot be
edited or deleted here. A remittance whose claims carry **assigned** amounts
prints a note saying the complementary claims have not been created — that step
is still the desktop's.

### Printing a remittance

**Print** opens the remittance in a new browser tab, laid out for paper: the
check's facts, then each claim with its CPT lines, and **under each line the
payer's own explanation of what it adjusted** — the code and the sentence that
goes with it, which the grid has no room for.

Highlight a claim first and the button reads **Print claim**: the tab then
holds that one claim, and its total line says how many claims are on the whole
check.

The tab has its own **Print** button, which opens the browser's print dialog.
That dialog is also where you save it as a PDF — choose **Save as PDF** as the
destination instead of a printer.

**Page numbers come from the print dialog, not from the page.** Browsers do
not let a page number its own sheets, so "Page 2 of 9" is printed by the
browser itself, along with the date and the document name, when **Headers and
footers** is ticked. It is ticked by default, under **More settings** in
Chrome and Edge. If you turn it off the numbers go with it; every page still
carries the payer and check number across the top, so a loose sheet can be
placed.

A long check paginates properly: the column headings repeat at the top of each
page, a claim's heading never sits alone at the foot of one, and a line is
never separated from the explanation beneath it.

### Who may do what

| To | You need |
|---|---|
| open Remittances and look | the billing permission (**BillingUsers**) |
| key, edit or delete an unposted remittance | the same |
| **post** a remittance | **remits.post** as well — your administrator grants it in the desktop Admin module |
| print | the billing permission |

A button you may not use is not shown at all, so a Remittances page with no
**Post** button means your account does not have that one.

## Reports

**Reports** in the menu opens a page of report cards, grouped Financial,
Productivity and Coding. It needs the reports permission, which the desktop
Admin module grants with the billing group.

Five reports run today:

- **Ageing of Accounts** — what is owed, by how long it has been owed.
- **Payments + Adjustments** — what came in over a period, and what was
  written off.
- **Billing Report** — what was billed over a period.
- **Periodical Summary** — the period's totals in one sheet.
- **Claims Status** — claims by the payer's latest answer.

The rest of the cards are greyed and marked as not built yet. They are named so
you can see what is coming rather than wonder whether you have lost a
permission.

Each report works the same way: choose the parameters on the left, **Run**, and
the result appears with a chart where one helps and a grid under it. **Export**
saves the grid as a spreadsheet; **Print** opens the browser's print dialog,
where the destination list also offers **Save as PDF**. As with the remittance
print above, page numbers come from the dialog's **Headers and footers**, not
from the report.

## The grid action bar

The patients list and both encounters grids (the standalone Encounters list
and the patient's own Encounters tab) each have a row of buttons in the
bottom-left of the grid, next to the paging controls. Most buttons only work
once you've selected a row — select a row first, then choose the action.

**On the Patients list**, the buttons are:

- **New** — start a new patient record. Works without selecting a row.
- **Edit** — open the selected patient's record.
- **Encounters** — open the selected patient's encounters in a window over the
  list, without leaving it.
- **Claims** — open a read-only list of the patient's claims.
- **Notes** — open the patient's notes. Unlike Claims, Audit log, and
  Appointments, you can add a new note here (type it in the box and select
  **Add note**) — you cannot edit or delete an existing note.
- **Audit log** — a read-only history of changes made to this patient's
  record, on the web or on the desktop.
- **Appointments** — a read-only list of the patient's appointments.
- **Attachments** — documents held against the patient: consents, referrals,
  results. Upload one, or scan one on the spot. See **Attachments**.

Each of these opens in its own dialog on top of the patients list, so
searching or paging the list underneath is undisturbed.

**On an Encounters grid** (the standalone list and the patient's Encounters
tab), the buttons are:

- **New** — start a new encounter. Works without selecting a row.
  **Only on the patient's Encounters tab**, not on the standalone Encounters
  list. An encounter belongs to a patient, and the standalone list has no
  patient in hand — so it is created from the patient's own record, where you
  can see whose visit you are creating. The desktop works the same way.
- **Edit** — open the selected encounter.
- **Orders** — record what the patient is in for. See **Orders**.
- **Results** — the results that have come back for this encounter, and
  printing them. See **Results**.
- **Active on/off** — **on the patient's Encounters tab only**, not on the
  standalone encounters list. It flips the selected encounter's status, and
  asks you to confirm. An inactivated encounter is greyed out in the list, not
  removed, and can be reactivated the same way. (The encounter form also has
  **Closed** and **Inactive** checkboxes, which change the same thing without
  asking.)
- **Sign** — see **Patient signatures from a signature pad**.
- **Print Enc**, **Label**, **Preview Enc** and **Preview Lbl** — see
  **Printing encounter forms and labels**.

The button names are short because the bar is narrow: **Enc** is the encounter
form, **Lbl** the label. **Print** sends it to the printer; **Preview** shows
it on screen first.

Every one of these opens **in a window on top of the grid**, and the grid is
untouched behind it — same filters, same page, same selected row when you
close. That applies to patients and encounters alike, from either grid.

If you reach an encounter as a full page instead — a bookmark, or a link
somebody sent you — the **Back to list** button takes you to the encounters
list.

**Reset layout**, on the patients list, the standalone encounters list and the
worklist, restores the grid's columns to their
original widths and order. It leaves your search and filters alone.

### Working an encounter

Two fields on the encounter form check what you type against the practice's
own lists rather than accepting anything.

**Doctor.** Type an id, or a surname, or a first name, and a short list of
matches drops down — choose one with the mouse, or the arrow keys and Enter.
Click the small arrow at the right of the box to browse the whole list
instead of typing.
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

## Results — what came back

Orders are what the patient came in for. **Results** are what came back: the
reports, readings and studies recorded against that encounter.

Select an encounter on either Encounters grid and choose **Results**. A window
opens listing every result on that encounter, newest first, showing the case
type, the version, the ordering and performing doctors, and when it was
created.

Select a result, then:

- **View** — opens it on screen, in a new browser tab.
- **Print** — sends it to the forms printer at the computer you are sitting at.

Nothing here changes a result. This window shows and prints them; results are
still produced and edited in the desktop application.

There is also a **Signed** column. On this practice's data it is blank on every
result, because the signing step it reports has never been used here — it is
shown rather than hidden so that it starts telling you something the day that
changes. An empty Signed column does not mean a result is unfinished.

### Transcribed and scanned results are different, and the list says which

The **Kind** column tells you how a result was produced, and it changes what
you get:

- **Transcribed** — the report was typed. **View** and **Print** lay it out on
  the practice's own report form, with the patient, the encounter and the
  doctors filled in.
- **Scanned** — the report arrived as a scan or an imported file, so the result
  *is* that document. **View** shows you the file itself.

**Most scanned results are TIFF files**, which browsers cannot display in a
tab. When that happens the tab tells you so and offers the file to download
instead, named properly so it opens in your usual image viewer. That is not a
fault — it is the format the document was filed in.

### If a result will not print

- **"No printer whose name contains FORMS…"** — this computer has no printer
  set up for forms. You'll be offered the list of printers to choose from; pick
  the one you want, and you can ask to be remembered so you are not asked
  again.
- **Nothing happens, or the helper is not answering** — the small Mp10 helper
  program that does the printing is not running at your desk. Same program, and
  same fix, as for encounter forms and the signature pad. See **When printing
  does not work**.
- **A message naming your printer** — that text came from the printer's own
  driver. Fix what it names and try again.

Whether an encounter has any results at all is visible without opening this
window: see **The Flags column**.

## The Modality worklist

**Modality worklist** in the menu shows the imaging orders queued for the
scanners — the same queue the imaging equipment itself reads when it asks
"what am I scanning today". It is a read-only view: nothing here is edited
from the browser.

Each row is one scheduled procedure, with the patient's name, the accession
number, the modality (CR, CT, US and so on), the procedure, and when it is
scheduled for.

> **Modality is blank on every row today.** The field it comes from is empty
> in every dictionary so far, so the column shows `—` and its funnel opens on
> *"Nothing to choose from yet."* That is a data question for your
> administrator, not a fault in this screen.

**Source and Station.** Source is the site the encounter belongs to. Station
is the scanner the entry is routed to — the AE title your administrator
mapped for that site and modality under Imaging Stations in the desktop
Admin module. A dash in Station means no mapping exists for that pairing;
the entry is still in the worklist, but a scanner that filters by its own AE
title will not see it. That is the first thing to check when a machine says
its list is empty.

- **Search** by patient name or accession number. Case does not matter — type
  `rivera` or `RIVERA`, whichever is quicker.
- **Status** is **Scheduled** by default. **Cancelled** shows what has been
  called off, and **All** shows both.
- **Modality** and **Source** are funnels in their column headings, and each
  takes more than one at once — CT and MR together, say, or two sites.
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
  there is genuinely no work;
- **the worklist is not enabled in this dictionary** — different from the one
  above, and not something waiting will fix. It needs an administrator.

The **Mp10 AutoTasks** guide covers what fills this table and how it is
turned on, which is an administrator's job rather than a desk one.

## Capturing images

Both the patient ID card (Demographics tab) and insurance cards (Insurance
tab) let you attach a photo. There are three ways to get one in.

- **Scan** — if a scanner is attached to the computer you are sitting at, put
  the card on the glass and select **Scan**. See below; this is the quickest
  of the three and the one to prefer for a card.
- **Capture / Upload**, on a desktop computer, opens a file picker. Choose an
  image you've already scanned into a folder, the same scan-to-folder process
  your office already uses.
- **Capture / Upload**, on a phone or tablet, can open your camera directly,
  so you can photograph the card on the spot.

Accepted formats are JPEG and PNG, up to 4 MB. Large photos are automatically
shrunk before they're uploaded, so a full-size phone photo is fine — you don't
need to resize it yourself. Select **Replace** to swap out an existing image,
or **Delete** to remove it.

### Scanning a card

Select **Scan** and a *Scan a card* window opens. Put the card on the glass,
press **Scan**, and it appears in the window. Press **Attach** to keep it or
**Cancel** to throw it away.

Two things happen to a scanned card that are worth knowing:

- **It is cropped to the card.** You do not need to line it up in a corner or
  trim it afterwards — the whole page is scanned and the card is cut out of it,
  the same as the Mp10 desktop does.
- **It is one page, and it replaces what is there.** These fields hold a single
  image, so scanning a card does the same thing as *Replace*. There is no
  second side.

If the card comes back **uncropped** — the whole sheet of glass, card somewhere
in the middle — the usual reason is the **scanner lid**, left open or too dark
to tell apart from the card. Close it and scan again. It is still a usable
image either way, which is why you get it rather than an error.

**No Scan button, or a greyed-out one**, means the small Mp10 helper program is
not running on your computer, or there is no scanner attached to it. It is the
same program the signature pad uses — see *If it does not start*, below.

## Attachments

Beyond the ID and insurance cards, a patient can hold **attachments** — consent
forms, referrals, results, anything scanned or received as a file. Select the
patient on the Patients list and choose **Attachments** from the action bar.

From there you can **Import file…** for a file you already have, or **Scan…** one on the
spot from a scanner attached to your computer.

### Scanning a document

Select **Scan…** and a window opens listing what your computer can scan with.
Where the scanner offers a choice, you can set:

- **Scanner**, if more than one is attached.
- **Source** — the glass, or the feeder, on a device that has one.
- **Resolution** and **Colour**.

Some scanners insist on asking for those settings themselves. When yours does,
those choices are missing here and the window says so — the scanner's own
window will appear when the scan starts, on your screen.

Then:

1. Put the first page on the scanner and press **Scan**. It appears as a
   thumbnail after a few seconds.
2. To add more pages, put the next one on and press **Scan another page**.
   Repeat for as many as you need.
3. Press **Attach** when you have them all. With more than one page the button
   says so — *Attach 3 pages as a PDF*.

**One page is attached as an image. Several pages become one PDF.** You do not
choose; it follows from how many you scanned.

Underneath the last page is a line reporting what came off the scanner and what
was stored — *"2550×3507 from the scanner (25.6 MB) stored as 1163×1600 JPEG
quality 75 (351 KB)"*. That is not an error. Every scan is shrunk to a sensible
size before it is stored, which is why a 300 DPI colour page does not produce a
25 MB attachment, and it is why choosing a very high resolution mostly costs
you scanning time rather than buying you a bigger file.

### When scanning does not work

- **The Scan… button is greyed out** — the helper program is not running on
  your computer. Same fix as the signature pad; see *If it does not start*.
- **"…can see no scanner"** — the helper is running but nothing is attached, or
  the scanner is off or asleep. Switch it on, check the cable, and press **look
  again**.
- **A message naming your scanner**, after a pause — that text came from the
  scanner's own driver. Paper jam, lid, cable. Fix what it names and scan
  again.
- **"That scan is N MB, over the 4 MB limit"** — too many pages in one
  document. Attach the ones you have and scan the rest as a second attachment,
  or choose a lower resolution and scan again.
- **It is slow** — a page takes several seconds, and your computer's helper does
  one thing at a time. During a scan the signature pad at that same desk will
  wait its turn.

Anything that survives all that is a job for whoever looks after the computers,
and it is covered in the **Mp10 Signature Helper** guide — though **Admin →
Signature helper** in this application answers most of it without leaving the
browser, including which scanners this desk can see — which, despite the
name, is the guide for scanning too. It is the same program.

## Patient signatures from a signature pad

If a **Topaz signature pad** is attached to your computer, you can take the
patient's signature straight from it, for an encounter or for the patient's
record.

**The patient does everything on the pad.** They never look at the browser.
The consent text, the options and the buttons all appear on the pad's own
screen, exactly as they do in the desktop application — so a signature taken
here and one taken from Mp10 are the same thing, recorded the same way.

Where to start it:

- **An encounter** — select it on either Encounters grid and choose **Sign**;
  or open the encounter and use **Capture on pad** in the
  **Signature** panel.
- **The patient's record** — open the patient, and on **Demographics** use
  **Capture signature** (or **Re-capture signature** where one is already on
  file).

What happens then is the same either way. A small window belonging to the
signature helper comes to the front on your computer, and the pad wakes up.
The patient reads the notice on the pad, pages through it if it is long, taps
the option they agree to if options were set up, signs, and presses **OK** on
the pad. **You then confirm it on the signature helper's own window at your
desk** — that small window is where the signature is accepted, and the button
in the browser says so while it waits: *"Signing on the pad — then confirm on
this workstation"*. It is stored at that point. **There is nothing to press in
the browser while this is going on** — it waits, and tells you when it is
done.

If the patient presses **Cancel** on the pad instead, nothing is stored and the
encounter or record simply stays unsigned.

**Re-capturing replaces what was there.** On a patient record that already has
a signature, the button says *Re-capture signature*, and the new one takes the
place of the old, along with the notice and the option that went with it.

### If it does not start

The signature helper is a small program that runs on **your own computer** —
it has to, because the pad is plugged into your desk, not into the server. It
is **not** a Windows service and does not start with Windows; it starts when
you sign in. If it is not running, or the pad is not attached, Mp10 Web says
so and nothing is stored.

**It is the same program that does the scanning**, despite the name. So a desk
where the pad will not wake up is usually a desk where the *Scan* buttons are
greyed out too, and one fix covers both.

Two things worth knowing:

- **It is Windows-only.** From a Mac, tablet or phone there is no pad and no
  helper, and pad capture is not available. The encounter's **Signature** panel
  still lets you attach a signature that arrived some other way, by upload or
  from the camera, and that works everywhere.
- **Check the Dashboard.** The **Signature helper** card says whether the
  program is answering at the desk you are sitting at. If it is not, that is
  the thing to fix first, and it is usually a matter of starting it.

Anything beyond that — installing it, the pad drivers, or a capture that
stopped responding — is in the **Mp10 Signature Helper** guide, and is a job
for whoever looks after the computers rather than something to sort out at the
desk.

## Printing encounter forms and labels

Printing happens on **the printer attached to the computer you are sitting
at**, using the same templates as the desktop application — so a label printed
here and a label printed from Mp10 come out identical.

Select the encounter on either Encounters grid, then:

- **Print Enc** — the encounter form, to this computer's `FORMS`
  printer.
- **Label** — the encounter label, to this computer's `LABELS` printer.
- **Preview** — exactly what would have been printed, opened as a PDF in a new
  browser tab, with nothing sent to a printer.

The printer is normally chosen automatically and nothing asks you which one:
the form printer is the one whose Windows name contains `FORMS`, and the label
printer the one whose name contains `LABELS`.

### If it asks you to choose a printer

Where the printers at a desk are not named that way, printing cannot guess, so
it shows you the printers this computer has and asks which to use. Pick one and
it prints. This is the normal way to work at such a desk — it is not an error,
and nothing has to be set up first.

Two things worth knowing when that window appears:

- **"Always use this printer for forms/labels on this workstation"** stops the
  question coming back. It is a setting for **the computer**, not for you: the
  next person who sits here gets the same printer. Leave it unticked for a
  one-off print at somebody else's desk.
- **If the printer you want is not in the list**, choosing another one will not
  help — the list is every printer the **print helper's own account** can reach, which is
not the same as the printers you see when you are signed in — a printer added
for one user does not appear there. That one needs
  installing, which is a job for whoever looks after the computers. Ticking the
  box is also not how you undo a wrong choice later; ask them to clear it.

Once a printer has been remembered, **Admin → Print helper** shows which one is
being used and says that it was chosen here rather than found by name.

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

### Page numbers, and printing to PDF

Two different printing paths exist in Mp10 Web, and they behave differently.

**Encounter forms, labels, claims and results** are printed by the print helper
at your desk, from the same templates the desktop uses. They are laid out by
Mp10, and anything printed on them — page numbers included — is part of the
template.

**Remittances and reports** are printed by the browser itself, from a tab.
There the **page numbers come from the browser's print dialog**: browsers do
not allow a page to number its own sheets, so "Page 2 of 9" is added by the
browser, along with the date and the document name, when **Headers and
footers** is ticked under **More settings**. It is ticked by default in Chrome
and Edge. The same dialog is how you make a PDF of either: choose **Save as
PDF** as the destination.

### When printing does not work

Nothing is ever half-printed: when it fails, nothing was sent, and the message
on screen names the cause. The commonest one — no printer named for `FORMS` or
`LABELS` — is no longer a dead end: you are offered the list instead, as above.
What is left is usually the print helper not running on that computer, which is
a setup task rather than something to fix at the desk.

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

## Admin → Signature helper

The companion to **Admin → Print helper**, for the program that drives the
signature pad and the scanner. Both live under the **Admin** heading in the
menu, which starts collapsed.

It answers the two questions the greyed **Scan** and **Capture on pad**
buttons raise, without anyone leaving the browser:

- **Is the helper running at this desk?** The top of the page shows its state,
  the build it is running, whether it is a service, which account it runs as,
  when it started, and its port.
- **Will it accept requests from this site?** *Allowed origins* lists the
  addresses it has been told to trust. If the address you use is not in that
  list, every request is refused — and a refused request looks exactly like a
  helper that is not running. This is the first thing to check.
- **What can this desk scan?** The **Scanners** card lists each scanner the
  helper can see, with its resolutions, colour modes, and whether it has a
  document feeder or can scan both sides. **Re-detect** looks again — use it
  after plugging a scanner in, rather than restarting anything.

If the Scanners card is empty, the helper genuinely cannot see a scanner; that
is a driver or cabling question for IT, not something this screen can fix.

Setting the helper up is an IT task, covered in the **Mp10 — Signature
Helper** guide.

## Who made a change

Every change made through Mp10 Web is recorded under the web user's own
operator code, the same way desktop changes are recorded under a desktop
operator code. This means a colleague using the desktop application can see
that a change came from you, from the web app, just like they would for any
other change.
