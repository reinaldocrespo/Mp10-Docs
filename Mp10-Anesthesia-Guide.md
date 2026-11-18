---
title: "Mp10 for Anesthesia Practices — User's Guide"
subtitle: "Recording the case once, and letting the claim build itself from it"
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

This guide is for the billers and billing supervisors of anesthesia
practices. It explains a set of Mp10 features that build most of an
anesthesia claim for you: the code to bill for each payer, the units, the
physical-status modifier and the price.

You already know how anesthesia is billed: base units, time units, the
physical status, the AA, QX and QZ modifiers, and the way each payer wants
them. This guide does not explain anesthesia billing. It explains what Mp10
needs to know about each case, where that is recorded, and what Mp10 then
does with it, so that you check claims instead of building them.

It follows the order in which the work happens:

1. why it is worth recording the case in Mp10;
2. the one-time setup, done once by a supervisor or administrator;
3. what is recorded on the encounter, every day;
4. what the claim window then does for you;
5. the auto-claim;
6. the tags you will meet, and what to do about each one;
7. a short daily checklist.

## Which program you use

| Your practice | The encounter | The anesthesia claim |
|---|---|---|
| **Professional** (an anesthesia group billing its own claims) | **PrBill10** | **PrBill10** |
| **Institutional** (a facility that also bills the anesthesia) | **Patients10** or **MpBill10** | **MpBill10**, as a second primary claim beside the facility claim on the same encounter |

Everything in this guide is about the Windows programs. Mp10 Web, the browser
version, does not have the encounter's Anesthesia tab, and its claim page does
not fill units from the anesthesia times. Record the times and the physical
status in the Windows programs.

## Other guides this one points to

| Guide | What it covers |
|---|---|
| *Mp10 Patients, Billing and Payments — User's Guide* | The whole billing cycle: encounters, claims, coding, sending, posting. Its chapter *Creating a claim* describes Auto-claim in general, and its sections *Anesthesia practices* and *Anesthesia price* are the reference for what this guide summarises |
| *Mp10 Orders — User's Guide* | The order catalogue and how orders are entered |
| *Mp10 Dashboard — User's Guide* | Every claim check and its tag |

# Why record the case in Mp10

## What Mp10 can do for you

Mp10 can do two things for an anesthesia biller:

- **Build the anesthesia claim for you.** The **auto-claim** creates the
  claim of an encounter from what is recorded on it: the code each payer is
  billed for that procedure, the units, the physical-status modifier, the
  price and the diagnoses. You then review the claim instead of typing it.
- **Fill in a line you add by hand.** When you type an anesthesia code on a
  claim, Mp10 fills the **units** from the anesthesia time, adds the
  **physical-status modifier** for the payers that bill one, and **prices**
  the line with the payer's anesthesia rate.

The advantage is simple: fewer hand calculations, fewer tags to clear, and
claims that go out the same day the case is recorded.

## What it needs, and what happens without it

Mp10 can only work from what is recorded. Each item below is either entered
for each case at the encounter, or kept once in the reference tables of the
administration module.

| What Mp10 needs | Where it is recorded | What it gives you | What happens without it |
|---|---|---|---|
| **The procedure**, as an order | The encounter's **Orders** tab, by the front desk | The auto-claim works out the code to bill for the claim's payer | PrBill10's auto-claim skips the encounter with `no orders`, and the claim is built by hand. (MpBill10 builds the anesthesia claim from the facility claim instead, so it needs no orders.) |
| **Anesthesia start and stop** | The encounter's **Anesthesia** tab | Units calculated from the time, not estimated | The auto-claim estimates the units from the payer's past claims and tags the claim **Anesthesia Units Estimated**: you must check the units against the anesthesia record. A line you add by hand starts at 1 unit, and you type the units |
| **Physical status** (P1 to P6) | The encounter's **Anesthesia** tab | The modifier is added to the anesthesia code for the payers that bill one | For those payers the claim is tagged **Physical Status Missing**, and you add the modifier by hand |
| **The anesthesia code of each surgical procedure** | Administration module, **Anesthesia Codes** | The auto-claim can bill the anesthesia code to the payers that are billed one for that procedure | The procedure can only be billed under its surgical code, even to a payer that is billed anesthesia codes |
| **CMS base units** | Administration module, **Load Anesthesia Base Units** | The base units in the price, and in the units of the payers whose units include them | No formula price for payers whose price adds the base units, and no units from the time for payers whose units include them |
| **The rate per unit of each payer** | Administration module, **Anesthesia Rates** | The price formula, and whether the payer's units include the base units | No formula price: the line is priced from the fee schedule row, and units from the time are time units only |
| **The code on the payer's fee schedule** | Administration module, **Fee Schedule** | The line can be billed at all | The claim window does not accept the code for that payer, and the auto-claim skips the encounter with `no fee for <code>` |

## What you still check

Mp10 does the arithmetic and remembers each payer's habits. It does not see
the anesthesia record. You still check:

- that the times on the encounter are the ones on the anesthesia record;
- that the physical status is the one the anesthesiologist recorded;
- the modifiers that depend on the case: who provided the anesthesia (AA, QK,
  QX, QY, QZ) and 57, 58, 59 and the like. The auto-claim uses the code and
  modifiers the payer is usually billed for the procedure, which may not fit
  this case;
- the diagnoses;
- every tag on the claim, before it is sent.

# One-time setup

This chapter is done once, by a billing supervisor or an administrator, and
revisited when a payer changes its rate. Billers do not need to do it, but it
is worth reading: it is what makes the rest work.

## Who does it

The setup screens are in the administration module (**PrAdmin10** for a
professional practice, **MpAdmin10** for an institutional one), in the
**File** menu. The four anesthesia options (**Load Anesthesia Base Units**,
**Anesthesia Rates**, **Anesthesia Codes** and **Build Procedure
Catalogue**) are open to everyone who can open the administration module:
members of the **Supervisors** and **Administrators** groups, and the ADSSYS
login. The settings are kept in the module's **System Registry** (the
*Actions* panel).

The decisions in the codes and rates screens are billing decisions, which is
why a billing supervisor can open them. Review each suggestion, and approve it
or correct it. Updating the data dictionary (step 1) is still the
administrator's job.

## Step 1: update the data dictionary

These features need new columns and tables. The administrator runs **Update
Data Dictionary** from the administration module's **Actions** panel, after
hours, with nobody else in Mp10, as for any new version. Until it has run,
the Anesthesia tab does not appear and the anesthesia options cannot be used.

## Step 2: turn the settings on

In the administration module's **System Registry**:

| Section / Entry | Value | Effect |
|---|---|---|
| `BILLING` / `ANESTHESIA-TIME` | `YES` | Shows the encounter's **Anesthesia** tab (start, stop, physical status) and lets the claim window fill units and the physical status from it. `NO` (the default) leaves everything as it was |
| `BILLING` / `AUTOCLAIM-CODE-SOURCE` | `PAYER-HISTORY` | The auto-claim bills each payer the code its own past claims used for the procedure. This is the mode anesthesia practices use. `NO` hides Auto-claim; `ITEM` bills the ordered item's code as it is |
| `BILLING` / `AUTOCLAIM-HISTORY-MONTHS` | `24` (the default) | How many months of past claims the auto-claim, the rate suggestions and the physical-status rule learn from |
| `BILLING` / `AUTOCLAIM-ICD10-HISTORY-MIN` | `0` (off) or a percentage, for example `70` | For a practice with no reports: an order with no report gets the procedure's most billed primary diagnosis when it is at least that share of at least 20 claims |
| `BILLING` / `IS_LAB_ORDERS_PRACTICE` | `YES` | Lets the front desk enter orders. A practice that has never entered orders may have it at `NO` |

PrBill10, MpBill10 and Patients10 read `ANESTHESIA-TIME` once per session:
close and reopen them after changing it.

## Step 3: load the CMS base units

CMS publishes a file of anesthesia base units by code each year.

1. Download it from CMS (a text file, one line per anesthesia code).
2. In the administration module: **File → Anesthesia → Load Anesthesia Base
   Units**.
3. Pick the file. Mp10 reads the year from the file name; confirm it or
   correct it when asked (*Year of this file:*).
4. The load ends with *N base units loaded for YYYY.*

Load the base units **before** reviewing the rates: the rate suggestions use
them. AutoTasks also loads a newer CMS file each week when one is posted.

## Step 4: the procedure catalogue

The front desk orders the **procedure** (for example the endoscopy or the knee
arthroscopy), once, whatever the payer. Mp10 then works out the code to bill
for each payer. For that, the catalogue needs one item per procedure.

**File → Build Procedure Catalogue** adds one item for each procedure (and
each side) on your fee schedule. It states the counts and asks first. Existing
items are never deleted: the ones it replaces are marked inactive, and orders
that point at them keep working. Run again, it adds only what is new. The
*Patients, Billing and Payments* guide describes it in its section *Setting
it up*, under *Anesthesia practices*.

## Step 5: review and approve the anesthesia codes

Each surgical procedure item needs the anesthesia code it is billed as. That
is what lets the auto-claim bill `00731AA` to one payer and `43239AA` to
another for the same ordered procedure, and what lets MpBill10 turn the
facility claim's surgical line into the anesthesia line.

Open **File → Anesthesia → Anesthesia Codes**. Mp10 reads the last 36 months
of claims first; a progress bar shows it, and **Cancel** stops it.

The screen lists the active surgical items, the most billed first:

| Column | What it shows |
|---|---|
| *Item* | The item's procedure code |
| *Description* | The item's description |
| *Suggested* | The anesthesia code Mp10 suggests |
| *Score* | How sure the suggestion is (below) |
| *Current* | The anesthesia code saved on the item, if any |
| *To save* | The code you have accepted or typed and not saved yet; *(clear)* when you are removing one |

**How the suggestion is made.** The *Score* says which of two kinds it is:

- **paired *n*** — at a practice that bills the facility claim and the
  anesthesia claim of the same encounter, *n* encounters billed this surgical
  code on one and the suggested anesthesia code on the other. This is the
  strongest evidence there is.
- **mix *nn*%** — how alike the diagnoses billed with the surgical code are to
  those billed with the anesthesia code. 100% would be identical mixes. A
  high score is usually right; a low one, or several anesthesia codes scoring
  close together, needs your judgement.

**Reviewing, row by row:**

- **Accept** takes the suggestion for the highlighted item.
- **Edit** lets you type the code yourself (*Anesthesia code*, five digits,
  00100 to 01999).
- **Clear** removes the item's code.

**Accepting many at once.** Type a score in *scoring at least … %* (70 by
default) and click **Accept all**. It takes the suggestion on every item that
has no code saved and none typed, when the suggestion scores at least that
much (a *paired* suggestion counts as 100). It never replaces a code that is
already there. It says how many it took and how many items are still without a
code.

**Nothing is written until you click Save.** Save writes every row whose code
changed and says how many were saved.

A practical way to work: accept all at a high score, then go down the list
from the top (the most billed procedures) and check each code yourself. A
few dozen procedures usually cover most of a practice's cases.

## Step 6: review and approve the anesthesia rates

Each payer's anesthesia price is a rate per unit, and the payers count units
in two different ways: some bill the time units and add the base units in the
price, others bill units that already include the base units. Mp10 needs to
know both for each payer.

Open **File → Anesthesia → Anesthesia Rates**. Mp10 reads the payers that have
anesthesia lines in the last 24 months (the `AUTOCLAIM-HISTORY-MONTHS`
setting), and opens on the first one. Pick the payer in **Payer**.

The screen has two lists.

**Suggested from the last 24 months of this payer's claims** — for each
year, the rate and the unit convention that reproduce most of the prices the
payer was billed:

| Column | What it shows |
|---|---|
| *Year* | The year of service |
| *$ per unit* | The suggested rate |
| *Units include base* | *Yes* when the payer's billed units already include the base units |
| *Lines agreeing* | The share of that year's lines this rate and convention reproduce |
| *Lines* | How many lines the year had |

**Confirmed rates (what the claim price formula uses)** — the rows you have
approved:

| Column | What it means |
|---|---|
| *From* | The first date of service the rate applies to. It applies until the next row's date |
| *$ per unit* | Dollars per unit |
| *Units include base* | The claim's units already include the base units |
| *P modifiers add units* | P3, P4 and P5 add 1, 2 and 3 units to the price, as in the ASA convention |

**To approve a suggestion,** highlight its year and click **Accept the
suggestion**. It becomes a confirmed rate from January 1 of that year. If the
payer already has a rate from that date, Mp10 asks before replacing it.

**To enter or correct a rate yourself,** click **Add** or **Edit**, and fill
in:

- *From (first service date)*;
- *Dollars per unit* (more than 0);
- *The claim's units already include the base units*;
- *P3 / P4 / P5 modifiers add 1 / 2 / 3 units*.

**Delete** removes a confirmed rate, after asking.

**How to judge a suggestion.** Look at *Lines agreeing*:

- **High (90% or more)** — the payer pays one rate, and the suggestion has
  found it. Accept it, after checking it against a recent remittance.
- **About half, or lower** — the payer's prices do not follow one rate. The
  payer may pay different rates in different cases, or the past prices were
  typed by hand inconsistently. Do not accept it blindly: take the rate from
  the payer's contract or its remittances, and enter it with **Add**.

A rate change from a date (a new year's conversion factor, say) is a new row
with **Add**, not an edit of the old row: the old row keeps pricing the claims
of earlier dates.

Rates are matched on the **payer's name** as the claim bills it. PrBill10 and
MpBill10 see a new or changed rate after they are restarted.

A payer with no confirmed rate is not priced by the formula; its anesthesia
lines are priced from the fee schedule row, as before.

## Step 7: check the fee schedules

Each anesthesia code you bill a payer must be on that payer's fee schedule
(the administration module's **Fee Schedule**), in the form the payer is
billed: `00731AA`, for example, or `00731AAP1` to `00731AAP4` for a payer
whose schedule keeps the physical status in the code. The rate prices the
line, but a code that is not on the payer's schedule cannot be billed to it at
all: the claim window does not accept it, and the auto-claim skips the
encounter with `no fee for <code>`.

# At the encounter

## Orders: the procedure

The front desk orders the **procedure** the patient is having, once, on the
encounter's **Orders** tab, as for any order. The *Mp10 Orders* guide
explains how, and its item search finds an item by words, by CPT or by an
abbreviation.

Order the procedure, not a billing variant of it: not `00731AA` or `43239AA`,
just the endoscopy. Order the pre-anesthesia evaluation and the EKG too, when
the practice bills them: the auto-claim leaves them off for the payers that
are never billed them with the procedure.

An institutional practice does not need orders for the anesthesia claim:
MpBill10 builds it from the facility claim (see *The auto-claim for
anesthesia*).

## The Anesthesia tab

With `BILLING / ANESTHESIA-TIME` set to `YES`, the encounter window has an
**Anesthesia** tab, after the others. It is on the professional encounter
window (PrBill10) and on the institutional one (Patients10 and MpBill10),
but not when an encounter is opened from the appointment calendar
(MpXtrmCal10). The window still opens on its first tab.

### Start and stop

| Field | What to enter |
|---|---|
| *Anesthesia start*: *Date* and *Time (hh:mm, 24 h)* | When anesthesia time began, as on the anesthesia record. When you type the time and leave the date empty, the date fills in with the encounter's date |
| *Anesthesia stop*: *Date* and *Time (hh:mm, 24 h)* | When anesthesia time ended. **Leave the stop date empty for the start's day; a stop earlier than the start is then the next day.** Type a stop date only for a case that ends on another day than that |

Times are typed in 24-hour form: `07:42`, `13:05`, `00:35`.

As soon as both times are in, a line under them shows the result, for
example *49 minutes = 4 time units (15 min, rounded up)*. A time unit is 15
minutes, rounded up: 15 minutes is 1 unit, 16 minutes is 2.

A case from 23:50 to 00:35 needs no stop date: the stop is earlier than the
start, so it is the next day, and the line reads *45 minutes = 3 time units
(15 min, rounded up)*.

**Clear** empties both times. It does not change the physical status.

### What Save refuses and what it asks

When you save the encounter, Mp10 checks the times. It **refuses** to save,
and shows the Anesthesia tab, when:

- only one of the two is filled in: *Enter both the anesthesia start and
  stop, or neither.*
- the stop is not after the start: *The anesthesia stop must be after the
  start.*
- a time is not a valid hour and minute: *The anesthesia time is not a valid
  hh:mm.*

It **asks** before saving, and you answer Yes to save as it is, when:

- the anesthesia time is over 12 hours (*Anesthesia time is 13h 05m. Save
  it?*);
- the start is on another day than the encounter (*Anesthesia starts on …;
  the encounter is …. Save it?*).

It asks only when the times have changed since they were last saved, so an
answer you have given is not asked again on every save.

### Physical status

Under the times, **Physical status** has seven choices; one is always
selected:

| Choice | Means |
|---|---|
| *Not recorded* | No status recorded (the starting choice) |
| *P1 - Healthy* | P1 |
| *P2 - Mild systemic disease* | P2 |
| *P3 - Severe systemic disease* | P3 |
| *P4 - Severe disease, constant threat to life* | P4 |
| *P5 - Not expected to survive without the operation* | P5 |
| *P6 - Brain-dead donor* | P6 |

It is saved with the encounter. Picking *Not recorded* and saving removes a
status recorded earlier.

### When to fill it in

Fill in the tab **before the claim is created**. A claim line takes the times
and the status when the line is added; lines already on a claim do not change
by themselves when the encounter is corrected later (see *Lines already on a
claim*).

If the encounter window says *The anesthesia data could not be read; the
Anesthesia tab is not shown.*, Mp10 could not read what is stored. The tab is
left out so that nothing stored is overwritten. Close the encounter and open
it again; if it keeps happening, tell your administrator.

# On the claim

This chapter is about the claim window, in PrBill10 and MpBill10, when you add
an anesthesia line yourself. It applies to anesthesia codes, 00100 to 01999.
A surgical code billed with AA, at one unit, is left alone.

## Units from the time

When you type or pick an anesthesia code on a line, and the claim's encounter
has its start and stop, the line's units (the *Unts* column) are filled from
them instead of 1:

> units = time units, plus the CMS base units when the payer's rate says its
> units include the base units

where the time units are the minutes divided by 15, rounded up. The physical
status never adds to *Unts*; where a payer counts it, it is added in the
price (below).

**Hover over the *Unts* cell** to see how the number was reached, for example
*anesthesia 07:42-08:31, 49 min = 4 time units + 5 base*. The text shows only
while the line still has those units: once you type another number, it is
gone.

**The button in the *Unts* cell** recalculates the line from the encounter's
times now, and prices it again. Use it after the times were entered or
corrected on the encounter. It tells you why when it cannot:

| Message | Meaning |
|---|---|
| *This encounter has no anesthesia times; enter them on the encounter's Anesthesia tab.* | Record the times first |
| *Not an anesthesia code.* | The line is not an anesthesia code |
| *no CMS base units for 00731* (for example) | The payer's units include the base units, and that code's base units are not loaded |
| *no anesthesia rate for …: time units only* | The units were set to the time units; the payer has no confirmed rate, so Mp10 cannot know whether its units include the base units |

You can always type the units yourself: what you type wins.

The button, the fill and the hover need `BILLING / ANESTHESIA-TIME` set to
`YES`. With it off, the *Unts* cell works as it always did.

## The physical status on the code

When you add an anesthesia code without a physical status, and the encounter
has one recorded, Mp10 adds it as the code's last modifier, **for the payers
that bill one**: a payer that has billed a physical status on at least 1 in 5
of its anesthesia lines in the last 24 months (`AUTOCLAIM-HISTORY-MONTHS`),
the same rule as the *Physical Status Missing* tag. For any other payer the code is left as you typed it.

- `00731AA` on a P2 encounter, for such a payer, becomes `00731AAP2`.
- If the payer's fee schedule has the code with the status (`00731AAP2`),
  that row is the one used for the line. If it has only the code without the
  status, that row is used and the status is still added.
- **A status you type is kept.** Type `00731AAP3` and the line stays P3,
  whatever the encounter says.
- A code that already carries a status keeps it, even when it was picked from
  the list with one.

The status comes first, then the units, then the price, so a payer that adds
units for P3 to P5 is priced with the right status.

## The price

For a payer with a confirmed **Anesthesia Rate**, the line is priced by the
formula whenever you add the code or change its units or modifiers:

> price = (units + base units + P units) × rate

- **Base units** are the code's CMS base units for the date of service,
  counted as 0 for a payer whose units already include them.
- **P units** are added only for a payer whose rate is set to add them: P3
  adds 1, P4 adds 2, P5 adds 3.
- **Rate** is the payer's dollars per unit in force on the date of service.

**Hover over the Fee cell** to see how the price was made, for example
*(5 base + 4) x 22.0300, rate of 2026-01-01*. The text shows only while the
price is still the formula's. You can still type over the price. The *Patients, Billing and Payments* guide, section *Anesthesia price*,
is the reference for the formula.

A payer with no rate is priced from its fee schedule row, as before: changing
the units then scales *Expected* and the deductible, and leaves *Fee* as it
was. Check *Fee* yourself.

## Worked examples

The numbers below are examples. Use your own payers' rates.

**A 49-minute case.** Code 00731 (CMS base units 5), anesthesia from 07:42 to
08:31: 49 minutes, 4 time units. The physical status on the encounter is P2.

| Payer (example) | How it counts units | The line Mp10 fills | Units | Price |
|---|---|---|---|---|
| MCS Classicare: bills a physical status, units are time only, 22.03 per unit | Time only | `00731AAP2` | 4 | (5 + 4) × 22.03 = **198.27** |
| Triple-S Advantage: no physical status, units include base, 20.51 per unit | Time + base | `00731AA` | 9 (4 + 5) | 9 × 20.51 = **184.59** |

**A case past midnight.** Anesthesia from 23:50 to 00:35: 45 minutes, 3 time
units. The first payer above is billed 3 units, the second 8 (3 + 5).

**A payer that adds units for the status.** If the first payer's rate were
set to add P units, a P3 encounter would give `00731AAP3`, still 4 units, and
a price of (5 + 4 + 1) × 22.03 = 220.30.

## Lines already on a claim

Lines already on a claim **never change by themselves** when the times or the
status are entered or corrected on the encounter later. That is deliberate:
a claim you have checked does not change behind your back.

- **To bring the units up to date,** click the button in the line's *Unts*
  cell.
- **To bring the physical status up to date,** type the code again without
  the status (for example `00731AA`): the line takes the encounter's status,
  and its units and price are filled again.

# The auto-claim for anesthesia

The auto-claim is described in the *Patients, Billing and Payments* guide,
chapter *Creating a claim*, section *Auto-claim*, and in its section
*Anesthesia practices*. This chapter summarises what it does with an
anesthesia case.

## What it does

In PrBill10, highlight encounters in the **Unbilled encounters** list and
click **Auto-claim** in the side panel. Only BillingUsers and Administrators
see it. For each encounter, Mp10:

1. **Works out the code to bill from the ordered procedure.** For the claim's
   payer, it uses the code the payer's own reviewed claims were billed for that
   procedure: the anesthesia code with AA, the surgical code with AA, or the
   surgical code alone. The anesthesia code comes from the item's code saved in
   **Anesthesia Codes**.
2. **Leaves off what the payer is not billed.** A consult or an EKG is left off
   when the payer was billed it on fewer than half of its anesthesia claims.
3. **Adds the physical status** recorded on the encounter, for a payer that
   bills one (the 1-in-5 rule above). It replaces any status the payer's
   history would have suggested. Without a status on the encounter, the code
   is built without one, and the claim is tagged **Physical Status Missing**
   for such a payer.
4. **Sets the units.** With the anesthesia start and stop on the encounter,
   the units come from the time, exactly as in the claim window, and **no tag
   is added**. Without them, the units are estimated from the units the payer
   usually bills for that code, and the claim is tagged **Anesthesia Units
   Estimated**.
5. **Prices the line** with the payer's anesthesia rate; without a rate, at
   the payer's usual price for those units in its history; failing that, the
   fee row times the units.
6. **Adds the diagnoses** on the encounter, those the report confirms, and,
   when the practice has set it up, one diagnosis from history.
7. Runs the same checks as when you save a claim, and saves the claim with the
   tag **Auto-created - review**.

The auto-claim uses the times and the status wherever they are recorded.

**Only one anesthesia line takes its units from the time.** On a claim with
two anesthesia lines, the first takes the time; the second keeps the
estimate and its tag, so the same minutes are never billed twice. Its
explanation says *anesthesia time already used by* the first line's code.

**MpBill10.** In an institutional practice the auto-claim builds the
anesthesia claim from the encounter's **facility claim**, not from orders: the
facility claim's surgical lines are taken to their anesthesia code (the code
saved in **Anesthesia Codes**, else the one most often paired with it in
history), and the claim has one anesthesia line. The payer is the encounter's
insurance, taken to its anesthesia payer through the history. The times, the
status, the units and the price work as above. The *Patients, Billing and
Payments* guide explains this path under *MpBill10: the anesthesia claim from
the facility claim*.

## Reviewing what it built

Every auto-created claim waits for a person. Open **Auto-claims to review**
(the toolbar button after **Unbilled encounters**, or *Tables → Auto-claims to
review*). The Billing dashboard's *Needs attention* card also counts the
auto-created claims **you** made that nobody has reviewed; click that line to
open the list on just those claims.

For each claim:

1. Open it. Check the payer, the code on each line and its modifiers.
2. Check the units against the anesthesia record. Hover over the *Unts*
   cell: when the units came from the encounter's times, it says how.
3. Check the price. Hover over the *Fee* cell for the formula.
4. Check the diagnoses.
5. Deal with each tag (next chapter).
6. Remove **Auto-created - review** with its **x**, and click **Save**.

Only claims reviewed this way teach the auto-claim; see *Auto-claims to
review* in the *Patients, Billing and Payments* guide.

# The tags you will meet

A tag is a coloured label at the bottom of the claim window. Hover over it to
see what it found; click it for the full explanation. The full list of checks
is in the *Mp10 Dashboard* guide, chapter *The claim checks*.

## Anesthesia Units Estimated

**What it means.** The auto-claim estimated the units of an anesthesia line
from the payer's past claims, because it could not calculate them from the
time.

**Why it appeared.** One of these:

- the encounter had no anesthesia start and stop;
- the payer's units include the base units, and the code's CMS base units
  are not loaded (the explanation ends *anesthesia time not used: no CMS base
  units for …*);
- the line is the claim's second anesthesia line (*anesthesia time already
  used by …*).

The explanation gives, for each estimated line, the units, the range the
payer's history shows (for example *4 units (history 2-10)*), and where the
price came from: the payer's rate, the history, or the fee row times the
units.

**What to do.** Check the units against the anesthesia record and correct
them; with a rate, the price follows. If the encounter is missing its times,
record them on the Anesthesia tab, then click the button in the *Unts* cell.
Remove the tag with its **x** and save. This tag does not go away by itself:
it is cleared by the person who checked the units.

## Physical Status Missing

**What it means.** An anesthesia-code line has no physical-status modifier,
and the claim's payer is one that bills it: it has billed a physical status
on at least 1 in 5 of its anesthesia lines. The explanation names the line
and the payer's share.

**Why it appeared.** The encounter had no physical status recorded when the
line was added, or the line was typed with a code that did not get one.

**What to do.** Add the modifier to the code (for example `00731AAP2`) and
save; the tag clears by itself on that save. Where the Anesthesia tab is in
use, the explanation adds *Record it on the encounter's Anesthesia tab:
anesthesia lines added after that get it.* Recording it there helps the next
lines and the auto-claim; it does not change the lines already on the claim.

This check runs on every claim you save, made by hand or by the auto-claim. A
practice whose payers never bill a physical status never sees it. It counts
in the dashboard's claims with error tags.

## ICD10 From History

**What it means.** The auto-claim added the encounter's only diagnosis from
the history of claims billing the procedure, because there was no report.
The explanation gives the code and how many past claims it came from.

**What to do.** Check the diagnosis against the chart, correct it if needed,
remove the tag with its **x** and save. It appears only where
`AUTOCLAIM-ICD10-HISTORY-MIN` is turned on.

## Auto-created - review

**What it means.** The auto-claim made this claim and nobody has checked it
yet.

**What to do.** Review the claim as in *Reviewing what it built*, then remove
the tag with its **x** and save. The tag does not stop a claim from being
sent. In PrBill10, tick **Hide unreviewed** in the Claims grid before
sending, to keep unchecked claims out of the 837.

# Daily checklist {.unnumbered}

1. **Front desk:** on each encounter, order the procedure (and the evaluation
   and EKG, if billed).
2. **After the case:** fill in the Anesthesia tab from the anesthesia record:
   start, stop and physical status. Save.
3. **Billing:** run **Auto-claim** on the day's encounters in the Unbilled
   encounters list, or add the anesthesia line by hand and let Mp10 fill the
   units, the status and the price.
4. Open **Auto-claims to review** (or the dashboard line for your own claims).
5. On each claim, check units, code and modifiers, price and diagnoses
   against the record; deal with every tag.
6. Remove *Auto-created - review* and save.
7. Send (in PrBill10, with **Hide unreviewed** ticked).
8. **When a payer's rate changes:** add a dated row in **Anesthesia Rates**.
