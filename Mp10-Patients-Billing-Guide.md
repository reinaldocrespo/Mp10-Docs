---
title: "Mp10 Patients, Billing and Payments — User's Guide"
subtitle: "From the patient at the front desk to the payment on the claim"
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

This guide follows a visit through Mp10 from start to finish. The patient is
registered, their insurance card is scanned, they sign, an encounter is opened
and what they came for is ordered. The biller turns that encounter into a
claim, codes it and sends it. The payer answers. The answer is posted, and
whatever is still owed is worked until it is paid.

It is written for the people who do that work every day:

- **Front desk and admissions staff** — Part I, *Patients and encounters*.
- **Billers and coders** — Part II, *Claims, coding and collections*.
- **Payment posters** — Part III, *Remittances and payments*.

Each part can be read on its own, but each one hands over to the next, and
the reasons behind many of the steps only make sense further along. The
insurance card you mark as primary at the front desk is the one the claim
bills. The order you enter on the encounter is the line the biller starts
from. The remittance you post whole teaches the system which codes a payer
refuses. It is worth reading the parts you do not do yourself, at least once.

## Which program you use

Mp10 comes in two editions, and the screens differ a little between them.

| Your practice | Patients and encounters | Claims and payments |
|---|---|---|
| **Professional** (physician practice, clinic, imaging centre) | The billing program **PrBill10**, or the appointment calendar **MpXtrmCal10** | **PrBill10** |
| **Institutional** (hospital, facility) | **Patients10** | **MpBill10** |

This guide describes the professional edition first. Where the institutional
edition works differently, a short note headed **Institutional** says how.

Everything here is about the Windows programs. Mp10 Web, the browser version,
has its own guide, the *Mp10 Web — User Guide*.

## Other guides this one points to

Several subjects already have a guide of their own. This one tells you when
you need them rather than repeating them:

| Guide | What it covers |
|---|---|
| *Mp10 Orders — User's Guide* | Setting up the order catalogue, order labels, HL7 orders and results |
| *Mp10 Dashboard — User's Guide* | The dashboard cards, the full list of claim checks and their tags, unbilled encounters |
| *Mp10 Web — Signature Pads* and *Mp10 — Signature Helper* | Installing the signature pad, and the text and options it shows the patient |
| *Mp10 AutoTasks — Administrator's Guide* | Downloading claim responses and remittances, and reading remittance PDFs automatically |

## Who can do what

What you can do depends on the groups your user account belongs to. Your
administrator sets this up.

| Group | What it opens |
|---|---|
| **Admission** | Patients and encounters. In PrBill10 it also shows the *Encounters Table* button on the toolbar |
| **BillingUsers** | Creating and opening claims, from encounters and from the patient record |
| **Supervisors** | Deleting an insurance card, changing an encounter's date after it is saved, moving charges between encounters, adjusting balances in bulk, collecting clearinghouse files by hand |
| **Administrators** | The same billing work as BillingUsers, deleting an unposted remittance, adjusting balances in bulk, collecting clearinghouse files by hand |
| **AdjustmentAuthUsers** | Authorizing adjustments. On its own it opens nothing |

If a button is missing for you, or a screen says **Check user permissions**,
your account is not in the group that screen needs. That is usually correct;
ask your administrator if you think it is not.

## How the screens behave

A few habits are the same on every grid in Mp10:

- **Click a column header to sort by it.** The active header turns red.
- **Type while a grid has focus to jump** to the first row that starts with
  what you typed, in the column the grid is sorted by. What you have typed
  shows in the *Incremental Search* box on the left.
- **F8 (or Ctrl+F) opens the Search Dialog.** It jumps to the first match
  in the sorted column. It does not hide the other rows.
- **The Filter panel on the left hides rows.** It works only while the filter
  is switched on: click the LED or the word *Filter* to turn it on and off.
- **Enter or F7 opens** the highlighted row. **Insert or F5 adds** a new one.
- **Right-click a grid** for printing, *Excel*, *Change Font*, and *Save
  Current Grid State*, which keeps your column widths and order for next time.

# Part I — Patients and encounters {.unnumbered}

Finding and registering the patient, their ID, signature and insurance
cards, opening the encounter, ordering what they came for, and seeing which
visits are still waiting to be billed.

# Finding a patient

Open the patients list with the **Patients Table** button on the toolbar (or
*Tables → Patients*). The *Patients Data Grid* lists every patient, sorted by
name.

There are three ways to find someone:

1. **Type the name.** With the grid sorted by *Name*, type the last name
   followed by the first: `GARCIA MARIA`. The grid jumps as you type. It
   matches from the start of the name, so you do not need to type all of it.
2. **Search (F8).** Sort by the column you know — *MedRec*, *SSN*, *Name*,
   *DOB*, *City*, *Insurance* or *Creation* — then press F8 and type the value.
   For a record number you do not need the leading zeros; the search adds them.
3. **Filter.** In the *Filter* panel on the left, fill in any of *Last*,
   *Name*, *SSN*, *City*, a range of birth dates (*dob from*, *dob thru*) or a
   range of creation dates, and switch the filter on. Last, first name and SSN
   match anywhere in the field, so `ROD` finds RODRIGUEZ and RODNEY alike. The
   number of patients found shows under the search box.

You cannot search by phone number.

Small icons in the grid tell you more about a patient before you open them:

| Icon | Where | Means | Click it to |
|---|---|---|---|
| Paperclip or note | *MedRec* | The patient has attachments or notes | Open the attachments |
| Tag | *Name* | The patient has tags | — |
| 271 (green or red) | *Insurance* | An eligibility answer is on file, accepted or not | Open the eligibility window |

## Possible duplicate patients

Two records for the same person cause trouble for years: the history is
split, claims go out under two record numbers, and payments land on the wrong
one. Mp10 catches most duplicates when the patient is entered (next chapter),
and the dashboard's *Patient records* card lists the ones that slipped
through as **possible duplicate patients**: records with the same last name,
first name, birth date and city. From that list, highlight the record to keep
and press **Merge group...**.

A record number can also be changed or merged by hand with **Chng Record** in
the patients grid's side panel. Mp10 merges two records only when the name,
date of birth and SSN all match; otherwise it refuses.

# Adding and editing a patient

## Opening the patient window

- **New patient:** press Insert (or F5), or click **New Patient** in the side
  panel.
- **Existing patient:** double-click the row, or press Enter.

The patient window has these tabs in PrBill10: **Demographics**,
**Insurance**, **Encounters**, **Claims**, **Letters** and **Pre-Auths**. The
calendar shows only the first three.

At the bottom are **Save** and **Exit**. Save keeps the window open, so you
can carry on with the insurance and the encounter. **Ctrl+S** saves and
closes in one step; **Ctrl+E** exits.

> **Institutional:** Patients10 adds an **Address** tab (postal and physical
> address, employer) and shows the encounters and claims of the patient
> read-only on their tabs.

## The Demographics tab

Type the patient's details: *Last Name*, *First Name*, *SSN*, *DOB*, address,
phones, e-mail, marital information and an emergency contact.

- **Last Name holds both surnames** (`RIVERA LOPEZ`). There is no separate
  second-surname field.
- Names and the SSN are kept in capitals. Mp10 removes anything that is not a
  letter, a space or a hyphen from a name when you save.
- **City and Zip** have a search button. Pick either one and the other two
  (city, state, zip) fill in together.
- Phone numbers start with the practice's area code.
- *Age* is worked out from the date of birth.

Only the last name is required. A future date of birth is refused with
*Invalid DOB*, and an e-mail address that does not look like one asks you to
confirm.

## The record number

The record number (*MedRec*) is given **when you first save** the patient,
not when you open the window. You cannot type it. Once saved, the window's
title shows it.

Several things are unavailable until the patient has been saved once,
because they need the record number: the insurance cards, the HIPAA
signature, the encounters and tags. **Save the patient first, then carry
on.**

## When Mp10 thinks the patient already exists

On a new patient, as you leave the name, SSN or date-of-birth fields, Mp10
looks for someone already on file with the same SSN, or a similar name. If it
finds one, a window shows that patient's name, SSN, date of birth and address,
with two buttons:

- **Jump to Existing Pte** closes the new record and takes you to the one on
  file. Choose this when it is the same person.
- **Continue W/New Pte** carries on with the new record. Choose this only
  when you are sure it is someone else — two relatives with similar names,
  for instance.

Your administrator can turn this check off, but should not.

## Saving and the primary insurance

When you save a patient who has insurance cards, one of them must be marked
**Primary**. If none is, Mp10 refuses with *Can't save: no primary insurance*
and asks you to open the card that should be primary, tick *Primary*, save
it, and save the patient again.

When the patient is saved, the primary card's insurance becomes the
*Insurance* shown for the patient in the grid and on reports.

# The patient's photo ID

The **ID** box on the Demographics tab holds a picture of the patient's
driver's licence or ID card. It is how anyone in the practice, now or later,
can confirm the person in front of them is the patient on file.

| Button | What it does |
|---|---|
| Scanner (*Capture Image From Scanner*) | Scans the card from the desk scanner |
| Database (*Import id card image from file*) | Loads a JPG file instead |
| Magnifier (*Zoom IN*), or click the picture | Shows the picture full size |
| Delete (*Remove id image*) | Removes the picture |

To scan: place the card on the scanner and click the scanner button. Mp10
crops the picture to the card by itself. **The picture is stored when you
save the patient**, so press Save afterwards.

A patient has one ID picture. Scanning again replaces it.

For anything else — the back of the ID, a referral, a signed consent form —
use **Attachments** in the patients grid's side panel. There you can scan,
import JPG or PDF files, scan several pages into one PDF (*Scan to PDFs*), and
give each document a type and a date.

If the scanner does not respond, or the wrong scanner is used, ask your
administrator: the scanner each workstation uses is a setting.

# The HIPAA signature

The patient signs the practice's HIPAA notice once, on the signature pad, and
the signature is kept on their record. The **HIPAA Signature** box on the
Demographics tab shows it, with the date and time it was taken underneath.

## Taking it

1. Save the patient first. The pen button stays disabled on a new, unsaved
   patient.
2. Click the pen, **Pickup Patient Signature using signing pad**.
3. The pad shows the practice's notice, a page at a time. The patient taps the
   arrow on the pad to move to the next page. While they read, your screen
   shows *Wait for user input* with a Cancel button.
4. The patient signs and taps **OK** on the pad (or **CLEAR** to start
   again).
5. Your screen shows the signature in the *Signature Dialog*. Click
   **Accept** to keep it, or **Cancel** to throw it away.

Mp10 stores the signature, the time, who took it, and a fingerprint of the
exact text the patient was shown. If the notice is reworded later, you can
still tell which wording each patient signed.

**Taking a signature again replaces the old one, without asking.** Only press
the pen when a new signature is wanted.

The printer button beside it, **Print HIPAA documents with patient signature
printed**, prints the practice's HIPAA document with the patient's signature
on it.

## The text on the pad

What the pad shows is set once by the administrator, in the system registry
under **TOPAZ / Patient-Record-DisplayMessage**. It can be plain text, or a
more complete setting that also chooses the font, the pad model and a list of
options for the patient to tap. The format is described in the *Mp10 Web —
Signature Pads* guide, chapter *Using it*, and in the *Mp10 — Signature
Helper* guide. Those guides describe the setting for the encounter signature;
the HIPAA setting takes exactly the same format.

The dashboard's *Patient records* card counts the patients seen in the last
90 days who have no HIPAA signature, so the ones missed are easy to find.

## If the pad does not respond

*Topaz signature device could not be activated* or *TOPAZ signature pad is
not found or missing required components* means this workstation's pad is
not connected or its software is not installed. Check the cable first; the
installation is covered in the *Mp10 Web — Signature Pads* guide, chapter
*Installing it — IT task*.

# Insurance cards

The **Insurance** tab of the patient window lists every insurance card the
patient has ever presented, newest and oldest together. Each row is one card.
Below the list are the card's details, and on the right, the picture of the
card.

## Why the card history matters

Patients change insurance. When they do, **do not overwrite the old card with
the new one.** Add the new card as a new row, make it primary, and leave the
old one on file. The old card is the record of who was billed for the earlier
visits: when a payer asks about a claim from two years ago, or a payment
arrives late for an old plan, the card that was in force then is still there,
with its picture.

## The fields

| Field | What to enter |
|---|---|
| *Member Number* | The member or subscriber ID, exactly as on the card |
| *Group* | The group number. **Fill it in whenever the card shows one** — see below |
| *Insurance Co.* | The payer. Use the search button to pick from the payers your practice has set up |
| *Effective Date*, *Exp Date* | When the coverage starts and ends |
| *Insured First, Last Names* | The subscriber — the person who holds the policy |
| *Relationship* | The patient's relationship to the subscriber: Self, Spouse, Child, Other… Choosing *Self* fills in the subscriber's name from the patient |
| *DOB* | The subscriber's date of birth, as the payer has it |
| *Name as on card*, *Last as on card* | The name printed on the card, if it differs |
| *Coverage Codes*, *Ded Amt* | Deductible information |
| *Primary*, *Secondary* | Which payer is billed first and second |

For Medicare, Mp10 checks that the member number looks like a Medicare
Beneficiary Identifier and asks you to confirm if it does not.

The *Group* matters because when Mp10 writes the electronic claim it chooses,
among the patient's cards for that payer, the one that has a group number.
A new card left without a group can mean the claim goes out with the old
card's details.

## Adding a new card

1. Save the patient if it is new — the Insurance tab works only on a saved
   patient.
2. Click **New**.
3. Fill in the fields.
4. Scan the card (next section).
5. Tick **Primary** if this is now the payer to bill first, or **Secondary**,
   or neither.
6. Click **Save**.

To change a card, highlight it, click **Modify**, make the change and click
**Save**. **Cancel** throws your changes away. Double-clicking a row is the
same as Modify.

## Making the newest card primary, and keeping the old one

When you save a card marked **Primary**, Mp10 takes the Primary mark off
whichever card had it before. That card stays on file, unmarked, as history,
with its picture. The same happens with **Secondary**. There is always at
most one primary and one secondary card.

So, when a patient brings a new card:

1. Click **New** and enter the new card.
2. Tick **Primary**.
3. Save.

The old card is now history and the new one is billed. If you know the old
coverage ended, open the old card with Modify and enter its *Exp Date*. Mp10
does not set it for you.

What uses which card:

- **A new encounter** takes the primary card's payer.
- **A new claim** takes the primary card's payer and member number, and the
  secondary card's payer as its complementary insurance.
- **An eligibility check** asks the primary payer.
- **Existing encounters and claims keep the payer they were created with.**
  Changing the primary card later does not change them.

Only **Supervisors** can delete a card. That is deliberate: a card that is
no longer valid should be left as history, not deleted.

## The picture of the card

Each card row keeps one picture of the card. While you are adding or
modifying a card:

- **Scan** captures it from the desk scanner. Mp10 crops to the card.
- **Import card image from file** loads a JPG or PDF file.
- **Delete** removes the picture.

The picture is stored when you save the card. To see it later, highlight the
card in the list: its picture shows on the right. **Preview**, or a click on
the picture, shows it full size.

A card row holds one picture, normally the front. Keep the back of the card,
when you need it, in the patient's **Attachments**.

The **OCR** button does not read the card for you; enter the details by hand.

## Checking eligibility

Before the visit, ask the payer whether the patient is covered. In the
patients grid, highlight the patient and click **Eligibility** in the side
panel, or click the 271 icon in the *Insurance* column.

The *Insurance Eligibility* window lists the answers already received. Click
**Fetch New Eligibility record.** to ask the primary payer now. Mp10 connects
to the clearinghouse (*Connecting with clearing house. Stand by*) and adds
the answer to the list.

If the payer's answer differs from what you have on file — a different
spelling, a different date of birth — **Fix patient record with data from
Eligibility.** shows the patient record as the payer has it, with the
differences highlighted. **Accept** saves those changes; **Cancel** leaves the
record alone.

Your administrator can also have Mp10 ask automatically each time the
primary member number changes.

# Encounters

An **encounter** is one visit: one patient, one date, one payer. Everything
that happens during the visit hangs from it — what was ordered, the
patient's signature, the questionnaire, and in the end, the claim. Mp10
numbers encounters `EN` followed by the year and a sequence, such as
`EN26-00012345`.

## Creating an encounter

1. Open the patient (and save it, if it is new).
2. Go to the **Encounters** tab.
3. Click **New** (*Insert New Case*), or press Insert.

The encounter window opens over the patient. Its tabs are **Encounter Data**,
**Remarks**, **Orders** and **Questionnaires**.

On **Encounter Data**:

| Field | What to enter |
|---|---|
| *Encounter Number* | Filled in by Mp10 when you first save |
| Encounter type (the list at the top) | The kind of visit, from your practice's list |
| *From* | The date of the visit. **Tick the box and pick the date** — a new encounter starts with no date |
| *Thru* | The last day, for a visit that spans more than one |
| *Referring Phys* | The referring doctor. Use the search button; only active doctors are offered |
| *Insurance* | The payer for this visit. It starts as the patient's primary card |
| *Inactive encounter* | Tick it for a no-show (below) |
| Accident | *Is Auto Accident*, *Work related Accident* or *Is Other Accident*, and the accident date, when the visit is the result of one |

The referring doctor here becomes the referring doctor on the claim, and the
accident boxes are sent on the electronic claim, so fill them in carefully.

Click **Save**. Mp10:

- refuses if there is no date (*Encounter date cannot be blank*);
- warns if the patient already has an encounter on the same date, and asks
  whether to continue — answer **No** if this is a duplicate;
- gives the encounter its number.

The window stays open after Save, so you can go on to the orders.
**Exit** closes it.

If Save says *This encounter is open somewhere else*, another user has the
same encounter open. Ask them to close it, then press Save again; nothing was
lost.

From the patient's Encounters tab you can also print the encounter form
(**Print Case sheet**) and a label (**Print Case Label**).

> **Institutional:** in Patients10 the encounter window has the tabs
> **Encounter**, **Other Information**, **DRG/APC** and **Remarks**. The
> encounter type is required, and you can enter the pre-authorization, admitting diagnosis, department, source,
> weight and height. The encounter window prints its own form and label.
> Orders are entered from the Encounters grid, not from a tab (next chapter).

## Encounters made from the calendar

In the appointment calendar (MpXtrmCal10), an appointment for someone who is
not yet a patient has a **Create New Patient Record** button. It opens a new
patient window filled in from the questionnaire the patient answered on-line,
and when you save the patient, the appointment is linked to the new record.

The calendar does not create encounters. When the patient arrives, open
them (the **...** button beside *Medical Record* on the appointment) and
create the encounter on their Encounters tab. Once the encounter exists, the
calendar shows the appointment as arrived.

## Encounters dated in the future

You can create an encounter ahead of time, for a scheduled visit. The Billing
dashboard shows these as *Upcoming encounters pre-created* and does not count
them as unbilled.

## No-shows: inactivating an encounter

When the patient does not come, do not leave the encounter as it is: it would
sit on the unbilled list for ever. Open it, tick **Inactive encounter** and
save. Inactive encounters are shown in grey and left out of the unbilled
lists and the dashboard counts.

## The Questionnaires tab

On-line questionnaires the patient filled in before the visit are linked to
the encounter when you save it, by the patient's record number or cell phone
and the appointment date. The tab lists them; **Open** shows one. You can
also send the patient a text message asking them to fill one in.

# Orders: what the patient came for

An **order** records what the patient is in for — the study, test or
procedure — picked from your practice's catalogue. The *Mp10 Orders* guide
explains orders in full: setting up the catalogue, labels, sending orders by
HL7, results coming back. This chapter covers entering them on the encounter
and what they do for billing.

## Why the front desk should enter them

Orders are how the front desk tells the biller what happened. When the
biller creates the claim for an encounter that has orders, **the claim's
procedure lines start from them**: one line per procedure code, dated on the
day of the visit. The biller confirms them instead of reconstructing the
visit from paperwork.

This applies to every kind of visit, not only to studies. If your catalogue
has an item for a follow-up visit, an injection or a consultation, ordering it
puts its code on the claim. Ask your administrator to add an item to the
catalogue for every service the practice bills.

Orders also fill the **Ordered** column of the unbilled encounters list, so
the billing backlog reads as work — "MRI KNEE RT, FOLLOW-UP" — not as a list
of names.

## Entering orders on the encounter

1. Save the encounter once — the Orders tab works only on a saved encounter.
2. Go to the **Orders** tab.
3. Click **New Order** (or press Insert).
4. In the *Item* cell, type the item code, or click the cell's button to
   pick from the catalogue (the *Pick* list shows each item's description and
   body part).
5. Add as many orders as the visit needs.
6. Click **Save**.

**Delete Order** removes the highlighted order when you next save.
**Print Label** prints a label for a saved order.

When you save, Mp10 warns if the encounter now has the same study ordered
twice, or if the payer has already refused this item for this patient. The
orders are saved either way; the warning is for you to act on. The *Mp10
Orders* guide explains both warnings.

The patient's Encounters tab, and the Encounters and Unbilled grids, also
have an **Orders** link that opens a separate *Orders for* window over the
same orders. It appears only when your practice has orders switched on.

> **Institutional:** in Patients10, open the **Encounters Table**, highlight
> the encounter and click **Orders** in the side panel. On institutional
> claims the procedure lines come from the charges posted to the encounter,
> not from the orders.

# The patient's signature on the encounter

Besides the HIPAA signature, which is taken once, many practices have the
patient sign at every visit: a consent, a financial agreement, an
acknowledgement. That signature belongs to the encounter.

## Taking it

1. Open the patient and go to the **Encounters** tab.
2. Highlight the encounter.
3. Click the pen, **Pickup Patient Signature using signing pad**.
4. The patient reads the text on the pad, page by page, taps an option if
   one is offered, signs and taps **OK**.
5. Click **Accept** on your screen.

The signature shows in the *Signature* column of the encounter list. As with
the HIPAA signature, Mp10 keeps who took it, when, the option the patient
picked, and the exact text shown. **Signing again replaces the earlier
signature.**

If the patient signed the on-line questionnaire before the visit, that
signature is copied onto the encounter when you save it, unless the
encounter already has one.

## Showing your own text on the pad

What the patient reads before signing is the practice's own text, set once
by the administrator in the system registry under **TOPAZ /
Encounter-Signature-DisplayMessage**. It can be:

- **plain text** — the pad shows it as it is, wrapped and paged;
- **the path of a text file** — the file is read each time, so the text can be
  changed without touching the registry;
- **a JSON setting** — which also sets the pad model, the font and size, a
  list of options for the patient to choose from (for example *I agree* /
  *I do not agree*), and whether the options and the signature start on a
  page of their own.

The text can include the patient's e-mail address with `^email`, so the
patient can confirm it while signing.

The *Mp10 Web — Signature Pads* guide, chapter *Using it*, gives the format
with examples, and the *Mp10 — Signature Helper* guide covers every option. The
same setting is used by the Windows programs and by Mp10 Web.

## Finding encounters with no signature

The dashboard's *Encounters missing something* card counts recent encounters
with no patient signature. The Billing dashboard's *Needs attention* card
shows unsigned encounters from the last seven days.

# Payments at the front desk

Copays and deposits taken at the desk are posted with **Pat Payment** in the
patients grid's side panel (*Post patient payment*). The *Patient Payment
Dialog* opens on the **New Payment** tab.

1. Choose the encounter in *Enc. Num.*, or the claim in *ClaimKey* if one
   already exists. A patient with no encounter cannot be given a payment:
   create the encounter first.
2. If the patient pays by check, type the check number in *Chk Num.*
3. On each line, choose what the payment is for in *Concept*, type the amount
   — in *Deductible* if it counts toward what the patient owes on the claim,
   in *NonDeductible* if it is for something else, such as materials — and the
   *Method* (Cash, Check, ATH, Visa, M/C, Amex).
4. Click **Save**, then **Print** for the receipt.

All the lines saved together share one receipt number. The **Previous** tab
lists the patient's earlier payments and reprints any receipt.

A payment taken before the claim exists is held on the encounter, and the
claim shows it as *Recvd @ Pay Wndw*. Until the claim takes it over, the
claim carries the tag **Unattached Patient Payments**.

# Encounters that have not been billed

An encounter with no claim is a visit nobody has been paid for. The longer it
waits, the closer it gets to the payer's filing deadline.

In PrBill10, the **Unbilled encounters** button on the toolbar (or *Tables →
Unbilled Encounters*) lists them. The dashboard's Billing card shows how many
there are in the last 90 days and the date of the oldest; click that line to
open the list for those 90 days.

The list shows active encounters with no claim. Its columns include the
patient, the date, the doctor, the payer, and **Ordered**: what the patient
came for, from the orders.

- The *Filter* panel narrows it by encounter type, payer and dates. **Changes
  apply when you click Refresh.**
- **Show Inactive Encounters** includes no-shows; **Show Only Unbilled**,
  unticked, shows billed encounters too.
- **New Claim** in the side panel creates the claim for the highlighted
  encounter (Part II). After you save the claim, click Refresh and the
  encounter leaves the list.

The *Mp10 Dashboard* guide, chapter *Unbilled encounters*, explains how to
work through the list and when to inactivate rather than bill.

For a printed list, use *Reports → Unbilled Report*.

> **Institutional:** Patients10 has no unbilled list of its own; use
> *Reports → Unbilled* there, or the Unbilled Encounters list in MpBill10.

# Part II — Claims, coding and collections {.unnumbered}

Turning an encounter into a claim, coding it, the error tags, sending it,
and following it with the status flags until it is paid.

# Creating a claim

A claim is the bill sent to the payer for one encounter. Mp10 numbers claims
`CL` followed by the year and a sequence, such as `CL26-00012345`.

## Where to start

Claims are created from the encounter they bill:

- **From the unbilled list** (the usual way): highlight the encounter and
  click **New Claim**.
- **From the Encounters grid** (*Tables → Encounters*): highlight the
  encounter, click **New Claim**.
- **From the patient**: on the patient's **Claims** tab, click **Create New
  Claim** (or press Insert), then choose the encounter in the claim's
  *Encounter* list.

The Claims grid itself has no New Claim button: a claim always starts from
its encounter. Only BillingUsers and Administrators can create claims.

## What is filled in for you

A new claim starts from what is already known:

- **Insurance** and **Card Number** — the patient's primary card.
- **Comp Ins** — the patient's secondary card's payer, if there is one.
- **Serv/Date** — the encounter's date.
- **Referring Phys** — the encounter's referring doctor.
- **Facility** — your practice's facility.
- **Procedure lines** — one line for each procedure code ordered on the
  encounter, dated on the visit date, with one unit, pointing at the first
  diagnosis.

Lines that come from the orders are priced from your fee schedule for the
claim's payer, as of the date of service: fee, expected amount, deductible,
description, place and type of service. A code the schedule does not carry
for that payer stays at 0.00 — look at the *Fee* total before saving, and
price any such line by picking the code in its *BillCode* cell.

## The claim window

Its tabs, in PrBill10, are **Summary**, **Procs** (procedures), **ICD9**,
**ICD10**, **Rmrks** (remarks), **Pymnts** (payments), **EncReps** and
**PaprWrk** (paperwork). The procedure lines appear on both Summary and Procs;
they are the same lines.

The Summary tab holds the claim's header:

| Field | What it is |
|---|---|
| *Medical Record*, *Encounter* | The patient and the encounter. Fixed when the claim was started from the encounter |
| *Insurance* | The payer billed |
| *Comp Ins* | The complementary (secondary) payer, billed after this one pays |
| *Card Number* | The member ID. The **270** button beside it asks the payer for eligibility now |
| *Facility* | Where the service took place |
| *Pre-Auth Numb.* | The prior authorization number, sent on the claim |
| *Perf Phys* | The performing (rendering) physician |
| *Referring Phys* | The referring physician |
| *ICN* | The payer's number for this claim, needed to correct it later — see *Correcting and resubmitting a claim* |
| *Serv/Date*, *Bill Date* | Date of service and the date billed |
| The money fields | Payments, adjustments and balances. Worked out by Mp10; you cannot type in them |

At the bottom: **Save**, **Exit** and **Print**, and the claim's **tags**
(see *Error tags*).

> **Institutional:** MpBill10's claim window adds the tabs **UbCds**,
> **Rec Room** and **Charges**. Its lines come from the charges posted to the
> encounter, an encounter number is required, and the pre-authorization comes
> from the encounter.

## Primary and secondary claims

The claim you create is always the **primary** claim. You do not create the
secondary claim yourself. When the primary payer's payment is posted and part
of the balance is assigned to the complementary payer, **Mp10 creates the
secondary claim automatically**, billing exactly what was assigned. See
*Complementary insurance claims* in Part III.

A secondary claim opens with the bar *Complementary Insurance Claim* and
does not let you change its lines.

# Coding the diagnoses (ICD-10)

## The ICD10 tab

The **ICD10** tab (Alt+C) lists the claim's diagnoses in order. **The first
diagnosis is the principal one** — the reason for the visit.

- Type a code straight into the *IcdCode* cell, with its dot (`M17.11`). If
  the code is valid, its description fills in. If it is not an exact code, the
  row turns **red** and the description of its category is shown instead.
- **New** adds a row. Delete removes the highlighted one.
- **Move up** and **Move Down** change the order.
- **Run Analizer** checks every code on the claim (next section).

The diagnoses belong to the **encounter**, not to one claim: if an encounter
has more than one claim, they share the list. Up to twelve are sent on the
electronic professional claim; any after the twelfth are not.

## Looking codes up

Click the small button inside the *IcdCode* cell to open the **Diagnosis
Table**, the ICD-10 encoder.

- **Search by words:** type them in *Filter Keys* on the left and click *Run
  Filter*. Tick *Left*, *Right* or *Bilateral* to narrow to one side.
- **Search by code:** on the *ICD10 Diagnosis* tab, sort by the *Icd10*
  column and type, or press F8.
- **Browse:** open the chapters and sections in the *Icd10 Chapters +
  Sections* tab. Opening a subsection lists its codes.
- **Pick:** double-click a code. It is added to the *Coding* box on the
  left. Pick as many as you need, then **Close Encoder**.

When the encoder closes, Mp10 checks the codes you picked and adds them to
the claim. Along the way it may ask you:

- **A code is not billable** — *ICD10 Code seems invalid*, with the reason:
  not an ICD-10-CM code, a header code that has more specific codes under it,
  retired, or not yet valid. Choose the more specific code.
- **An additional code may apply** — ICD-10-CM says "use additional code";
  tick the ones that apply.
- **A seventh character is needed** — pick it from the list.
- **Excludes1** — two codes ICD-10-CM says are never coded together. Keep
  both only if the two conditions are unrelated.
- **Diagnosis sequence** — a "code first" instruction asks for the codes the
  other way round. Move them.

## Pointing lines at diagnoses

Each procedure line has a **Diag** column: which diagnoses justify it, by
their position in the ICD10 list. `1` is the first diagnosis, `1,2` the first
and second. Separate numbers with commas.

Two things to watch:

- **Picking a code on a line resets its Diag to 1.** Set the pointers after
  the codes.
- **Reordering the diagnoses changes what the numbers mean.** After moving a
  code up or down, check every line's pointers.

# Coding the procedures

## The procedure lines

Each line is one service:

| Column | What it holds |
|---|---|
| *BillCode* | The procedure code, followed by its modifiers |
| *Unts* | Units (at least 1) |
| *Fee* | Your charge |
| *Expected* | What the payer is expected to allow — this is what the claim bills |
| *Deduct* | The patient's deductible share |
| *From*, *Thru* | Dates of service |
| *Diag* | The diagnosis pointers (above) |
| *Tos*, *Pos* | Type and place of service |
| *Performing* | The performing physician for the line |

Add a line with **New** (*Add a procedure line*), Insert, or the down arrow on
the last line. Delete removes the highlighted line straight away.

## Codes, prices and the fee schedule

Type the code in *BillCode*, or click the cell's button to pick it from **Valid
Billing Codes** — the codes your practice has priced **for this claim's
payer**, as of the date of service. When you pick or confirm a code, Mp10 fills
in *Fee*, *Expected*, the description, the revenue code, the place and type of
service, and the deductible.

Prices come from your practice's billing codes table, kept by payer and by
effective date in MpAdmin. If a code is not priced for the payer, the picker
opens so you can choose; if it is not there at all, ask whoever keeps the
fee schedule to add it.

Changing the units changes *Expected* and *Deduct* in proportion.

## Modifiers

Type modifiers **after the code, separated by spaces**: `76881 TC RT`. Up
to four are sent. If your fee schedule prices the code with its modifiers as a
separate entry, that price is used; otherwise the base code's price is. After
the price fills in, check the modifiers are still on the line.

## Checking a batch of claims before sending

In the Claims grid, select claims and click **Analyze 837** in the side panel.
Mp10 reviews them all and prints a list of warnings: missing or invalid
codes, duplicate diagnoses, codes that need a seventh character or a side,
missing place of service, the tags each claim carries. Nothing is changed or
sent.

# Saving the claim, and error tags

## When you save

Click **Save**. Mp10:

- refuses a new claim with no lines (*Nothing to save*);
- warns if the same codes, dates and payer are already billed for this
  patient (*Billing Codes, dates, and insurance seems to be duplicated*) —
  answer No if it is a duplicate;
- refuses a payer that is not set up in your insurance list;
- gives a new claim its number;
- runs the **claim checks** and tags the claim with what they find.

If anything fails, nothing is saved: *The claim was NOT saved. Everything
from this save has been rolled back*, followed by the reason.

## Error tags

A **tag** is a coloured label at the bottom of the claim window naming a
problem a check found: *Wrong Side ICD10*, *ICD10 Relation*, *NCCI Pair
Needs Modifier*, *Denied Before*, and others. Tags never stop you from saving
or sending a claim. They tell you what a payer is likely to reject, before it
does.

- **Hover a tag** to see what it found.
- **Click it** for the full explanation: which lines, which codes, and why.
- **To clear it,** fix what it names and save. Each check runs again on every
  save and removes its own tag when the problem is gone.

Three checks offer to fix the problem for you as you save, and change
nothing unless you answer Yes:

- **Insurance DOB Mismatch** — the payer's last eligibility answer gives a
  different date of birth. Yes writes the payer's date on the patient's
  insurance card, not on the patient.
- **Insurance Sex Mismatch** — Yes updates the patient's record.
- **Payer Not On Patient** — the claim bills a payer the patient has no card
  for. Yes adds it to the patient's insurances; check the member number and
  dates afterwards. Answer No if the claim is the one that is wrong.

Two tags learn from the remittances you post (Part III): **Denied Before**,
when this payer has refused these codes often and recently, and **Refused For
This Patient**, when the payer refused these very codes for this patient and
has paid nothing for them since.

You can also add your own tag with the **+** button beside the tags, to mark
a claim for yourself or a colleague.

The full list of checks, what each looks for and how to switch one off is in
the *Mp10 Dashboard* guide, chapters *Claims with error tags* and *The claim
checks*. The dashboard's *Needs attention* card lists every claim tagged in
the last seven days.

In the Claims grid, a claim with tags shows a tag icon in *SrvDate* and its
row is coloured orange.

# Finding claims

Open the **Claims table** from the toolbar (or *Tables → Claims*). The *Claims
Data Grid* lists every claim.

## The columns

*ClaimKey*, *Creation*, *MedRec*, *Encounter*, *SrvDate*, *Contract* (the member
ID), *Insurance*, *BillDate*, *Name*, the charge (*Real Amt*), insurance paid,
adjusted and balance, the deductible, patient paid, balance and adjustments,
the user who created the claim and the performing physician.

The *ClaimKey* column shows the claim's **status flag** (next chapter).
Right-click it, or press F9, to see the claim's status history.

## The row colours

| Colour | Means |
|---|---|
| Orange | No bill date, no diagnoses, or the claim has tags |
| Cyan | Resubmitted: corrected, replacement or void |
| Magenta | Created by AutoTasks, Mp10's automatic service |
| Grey | No charge or no service date |
| Blue | Everything else |

## Filtering

The *Filter* panel on the left narrows the grid by any combination of:

- **Status** — one flag, or *Missing Flag* for claims with no status at all;
- **Ins** — one payer;
- **Srv From / Srv Thru** — dates of service;
- **Bill From / Bill Thru** — billing dates;
- **Wrk From / Wrk Thru** — the date the claim was created;
- **Med Rec** — one patient.

Switch the filter on with its LED. To jump to one claim, sort by *ClaimKey*
and press F8.

**Filter From File** shows exactly the claims on a list you have — a payer's
rejection report, for instance. Paste the claim numbers into its grid, or
point it at a text file with one per line, and click *Run Filter*.

## Doing things with claims

The side panel acts on the highlighted claim (or claims):

| Link | Does |
|---|---|
| **Open Claim** | Opens it (also Enter or double-click) |
| **Print Claim**, **Preview Claim** | The paper claim form, for every selected claim |
| **Post Payment** | The payment window for the claim (Part III) |
| **Notes**, **Attachments**, **AuditLog** | The claim's notes, documents, and history of changes |
| **Claim Status** | The status history; with several claims selected, change their flag together |
| **Eligibility** | Asks the payer now; with several selected, asks for each |
| **Analyze 837** | Reviews the selected claims before sending |
| **Outbound 837** | Builds and sends the electronic claims file |
| **Pat Payment** | A patient payment on this claim, with a receipt |

Double-click the *Encounter* cell to open the encounter, or *MedRec* to open
the patient. Ctrl- or Shift-click selects several claims. Claims are never
deleted: to cancel one, adjust it to a zero balance.

# Sending claims

## Electronically (837)

1. In the Claims grid, select the claims to send. **They must all be for the
   same payer**; filter by *Ins* first.
2. Optionally, run **Analyze 837** and correct what it finds.
3. Click **Outbound 837**. Mp10 writes the claims file and shows a report of
   every claim in it, with the file name and any warnings at the end.
4. Answer **Yes** to *Send claims file to clearinghouse*.

Each claim records, in its notes, the name of the file it went out in. That
file name is your batch number.

Outbound 837 also collects whatever the clearinghouse has waiting —
acknowledgements, claim statuses and remittances — and processes it, so the
flags in the grid are up to date afterwards.

Supervisors and Administrators also have *Actions → Send Outbound 837 Claims*,
which lists the claims files waiting in the outbound folder and sends them,
and *Actions → Fetch X12 Inbound Responses*, which collects and processes
the clearinghouse's files on demand. Normally AutoTasks does both on a
schedule; see the *Mp10 AutoTasks* guide.

## On paper

**Print Claim** in the Claims grid prints the claim form — CMS-1500 or UB-04
— for every selected claim; **Preview Claim** shows it first. *Actions → Print
Claims* prints all the claims of a date range for the payers you tick. Which
form a payer gets is set on the payer's record (*Form #*). A printer whose
name contains `FORM` is used without asking.

# What the payer's record decides

Several things about a claim are decided not on the claim but on the
**payer's record** — the insurance company, under *Tables → Insurance
Companies* (or in the administration program). A biller who knows these
settings exist can tell "the system did it wrong" from "this payer is set up
that way", and ask for the right change.

On the **Ins Info** tab:

| Setting | What it decides |
|---|---|
| **Professional** / **Institutional** | Whether this payer's electronic claims go out as professional (837P, the CMS-1500 content) or institutional (837I, UB-04), and which NCCI tables the claim checks use for it |
| **Form #** | Which paper form prints for this payer. A payer with no form number cannot print a paper claim (*Undefined Insurance Form*) and is left out of the professional Unbilled Report |
| **In-active** | The payer can no longer be put on a card or a claim: *Insurance is Inactive*. Old claims keep it |
| **Billing Fees From** | Institutional only: whether the payer is billed by DRG/APC, private rates or the fee schedule |

On the **X12** tab:

| Setting | What it decides |
|---|---|
| **Receiver ID**, **Submitter**, **Sender** and the ISA/GS fields | The clearinghouse addressing on every electronic claim, eligibility request and remittance match for this payer. Set once by the administrator with the clearinghouse's instructions; a wrong value rejects every claim |
| **Auto-Fetch Patient Eligibilities** | Whether AutoTasks asks this payer, by itself, for the eligibility of each upcoming encounter. Off, and eligibility is only checked when someone clicks *Eligibility* |
| **Include PRV Taxonomy segment** | Whether the electronic claim carries the provider taxonomy segments. Some payers demand them, some reject them |
| **Check NCCI PTP pairs** | **Off by default.** Tick it and the claim checks apply the NCCI procedure-to-procedure edits for this payer, placing the tags *NCCI Pair, Never Together* and *NCCI Pair Needs Modifier*. Tick it for every payer that follows Medicare's NCCI edits — most do. The *Units Over MUE* check runs for every payer regardless |

The fee schedule itself — the price and expected amount of each procedure
code, by payer and by effective date — is the **Billing Codes** table, kept
in the administration program. A line that prices at 0.00 on a claim means
the code is not in that table for that payer on that date.

# Status flags: the rebilling and collections system

Sending a claim is the start, not the end. Payers acknowledge it, reject it,
pay part of it or all of it, sometimes months apart. **Mp10 keeps every one of
those answers on the claim as its status history, and shows the latest as a
coloured flag.** The flags turn the Claims grid into a work queue: filter by
flag and you have the list of claims that need you.

## The flags

| Flag | Name | Means | Set by |
|---|---|---|---|
| Green | Fully Paid | Nothing left for the insurance to pay | Posting a payment or remittance |
| Yellow | Partially Paid (also *Pending*) | Something paid, something still owed | Posting; a payer status saying it is pending |
| Red | Rejected | Refused, by the clearinghouse or the payer, or paid nothing on the remittance | Claim status responses; posting a remittance that pays nothing |
| Grey | Received by Ins | Accepted by the clearinghouse or payer, not paid yet | Claim status responses and acknowledgements |
| Black | OverPaid | More was paid than billed | Posting; also a payer status saying the claim was not found |
| Blue, checkered, pink, purple | User Def 1–4 | Your practice's own meanings | You |
| None | No status | Nothing heard yet | — |

The four user flags can be renamed for your practice — *Appealed*, *Called
payer*, *Sent to collections* — by your administrator, in the system registry
under **BILLING / UserDefined-1-ClaimStatus** to **-4-**.

## Where flags come from

- **Claim status responses (277)** and the clearinghouse's acceptance reports
  arrive automatically and add a row to the history, with the payer's words.
  They move the flag only on a claim that has no status, is rejected or is
  received. They never move a claim that is paid, partly paid, overpaid or
  carries one of your own flags.
- **Posting a remittance or a payment** works out the flag from the balance:
  green when nothing is left, yellow when something is, red when nothing was
  paid, black when overpaid. A payment that settles the claim always turns it
  green.
- **You**, through the status history or *Change Flags* (below).

## The status history

Press **F9** on a claim in the Claims grid (or right-click its *ClaimKey*, or
use **Claim Status**). The *Claim status history* window lists every status
the claim has had, oldest first: the flag, the date it was received, what was
said, who recorded it and when.

The *Data* column holds the payer's or clearinghouse's own words for status
responses, *Remittance Posting.* for remittances, *Payment Posted by user.*
for payments keyed by hand. The button in the cell shows it in full, and its
**Print** prints a status response as a readable form.

To record a follow-up — a call to the payer, an appeal sent, a promise to
reprocess:

1. Click **Add New most recent status to list**.
2. Choose the flag.
3. Set the date.
4. Type a note in *Data* — what was done, who you spoke to, what was
   promised. A row saved without one reads *Changed by user manually*,
   which tells the next person nothing; write the note.
5. Click **Save**.

The new row becomes the claim's flag. Earlier rows cannot be changed, so the
history is a true record of what happened when.

To change the flag of many claims at once, select them in the grid and click
**Claim Status**. The *Change Claim Status* window lists the flags; tick one
and click **Change Flags**. Each claim gets the new flag and a history row
*Changed by user manually*. This is also the only way to clear a flag back to
no status.

Mp10 has no follow-up date. The date on the history row, and the *Days*
column of the Claims Status report (below), show how long a claim has been
waiting since its last status.

## Working the queue

A routine that keeps money from slipping past filing deadlines:

**1. Rejections, every day.** The dashboard's Billing card shows the number
of rejected claims in the last 90 days. Click it for the **Rejected claims**
list: each claim with the date it was rejected, where the rejection came
from — *277/CH* for the clearinghouse or payer, *835* for a remittance — and
the reason, in the payer's words or as the remittance's reason codes.
Double-click a claim to open it, correct it, and resubmit (next chapter).
Click **Refresh** and corrected claims leave the list.

**2. Claims with no answer.** A claim that has heard nothing at all after a
couple of weeks may never have reached the payer. Filter the Claims grid by
*Missing Flag* and a range of bill dates, and check those claims. The
dashboard shows the count of primary claims with no status and the date of
the oldest.

**3. Claims stuck in received or partly paid.** *Reports → Claims Status*
lists claims with an insurance balance by flag and payer, with **Days** —
how long each has sat on its current flag. Tick *Received* and *Pending*, read
the oldest first, and call or appeal. Record each call in the status history
so the next person knows. The report lists only claims that have a flag; use
the grid's *Missing Flag* filter for the ones with none.

**4. Dollars by age.** *Reports → Aeging of Accounts* totals what is owed by
age — 30, 60, 90 days and beyond — for insurance and for patients, by payer
or by financial class. Use it to see where the money is waiting, and the
Claims Status report to work it.

**5. What the patient owes.** When the insurance has paid its part, the rest
is the patient's: see *Collecting from the patient*.

# Correcting and resubmitting a claim

When a payer rejects or underpays a claim, correct the claim and send it
again, telling the payer which claim it replaces.

1. **Correct the claim.** Open it, fix what the rejection names, save. Check
   the tags.
2. **Set the submission code.** In the Claims grid, highlight the claim and
   press **F4**. In *Claim Submission Code*, choose:
   - **Corrected** or **Replacement** — this claim replaces the one the payer
     has (frequency code 6 or 7);
   - **Void** — cancel the claim the payer has (frequency code 8);
   - **Original Claim** — a first submission (the default).

   Click **Save**. The claim's row turns cyan, and the claim window's bar
   starts with *Corrected:*, *Replacement:* or *Void:*.
3. **Check the ICN.** A corrected, replacement or void claim must carry the
   payer's number for the original claim, its **ICN** (on the Summary tab).
   When the original was paid or denied on an electronic remittance you posted,
   Mp10 has already filled it in. When you type it yourself from a paper EOB,
   **type F8 before the number** — `F8` followed by the ICN, with no space — or
   it is not sent as the original claim's number. Mp10 does not warn you if the
   ICN is missing; the payer will reject the claim.
4. **Send it** with Outbound 837, or print it.

This is the first reason to post remittances whole: they bring the ICN onto
the claim for you. Part III explains the others.

# Collecting from the patient

Once the insurance has paid, what remains on the claim is the patient's
balance, shown in the claim's *Pat Balance* and in the Claims grid's
*PatBal*.

**Collection letters.** *Actions → Pat Collection Letters* lists the claims
with a patient balance for a range of dates — service dates or billing dates,
whichever box you tick — at or above the **Minimum Debt to print collection
letter** you enter. For each claim it shows how many days it has been since
the last letter, and which letter that was. Tick **Also Print Next
Corresponding Letter** to print each patient their next letter: the first
letter for a patient who has had none, and the next one in the series once
30 days have passed since the last. Your practice can have several letters,
each firmer than the last; how many is set by **BILLING /
MAXIMUM_PATIENT_COLLECTION_LETTERS** (three by default), and a patient who has
had the last one is not printed again. Every letter printed is recorded on
the patient's **Letters** tab.

**One patient at a time.** On the patient's **Claims** tab, *Current Patient
Bal* shows what they owe in total, and **Print Patient Statement** prints a
collection letter for them. The patients grid's **Prnt Statement**
prints a full statement of every service.

**Taking the payment** is described in *Payments at the front desk* (Part I),
or with **Pat Payment** in the Claims grid, which prints a receipt.

# Part III — Remittances and payments {.unnumbered}

Receiving, reading and posting the payer's remittances, the other ways to
enter a payment, and why the remittance is the one to use.

# What a remittance is

A **remittance** is the payer's explanation of one payment: one check or
electronic transfer, and every claim it pays, denies or adjusts. For each
claim it says what was billed, what was allowed, what was paid, what the
patient owes, and why anything was not paid, in standard reason codes. It
also carries the payer's own number for each claim, the ICN.

Mp10 receives remittances in two forms:

- **Electronic remittances (X12 835)** come from the clearinghouse. AutoTasks
  collects and imports them on a schedule.
- **PDF remittances** (EOB or ERA PDFs downloaded from a payer's website) are
  saved into a folder that AutoTasks watches. It reads them and imports them.

Both end up in the same place, the **Remittances** window, **as remittances
waiting to be posted**. Importing posts nothing: no claim balance changes
until someone posts the remittance. Setting up both is described in the
*Mp10 AutoTasks* guide, chapters *Electronic remittances: the two kinds*,
*X12 835 electronic remittances* and *Electronic remittances (EOB / ERA
PDFs)*.

A paper EOB that arrives by mail can be entered by hand in the same window:
see *Entering a remittance by hand*.

# Finding a remittance

Click **Remittances Data Entry Dialog** on the toolbar (the check), or
*Tables → Remittances*. The window has two tabs:

- **Remittances** — the list of remittances.
- **Detailed Check Tree View** — one remittance, open for posting.

## The list

The **Remittances** tab lists one row per check, with its *Check* number,
*Insurance*, *ChkDate* (check date), *RcvDate*, *Deposit* date, *ChkTotal*,
*Amount*, *Retained*, whether it is posted, the file it came from and the
number of lines. A green dot beside the check number means posted; a red
dot means waiting to be posted.

In the **Remittances Period** panel on the left, narrow the list by check
dates (*From*, *Thru*), by payer (*Insurance*, any part of the name), or tick
**Show Only Posting Pending** for the ones still to post. Then click **Reload
Remits** in the *Actions* panel.

To find a check number or an amount, sort by the *Check* or *ChkTotal*
column and look down the list.

A check that is partly posted shows twice: one row for the posted lines, one
for the lines still waiting. The waiting lines are almost always claims Mp10
could not match (see *When a claim is not found*).

## Opening one

Double-click it, or press Enter. It opens on the **Detailed Check Tree View**
tab.

# Reading a remittance

## The check

The **Remittance Info** panel on the left describes the check:

| Field | What it is |
|---|---|
| *Chk Num*, *Ins*, *Chk Date* | The check (or transfer) number, the payer, the date |
| *Rcvd On*, *Dep On* | When it was received and deposited |
| *Claims Pd* | The sum of everything paid on the claims |
| *PLB Adj* | Money the payer added or withheld outside any claim — interest, a recovered overpayment |
| *Check Amt* | *Claims Pd* plus *PLB Adj*: what was actually deposited |
| *PLB Rsn* | Why the payer made the *PLB Adj* |
| *Diff.* | *Claims Pd* less what the lines below add up to. **It should be zero before you post** |

## The claims

Below, each claim on the check is a bold row, and its procedure lines are
underneath. F9 opens every claim, F10 closes them.

| Column | What it holds |
|---|---|
| *ClaimKey / Bill-Code* | The claim, and the procedure code on each line |
| *Account*, *SrvDate*, *PatName* | The encounter, the date of service, the patient |
| *Expected* | What the claim expected the payer to pay |
| *Paid* | What the payer paid |
| *Adj* | The amount written off — the contractual adjustment |
| *Deduct* | The patient's deductible or coinsurance on this line |
| *CoIns* | The amount to bill to the complementary payer |
| *Pending* | What will still be owed after posting |
| *AdjCodes* | The payer's reason and remark codes, with their meaning |
| *ICN* | The payer's number for the claim |

The dot on each row says, before you post, what posting will do:

| Dot | Means |
|---|---|
| Green | Nothing will be left pending |
| Yellow | Paid, but something will still be owed |
| Red | Nothing paid — a denial |
| Black | Overpaid |
| Grey | The claim was not found in Mp10; this line cannot post |

**Open Claim** (right-click) opens the highlighted claim to look at, and
**Print Single Claim 835 EOB Notice** prints the payer's own text for that
claim. **Search** (F8) finds a claim, an account or a patient on a long
check.

# Posting a remittance

## Before you post

The import fills in what the payer paid, the deductible and coinsurance, the
reason codes and the ICN. Most write-offs it leaves to you:

**Contractual adjustments.** The difference between your charge and what the
payer allows (reason code CO-45, and others like it) has to be written off
before the claim balances. To write off what is left on a line, right-click
its *Adj* cell. On a claim row, the same writes off every line. To do the
whole check at once, click the *Adj* column header: *Maximum Amount To Adjust*
writes off every paid claim whose remaining balance is no more than the amount
you give. Read the reason codes first — not everything unpaid should be written
off.

**Deductibles.** Click the *Deduct* column header to choose what posting does
with deductibles: **Add to patient responsiblity** moves them to the
patient's balance (the default), **Ignore Deductibles** leaves them out.

**Coinsurance for the complementary payer.** When the claim has a
complementary payer, what is in *Deduct* and *CoIns* is billed to it (see
below).

**The ICN.** Check it is there; you can type it if it is missing.

Then watch **Diff.**: when it is zero, every cent of the check is accounted
for.

## Posting

1. Click **Save Remit** (Ctrl+S).
2. Click **Post Remit** in the *Actions* panel (or right-click → *Post
   Remittance*).

A progress bar runs, and Mp10 reports how many entries it posted:
*Remittance has been posted to payments table.*

Posting a check, in one step that either completes or changes nothing:

- records a payment, an adjustment and the reason codes on every claim line;
- recalculates each claim's balances;
- sets each claim's **flag** — green, yellow, red or black — and adds a
  *Remittance Posting.* row to its status history;
- moves deductibles to the patient's balance, as you chose;
- creates or updates the **complementary claims** (below);
- records the payer's refusals in the **denial history** that the claim
  checks learn from.

Only lines that are not yet posted are posted. Posting the same check again
posts nothing: *Already posted*. If Mp10 answers *Nothing posted*, the check
number, payer or date on the left do not match the ones the remittance was
saved with — correct them and post again.

## There is no undo

A posted remittance cannot be unposted. Check *Diff.* and the dots before you
post. A mistake found afterwards is corrected on the claim, with the payment
window (see *Other ways to enter a payment*).

An Administrator can delete a remittance that has **not** been posted, from
the right-click menu of the Remittances list (*Delete Remittance*): for
instance, a check entered twice by hand.

## Complementary insurance claims

When the patient has a second insurance that pays after the first (the
claim's *Comp Ins*), posting the primary payer's remittance hands the rest
over:

- The amounts in *Deduct* and *CoIns* are assigned to the complementary payer
  instead of the patient.
- **If there is no secondary claim yet, Mp10 creates it**: a claim for the
  complementary payer, billing exactly what was assigned, with the patient's
  secondary card. Send it like any other claim.
- If the secondary claim already exists, it is adjusted to the new amount.

Posting a check twice never creates a second secondary claim.

## When a claim is not found

A grey dot means Mp10 could not tell which claim the payer was paying — the
payer quoted a number Mp10 does not know. Those lines do not post; the rest
of the check does, and the check stays in the list as partly pending.

To fix one:

- Type the right claim number (or the encounter number) in its
  *ClaimKey / Bill-Code* cell; the claim's lines load. Save and post again.
- **Re-Fetch Missing Claims** (right-click in the Remittances list) tries the
  match again from the original file — useful after a patient or claim has
  been corrected.
- **Change Insurance Info** moves the whole check to the right payer, when
  it was filed under the wrong one.
- **Re-Fetch From 835** throws away an unposted remittance and imports it
  again from its file.

# Entering a remittance by hand

A paper EOB is entered in the same window, so that it is posted, reconciled
and remembered exactly like an electronic one.

1. Click **Start New Remit**.
2. On the left, enter *Chk Num*, the payer (*Ins*), *Chk Date*, and *Claims
   Pd* — the total the EOB says was paid on claims. Add *PLB Adj* if the payer
   withheld or added money outside the claims.
3. Add the claims on the EOB:
   - press Insert (or right-click → *Add New Line*) and type the claim number
     in *ClaimKey / Bill-Code*: the claim's lines load; or
   - click **Fetch Claims Set** to load many claims at once — all the unpaid
     claims of the ticked payers for a range of dates, or a list of claim
     numbers you type in.
4. On every line, **enter what the EOB says**. Mp10 fills *Paid* with the
   expected amount to save typing, so correct each one that differs. Enter
   *Adj*, *Deduct*, *CoIns*, and the *ICN* from the EOB.
5. Enter the reason codes in *AdjCodes*, one per line, as `[45]-` followed
   by the description. Codes entered this way count toward the denial history,
   like the ones that come in electronically.
6. When *Diff.* is zero, **Save Remit**, then **Post Remit**.

# Other ways to enter a payment

## A payment on one claim

**Post Payment** in the Claims grid, or **Add New Payment to Claim** on the
claim's **Pymnts** tab, opens the *Single Payment Dialog*. The claim must be
saved first.

On the **New Payment** tab, enter the *Check #*, *Check Date* and *Deposit
Date* (they are remembered for the next claim), then, on each procedure line:

| Column | Enter |
|---|---|
| *InsPaid* | What the insurance paid |
| *Substract* | The insurance write-off for the line |
| *PtePaid* | What the patient paid |
| *PSubstct* | The patient write-off |
| *CoIns* | What to bill to the complementary payer |
| *AdjReason* | Why, from your list of adjustment reasons |

The buttons above the grid fill columns for you: the deductible as paid, the
expected as paid, the insurance balance as coinsurance. The cell buttons do
the same line by line.

The **Simple Adjust** tab adds to or subtracts from the whole claim's
insurance or patient balance in one entry, with a reason and a note. **Save
simple adjustments separately**: when any Simple Adjust amount is filled in,
the line payments on the New Payment tab are not saved in the same Save.

The **Previous** tab lists everything already posted to the claim.

When you save, the claim's balances and flag are recalculated, and a
secondary claim is created if you assigned coinsurance and the claim has a
complementary payer.

## Patient payments

Payments from the patient are posted with **Pat Payment** — from the patients
grid at the front desk (Part I), or from the Claims grid, which ties the
payment to that claim and prints a receipt.

## Adjusting balances in bulk

*Actions → Adjust Accts* (Supervisors and Administrators) writes off small
remaining balances across many claims: by dates of service, a range of
balances, the payers you tick, and a reason. It first prints the list of
claims it would adjust, and asks you to confirm before it changes anything —
**Create report only** is ticked to begin with, so a first run only shows
you. Adjusted claims are marked paid.

## Refunds

Mp10 has no refund function. An overpaid claim shows a black flag; ask your
administrator how your practice records a refund.

# Why post whole remittances

Typing a check's payments claim by claim in the payment window balances the
claims, but it leaves out everything else the remittance knows. Posting the
remittance itself — electronic, PDF, or a paper EOB entered in the
Remittances window — gives you four things the payment window cannot:

**The ICN, for corrections.** The remittance carries the payer's number for
every claim, and Mp10 puts it on the claim. A corrected or replacement claim
must quote it, or the payer rejects it as a duplicate. Posted from a
remittance, it is already there when you need it.

**The denial history.** Every refusal on a posted remittance is remembered
— which payer, which codes, which reason — and the claim checks learn from it.
The **Denied Before** tag warns the biller, before the claim goes out, that
this payer often refuses these codes. **Refused For This Patient** warns,
at the front desk when the service is ordered and again on the claim, that
the payer already refused it for this patient. The dashboard's rejected
claims list explains each remittance denial with its reason codes. **All of
this reads the remittances only.** A denial typed into the payment window is
invisible to it, and the next claim goes out with the same mistake.

**Reconciliation.** The remittance must add up to the check: *Diff.* shows
you, before you post, whether every cent is accounted for, and *PLB Adj*
explains what the payer withheld. The payment window has no check total to
compare against.

**The flags and the secondary claims.** Posting sets every claim's flag and
status history in one step, and creates each secondary claim with exactly the
amounts the primary payer assigned.

So: post every remittance whole, and use the payment window for what is not
on a remittance.

# Reports for payment posting

| Report | Shows |
|---|---|
| *Print Remittance* (Remittances window) | The open remittance, claim by claim |
| *X12-835 Remittances Report* (right-click the Remittances list) | The payer's file itself; with several checks selected, totals by payer |
| *Reports → Remittances Report* | Remittances by check date, by payer or check by check, with paid, adjusted, deductible and retained totals |
| *Reports → Payments + Adjustments* | Payments and adjustments for a period — by payment date, deposit date or the date posted — grouped by payer, check, user or referring physician |
| *Reports → Claims Status* | Claims with an insurance balance, by flag, and how long each has waited |
| *Reports → Aeging of Accounts* | What is owed, by age |

Every report can be printed, exported, or scheduled to be e-mailed
automatically from its *Automate Report* tab (Supervisors and Administrators).

# Quick reference {.unnumbered}

**At the front desk**

| To | Do |
|---|---|
| Find a patient | Patients Table, type the name, or F8 |
| Add a patient | Insert; **save before** adding insurance or encounters |
| Scan the ID | Demographics → scanner button, then Save |
| Take the HIPAA signature | Demographics → pen → Accept |
| Add a new insurance card | Insurance → New → tick **Primary** → Save. The old card stays as history |
| Scan the card | While adding or modifying: Scan, then Save |
| Check eligibility | Patients grid → Eligibility → Fetch New Eligibility record |
| Open an encounter | Encounters tab → New → date, type, referring doctor → Save |
| Order | Encounter → Orders → New Order → Save |
| Take the visit signature | Encounters tab → highlight → pen → Accept |
| Mark a no-show | Open the encounter → Inactive encounter → Save |
| Take a copay | Pat Payment → encounter → lines → Save → Print |

**In billing**

| To | Do |
|---|---|
| See what is unbilled | Unbilled encounters (or the dashboard line) |
| Create a claim | Unbilled list → New Claim. Confirm each line's code so it prices |
| Code | ICD10 tab; the cell button opens the encoder. First code = principal |
| Point lines at diagnoses | *Diag*: `1`, `1,2`… Recheck after reordering |
| Read a tag | Hover it; click for the full explanation |
| Send | Claims grid → select one payer's claims → Outbound 837 |
| See a claim's history | F9 on the claim |
| Record a follow-up | F9 → Add New → flag, date, **note** → Save |
| Work rejections | Dashboard → rejected claims → double-click, correct, resubmit |
| Resubmit | F4 → Corrected / Replacement / Void; check the ICN (`F8`…) |

**Posting**

| To | Do |
|---|---|
| Find a check | Remittances → Show Only Posting Pending → Reload Remits |
| Write off contractuals | Right-click *Adj*, or the *Adj* header for the whole check |
| Post | *Diff.* = 0 → Save Remit → Post Remit |
| Fix a grey line | Type the claim number, or Re-Fetch Missing Claims |
| Enter a paper EOB | Start New Remit → claims → amounts and reason codes → Save → Post |
