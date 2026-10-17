---
title: "Mp10 Dashboard — User's Guide"
subtitle: "The cards on the Billing and Patients main windows, and what to do about what they show"
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

The Billing module (PrBill10 or MpBill10) and the Patients module (Patients10)
open on a **dashboard**: a set of cards drawn on the main window that tell you,
at a glance, what needs your attention today. Claims a check has flagged,
encounters that were never billed, rejections waiting to be worked, a backup
that did not run — each of these used to be something you had to go looking
for. The dashboard puts them in front of you as soon as you sign in.

This guide is for the people who use those modules every day: billers, front
desk and admissions staff, and the record room. It explains what each card
means, which lines you can click, and what to do about what they show.

Two parts deserve particular attention, and each has its own chapter:

- **Claims with error tags** — how to open a flagged claim and find out exactly
  why it was flagged. The chapter after it, **The claim checks**, lists every
  check and the tag it places.
- **Unbilled encounters** — what they are, why they matter, and how to clear the
  ones that will never be billed because the patient did not show up.

# Using the dashboard

## When it appears

The dashboard shows whenever **no grid and no option window is open** on the
main window. Open the Patients grid, the Claims grid, or any other window, and
the cards step aside; close it, and they come back.

You do not have to close your work to see the dashboard. Press the
**Dashboard** button on the toolbar (or choose **Dashboard** from the menu).
Every open grid is hidden — not closed — and the cards appear. Each grid comes
back exactly as you left it when you press its own toolbar button again.

## Which cards you see

Each card reads certain tables. If your user account is not allowed to read
one of them, that card (or that line of a card) is simply left out. Two people
at the same site can therefore see different dashboards; that is expected and
is not a fault. If you believe you should be seeing a card you do not, ask your
administrator to review your permissions.

Some lines also depend on what your site uses. The HL7 line appears only at a
site that runs an HL7 interface, the modality worklist line only where the
worklist is installed, and so on.

## How fresh the numbers are

The numbers are gathered in the background, so the dashboard never slows down
the window you are working in. They refresh:

- about every **five minutes** while the dashboard is showing, and
- every time you **come back** to the dashboard after working in a grid, so
  what you just changed is reflected.

When you first sign in, a card may say **Loading…** for a few seconds.

If a card's bottom line turns orange and reads **"Numbers as of hh:mm:ss · the
refresh has stopped"**, the numbers on screen are old. Close and reopen the
module. If it keeps happening, tell your administrator.

A card that shows a **red message** instead of numbers could not read its data.
The other cards are not affected. Report the message text to your
administrator.

## Colours

The colours follow one rule throughout:

| Colour | Meaning |
|---|---|
| **Grey** | Zero, or nothing to do |
| **Green** | All clear, or good news (paid claims, nothing outstanding) |
| **Amber** | Something is waiting for someone |
| **Red** | Something has gone wrong and needs action |

## Clicking

There are two kinds of click:

- **Click anywhere on a card** to open the grid it is about — for example the
  Encounters grid from the *Needs attention* card.
- **Click an underlined line** to open the list behind that one number. When
  the mouse is over such a line the pointer turns into a pointing finger. A
  line is underlined only when its count is above zero: there is nothing to
  open behind a zero.

A list opened from a line (for example *Claims with error tags*) belongs to the
dashboard. It hides when you open a grid and comes back, where you left it,
when you return to the dashboard.

# The Billing dashboard

The Billing module shows up to four cards, followed by two charts when there is
room on the screen.

## Today

The number of encounters dated today, one line per site, and at the bottom how
many of today's encounters are still unsigned and how many orders were placed.

On a day with no encounters yet — early morning, a weekend — the card says so
and shows the last day that had activity instead, so you are never looking at a
bare zero wondering whether something is broken.

Clicking the card opens the Encounters grid.

## Needs attention

The big number is the count of **encounters in the last 7 days that have no
patient signature**. It is amber while anything is outstanding and green at
zero. The bottom line shows the date of the oldest one still waiting.

Below it, depending on your permissions, two more lines:

- **Possible duplicate patients · new in 90 days.** Groups of patient records
  that look like the same person (same last name, first name, birth date and
  city) where at least one record was created in the last 90 days. Click the
  line to see the list. Nothing is merged or changed from that list; it tells
  you who to look at.
- **Claims with error tags · last 7 days.** See the next chapter.

Clicking the card itself opens the Encounters grid.

## Billing

This card covers the **last 90 days** of active encounters:

- **Unbilled encounters** (amber) — encounters with no claim. Click the line to
  open the Unbilled Encounters grid on those same 90 days. See the chapter
  *Unbilled encounters*.
- **Rejected claims** (red) — claims whose most recent status is a rejection.
  Click the line to open the list.
- **Paid claims** (green).
- **Claims with no status** — claims sent but with no response recorded yet.
- **of N encounters · last 90 days** — the total these are counted from.
- **Upcoming encounters pre-created** — encounters already entered for future
  dates. They are shown for information and are **not** counted as unbilled.

The bottom line gives the oldest unbilled encounter, the oldest rejection and
the oldest claim with no status. Those dates are the ones to watch: an old date
here is money at risk of passing the payer's filing deadline.

### The rejected claims list

The list shows each rejected claim with the patient, service date, insurance,
charges, the date it was rejected, where the rejection came from and **the
reason**. When the rejection came from the clearinghouse or the payer, the
reason is their own wording. When the claim was denied on a remittance (an
835), the reason is built from the adjustment and remark codes on that
remittance.

Double-click a row to open the claim, correct it, and resubmit. **Refresh**
re-reads the list after you have worked a few; **Print** gives you a paper
copy to work from.

## System health

One line per background service your site uses. A green dot means all is well,
amber means "look at this", red means it has stopped or failed.

- **HL7 interface** — whether the interface is listening, when the last message
  arrived, and how many messages are waiting to be processed. Red if the
  listener has not been heard from, or if no message has arrived since the last
  day the clinic was busy.
- **Modality worklist** — how many entries are scheduled, whether the worklist
  server is up, and when a scanner last asked for its list.
- **AutoTasks** — when the background service last ran a cycle and when the
  last backup was taken. Red if AutoTasks has stopped reporting or the last
  backup **FAILED**; amber if the last good backup is more than two days old.
  AutoTasks also keeps the CMS reference data current and emails what it
  fetched — see **The reference data keeps itself current** in the claim
  checks chapter.
- **Received today** — remittance files, claim statuses and eligibility
  responses that arrived today.

Any red line here should be reported to whoever looks after your server the
same day. A failed backup in particular must not wait.

## The charts

When the window is wide enough, two bar charts follow the cards:

- **Billed vs collected · last 12 months** — what was billed and what was
  collected, month by month.
- **Encounters per month · last 12 months.**

On a smaller screen the charts may not fit; maximise the main window to see
them.

# Claims with error tags

## What the line counts

When a claim is saved, Mp10 runs a set of checks on it — diagnosis codes and
modifiers that do not agree, a payer the patient does not have on file, a date
of birth the insurer reports differently, codes this payer has refused before,
and others. A check that finds a problem puts a **tag** on the claim: a short
coloured label with the name of the problem, such as *Wrong Side ICD10* or
*Insurance DOB Mismatch*.

The **Claims with error tags · last 7 days** line on the *Needs attention* card
counts the claims that carry at least one of these tags and were saved in the
last seven days. Tags that a user added by hand are not counted. Every check,
and the tag each one places, is described in the next chapter, *The claim
checks*.

> In this version the count, and the list behind it, cover the diagnosis,
> payer and denial-history tags. The four fee-schedule modifier tags and the
> three NCCI tags appear on the claim itself but are not yet included in the
> dashboard count.

Each of those claims is likely to be rejected or denied if it goes out as it
is. Working this line before the claims are sent is far cheaper than working
the rejections afterwards.

## Opening the list

Click the line. A list opens with one row per claim, newest first:

| Column | Shows |
|---|---|
| Claim | The claim number |
| Patient | Last name, first name |
| Service date | The claim's date of service |
| Insurance | The payer |
| Tags | Every tag on the claim, separated by semicolons |
| Saved by | The user who last saved the claim |
| Saved | When it was last saved |

The list title says how many claims it holds.

## Opening a claim and finding out why

**Double-click a row** to open that claim, just as it opens from the Claims
grid.

The claim's tags are shown along the tag strip of the claim window. Each tag is
only a name; to see what the check actually found:

- **Hover** the mouse over a tag. A tooltip shows the first finding in one
  short line — for example which line's modifier disagrees with which
  diagnosis. When there is more to say, the tooltip ends with
  **"(click for more)"**.
- **Click** the tag. A window lists **every** finding for that tag, in full.

The explanation is worked out from the claim **as it is now**, not as it was
when the tag was added. So:

- If you have already corrected the problem, the explanation says the check
  finds nothing on the claim any more, and **the tag clears the next time the
  claim is saved**.
- If the explanation still describes a problem, the claim still has it.

## Clearing a tag

1. Read the explanation (hover or click the tag).
2. Correct the claim — the diagnosis, modifier, payer, patient's insurance
   record, or whatever the explanation points to.
3. **Save** the claim. The checks run again; a tag whose problem is gone is
   removed.

Some tags call for judgement rather than a correction. *Denied Before*, for
instance, says this payer has often refused these codes; the claim may be
perfectly right and simply need supporting documentation. In that case the tag
can stay: it is a warning, and the claim can still be sent.

Press **Refresh** on the list after working a few claims to see what is left.
The count on the dashboard updates on its next refresh.

# The claim checks

This chapter lists every check Mp10 runs on a claim, the tag each one places,
what the tag means and what to do about it.

## How a tag is placed

- **The checks run every time a claim is saved** in the claim window. The
  diagnosis-side checks also run as you edit the procedure lines and
  diagnoses, so their tags can appear before you save.
- **Each check starts by removing its own tags** and adds a tag back only if it
  still finds the problem. The tags on a saved claim therefore always describe
  the claim as it was last saved. Fix the problem, save, and the tag is gone.
- **A tag never stops a claim from being saved or sent.** Each is a warning.
  The coder or biller decides, because the claim may be correct and the rule
  may not apply to it.
- **A check places a tag once per claim**, however many lines have the
  problem. Hover over the tag or click it to see every line it is about.
- **Three checks also offer a correction** when you save: the date-of-birth,
  sex and payer-on-file checks. Each asks first and changes nothing unless you
  answer **Yes**.
- **Tags you add by hand are different.** No check removes them. They stay
  until you remove them, and clicking one says it was added by a user.
- **Some checks need reference data**: the ICD-10 code set, the CMS fee
  schedule, the NCCI tables, and the denial history. The first three keep
  themselves current -- AutoTasks downloads each new release from CMS and
  imports it (see **The reference data keeps itself current** below). The
  denial history is built from your own remittances. Where data is missing,
  the check does nothing and places no tag. A missing tag is not proof that
  a claim is clean.

Secondary claims that Mp10 creates automatically after a primary payment are
checked when someone opens and saves them.

## Summary

| Tag | The claim has… |
|---|---|
| Missing ICD10s | No diagnosis codes |
| ICD10 Relation | A line pointing at a diagnosis that is not on the claim |
| Wrong Side ICD10 | RT with a left-side diagnosis, or LT with a right-side one |
| ICD10 Side Unspecified | RT or LT with a diagnosis that leaves the side unspecified |
| Bilateral CPT, One-Side ICD10 | A surgical code with modifier 50 and diagnoses for one side only |
| ICD10 Excludes1 Conflict | Two diagnoses ICD-10-CM says are never coded together |
| ICD10 Sequence | Diagnoses in an order a "code first" instruction contradicts |
| Modifier 50 Not Allowed | Modifier 50 on a code the fee schedule does not allow it on |
| Modifier 50, Units Over 1 | Modifier 50 on a line with more than one unit |
| Modifier 50 With RT/LT | Modifier 50 and RT or LT on the same line |
| RT/LT On Bilateral CPT | RT or LT on a code that already means both sides |
| NCCI Pair, Never Together | Two codes NCCI never pays together |
| NCCI Pair Needs Modifier | Two codes NCCI bundles unless a modifier shows they were separate |
| Units Over MUE | More units of a code than the CMS limit allows |
| Payer Not On Patient | A payer that is not on the patient's insurance record |
| Insurance DOB Mismatch | A date of birth different from the one the payer reported |
| Insurance Sex Mismatch | A sex different from the one the payer reported, or none at all |
| Denied Before | Codes this payer has refused often and recently |
| Refused For This Patient | A code this payer refused for this same patient, with nothing paid for it since |

## Diagnosis codes

### Missing ICD10s

The claim has no diagnosis codes at all. Payers reject a claim without them.
Add the diagnoses and save.

### ICD10 Relation

A procedure line's diagnosis pointer names a diagnosis that is not on the
claim. For example, the line points at diagnosis 3 but the claim has only two.
Correct the pointer on the line, or add the missing diagnosis.

### Wrong Side ICD10

A line carries modifier **RT** (right) and its diagnosis is for the **left**
side, or **LT** with a right-side diagnosis. The modifier and the diagnosis
contradict each other, and payers reject the line. Correct whichever of the
two is wrong.

The check reads the **first** diagnosis the line points at. Diagnoses that
have no side at all are never questioned.

### ICD10 Side Unspecified

A line carries RT or LT, but its diagnosis is the "unspecified side" code of a
family that has right and left codes. The modifier says which side; the
diagnosis should too. Replace it with the side-specific code.

### Bilateral CPT, One-Side ICD10

A **surgical** procedure (codes 10000 to 69999) carries modifier **50**
(bilateral), and every diagnosis the line points at is for one side only.

The bilateral procedure is supported, and no tag is placed, when:

- the line points at a **bilateral** diagnosis, or
- it points at both a right and a left diagnosis, or
- the claim carries the **same diagnosis for the other side** (or its
  bilateral form) anywhere in its diagnosis list, even if the line does not
  point at it.

Diagnostic procedures such as imaging are not checked. On those, a one-sided
finding on a bilateral exam is correct coding.

An administrator can exempt whole diagnosis families from the three side
checks with the **BILLING / ICD10-LATERALITY-EXCLUDE** setting in the System
Registry: a comma-separated list of code prefixes. It is empty by default.
The claim window reads it; the MpWeb overnight re-tag does not, so an
exempted family can still be tagged there.

### ICD10 Excludes1 Conflict

Two diagnoses on the claim are a pair that ICD-10-CM marks **Excludes1**:
conditions that should never be reported together. The classic example is
type 2 diabetes (E11.9) together with type 1 diabetes (E10.9). An Excludes1
note on a category or chapter applies to every code beneath it.

Clicking the tag names the two codes and the note that forbids them. Since
2017 the guidelines allow such a pair when the two conditions are genuinely
unrelated, so this is the coder's decision. Remove the code that does not
belong, or leave both if they are unrelated.

### ICD10 Sequence

A diagnosis carries a **"code first"** instruction naming another diagnosis
that is on the claim, but the claim lists them the other way round. For
example, a rib fracture listed before the associated spinal cord injury, which
should come first.

Clicking the tag says which code should come first. Reorder the diagnoses and
save. The order on the claim is the order sent to the payer.

## Modifiers and the CMS fee schedule

These checks compare a line's modifiers with the **bilateral surgery
indicator** in the CMS Medicare Physician Fee Schedule. AutoTasks imports
each quarter's file automatically; the MpAdmin menu remains for loading one
by hand. A code the fee schedule does not list is not checked, and neither
is indicator 3, on which payers disagree.

### Modifier 50 Not Allowed

Modifier 50 is on a code whose indicator is **0**, **2** or **9**. On those
codes the payer does not accept a bilateral adjustment:

- **0:** the procedure cannot be billed as bilateral.
- **2:** the code already covers both sides.
- **9:** the concept does not apply.

Remove modifier 50. If the procedure was really done on both sides of a code
with indicator 0, bill each side as the payer requires.

### Modifier 50, Units Over 1

A line with modifier 50 carries more than one unit. A bilateral procedure is
one line with one unit. Set the units to 1.

### Modifier 50 With RT/LT

Modifier 50 and RT or LT are on the same line. They are never used together:
use either modifier 50 on one line, or one RT line and one LT line.

This check and the previous one need no fee schedule data, so they work at
every site.

### RT/LT On Bilateral CPT

A line carries RT or LT on a code whose indicator is **2**, meaning the code
itself already describes both sides. Remove the side modifier.

## NCCI edits

The National Correct Coding Initiative (NCCI) publishes which codes cannot be
billed together and how many units of a code are plausible. AutoTasks imports
each quarter's tables automatically; the MpAdmin menus remain for loading
them by hand. Each payer is checked against either the professional or the
facility table, depending on whether the payer is set up as professional.

### NCCI Pair, Never Together

Two codes on the claim form a pair that NCCI never pays together on the same
day, **whatever modifier** is used. One of them should not be on the claim.
Clicking the tag names the pair.

### NCCI Pair Needs Modifier

Two codes on the claim form a pair that NCCI bundles together, unless the
second code is marked as a separate service. No tag is placed when the second
code's line carries one of these modifiers:

```
24  25  27  57  58  59  78  79  91  XE  XS  XP  XU
```

No tag is placed either when one line is RT and the other LT.

If the two services really were separate, add the modifier that documents why.
If they were not, remove the bundled code.

**Both pair checks run only for payers that ask for them.** An administrator
turns them on per payer by ticking **Check NCCI PTP pairs** on the X12 tab of
the insurance company's record. Payers without the tick are never checked for
pairs.

### Units Over MUE

A code carries more units than its Medically Unlikely Edit (MUE), the CMS
limit for one patient on one day. Depending on the code, the limit applies to
each line or to the claim's total for that code. This check runs for every
payer.

Bilateral billing is allowed for: one RT line and one LT line of the code, or
2 units of a code whose fee-schedule indicator says it may be billed
bilaterally. Correct the units, or check whether the service was entered
twice.

## Payer and patient

### Payer Not On Patient

The claim's insurance, or its secondary insurance, is a payer that does not
appear on the patient's insurance record. A payer counts as on file when the
name matches, or when it has the same payer ID as one the patient has. Claims
like this are rejected far more often than others.

When you save, Mp10 names the payers the patient does have and asks whether to
**add the claim's payer to the patient's insurances**, carrying over the
claim's policy number:

- Answer **Yes** if the patient's record is out of date. Then check the member
  number and effective date on the patient's record.
- Answer **No** if the claim is wrong. Use the magnifier beside the claim's
  insurance to pick one of the payers the patient has.

### Insurance DOB Mismatch

The date of birth the claim will send differs from the date the payer reported
in the patient's latest **eligibility response**. Mp10 prefers a response from
the claim's own payer and says so when the response came from a different one.

When you save, Mp10 shows both dates and asks whether to use the payer's date.
If you answer **Yes**, the date is written to the patient's **insurance record
for that payer**, which is the date claims to that payer send. The patient's
own date of birth on the patient record is **not** changed: the payer's file
can be the wrong one, and the clinical record must stay as it is.

### Insurance Sex Mismatch

The patient's sex on the patient record differs from what the payer reported
in the latest eligibility response, **or the patient record has no sex at
all**. A blank is a real problem: the claim then sends "unknown", and the
eligibility request leaves out the patient's date of birth too.

When you save, Mp10 asks whether to use the payer's answer. If you answer
**Yes**, the patient record is updated, because both claims and eligibility
requests take the sex from there.

### Denied Before

This payer has refused these codes before, **often and recently**. Mp10 learns
this from remittances that paid nothing for a line, and from payer rejections.
Clicking the tag names the code, the diagnosis, how often it was refused and
the payer's own reason.

To avoid crying wolf, the tag is placed only when all of these are true:

| Rule | Default |
|---|---|
| Refusals of the same payer, code and diagnosis | 2 |
| Refusals on at least this many different check dates | 3 |
| Refusals as a share of all this payer's remittance lines for the code | 20% |
| The most recent refusal is no older than | 6 months |
| Refusals counted from the last | 24 months |

Refusals that are not really a refusal of the service are ignored: patient
responsibility, duplicates, coordination of benefits, contractual
adjustments and reporting-only codes. Remark codes never count on their own.

This tag is evidence, not a rule. The claim may be entirely right this time.
Check that the diagnosis supports the procedure, attach documentation if the
payer usually asks for it, and send the claim.

The thresholds are System Registry settings under **BILLING**, named
**DENIAL-MIN-COUNT**, **DENIAL-MIN-DATES**, **DENIAL-MIN-RATE**,
**DENIAL-RECENT-MONTHS**, **DENIAL-MONTHS** and **DENIAL-IGNORE-CODES**.
**DENIAL-REASONCODES** narrows the warning to a list of denial reason codes;
empty, the default, counts every reason. A site that has not built its
denial history never sees this tag.

### Refused For This Patient

This payer has already refused one of the claim's codes **for this same
patient**, and has not paid that code for the patient since. Clicking the
tag names the code, the date of the refusal, and the payer's own reason —
both halves of it: the adjustment code and the remark that says what was
wrong, such as missing credentialing data or a modifier the payer expects.

This is the strongest warning Mp10 gives about a payer's behaviour. Measured
on real data, a code the payer refused for a patient is refused again about
two times out of three; once the payer has paid it, a later refusal is rare.
That is why a payment clears the tag: if the payer paid the code for this
patient after refusing it, nothing is shown.

Fix what the reason names before sending: an authorization, the provider's
credentialing with that payer, a modifier, or a frequency limit. If the
claim is right as it is, send it — the tag is a warning, as always.

The same check runs **when an order is placed** on an encounter, and a
message names the refused code when the encounter is saved. The
**Mp10 Orders** guide describes it, with the duplicate-order warning that
runs at the same time.

It looks back as far as **BILLING / DENIAL-MONTHS** (24 months by default)
and ignores the same reasons **BILLING / DENIAL-IGNORE-CODES** lists, the
settings Denied Before uses.

## The reference data keeps itself current

AutoTasks, the background service, checks the CMS website once a day. When
CMS publishes a new quarter of the fee schedule or the NCCI tables, AutoTasks
downloads and imports it. When CMS publishes the yearly ICD-10-CM code set,
effective each October 1st, AutoTasks downloads and imports that too, and
writes a report of every code that changed next to the downloaded files.
Nobody has to remember a quarter or a year again; the MpAdmin import menus
remain for loading a file by hand.

After any check that imported, downloaded or failed something, AutoTasks
sends an **email** saying exactly what it did -- a new ICD-10 year arriving
included. A quiet check, when CMS has published nothing new, never sends
mail.

The System Registry entries behind this:

| Entry | Default | Meaning |
|---|---|---|
| AUTOTASKS / FETCH_CMS | YES | NO switches the daily CMS check off entirely |
| AUTOTASKS / CMSFETCH_CHECK_HOURS | 24 | Hours between checks |
| AUTOTASKS / CMSFETCH_PATH | a cms folder beside AutoTasks.exe | Where the downloaded files and the ICD-10 change reports land |
| CMSFETCH / NOTIFY_EMAIL | empty | Who gets the result email; commas between addresses |

With **CMSFETCH / NOTIFY_EMAIL** empty, the email goes to **ERRORREPORTS /
NOTIFY_EMAIL** instead. With both empty, no email is sent and the AutoTasks
log still records everything. AutoTasks must have its outgoing mail (SMTP)
settings configured, as for backup notices.

## Switching a check off, or narrowing it

Most checks have no off switch: they warn, they never block, and a tag costs
nothing when the claim is right. Where a site can turn one off or narrow it:

| Check | How |
|---|---|
| NCCI Pair, Never Together / Needs Modifier | Per payer, **off by default**: tick **Check NCCI PTP pairs** on the X12 tab of the insurance company's record |
| Units Over MUE, the modifier-50 checks | No switch once the tables are loaded. **AUTOTASKS / FETCH_CMS = NO** stops future quarters from loading |
| The three side checks | **BILLING / ICD10-LATERALITY-EXCLUDE**: diagnosis-code prefixes to leave alone (claim window only) |
| Denied Before | The **BILLING / DENIAL-...** thresholds above; a very high DENIAL-MIN-DATES silences it, DENIAL-REASONCODES narrows it |
| Refused For This Patient | No switch. **BILLING / DENIAL-MONTHS** shortens how far back it looks; **DENIAL-IGNORE-CODES** names reasons that never count |
| Everything else | Always on. The DOB and sex checks only speak when an eligibility response exists to compare against |

## Tags you may see on older claims

Earlier versions placed **Missing Right ICD10** and **Missing Left ICD10**.
Those checks were replaced by the side checks above. The old tags are removed
automatically the next time such a claim is saved.

# Unbilled encounters

## What an unbilled encounter is

An **unbilled encounter** is an active encounter dated in the last 90 days that
has **no claim**. Every one of them is one of two things:

- a visit that happened and has **not been billed yet** — revenue the practice
  has earned and not asked for; or
- an encounter that should **not** be billed at all, most often because the
  patient **did not show up** after the encounter had been created.

Encounters created ahead of time for future dates are not counted; the card
lists them separately as *upcoming encounters pre-created*.

## Why it matters

- **Filing deadlines.** Every payer gives a limited time after the date of
  service to submit a claim. An encounter left unbilled past that limit is
  revenue that can no longer be collected. The **"Oldest unbilled since"** date
  at the bottom of the Billing card tells you how close the oldest one is.
- **A clean count is a useful count.** If no-shows are left in the list, the
  number grows with encounters nobody will ever bill, and the real unbilled
  visits hide among them. A billing team that trusts the number to mean
  "visits still to bill" can work it to zero every week.
- **Reports and statistics.** Encounters that never took place inflate visit
  counts, productivity figures and census reports until they are marked
  inactive.

Aim to keep the number low and the oldest date recent. Look at it daily; work
it at least weekly.

## Working the list

Click **unbilled encounters** on the Billing card. The Unbilled Encounters grid
opens on the same 90 days the card counted. Its **Ordered** column says what
each patient came for, from the encounter's orders; the orders icon beside
the date opens them, to check or to add orders while billing. An encounter
with nothing in Ordered had no orders entered, and has to be billed from the
paperwork. The **Mp10 Orders** guide explains why entering orders at
registration speeds billing up. For each encounter:

- **If the visit took place**, bill it as usual. Once it has a claim it leaves
  the list.
- **If the patient did not show up**, or the encounter was created by mistake,
  mark it **inactive** as described below.

## Inactivating a no-show encounter

1. In the Unbilled Encounters grid (or the Encounters grid in the Patients
   module), **double-click the encounter** to open it.
2. On the first tab of the encounter window, tick **Inactive encounter**.
3. **Save** the encounter.

What happens to an inactive encounter:

- It **leaves the unbilled count** and the dashboard's other encounter counts
  on the next refresh.
- It is **kept**, not deleted. Its history, notes and documents remain, and it
  still appears in the patient's record — shown in **grey** in encounter
  lists.
- It is left out of reports that count active encounters.
- **No charges can be posted** to it. If you later find the patient was in fact
  seen, open the encounter, clear **Inactive encounter**, save, and bill it as
  usual.

To see inactive encounters in the Unbilled Encounters grid, tick **Show
Inactive Encounters** in the grid's side panel.

Inactivate only encounters that genuinely will not be billed. An encounter that
is merely late in being billed belongs in the list until it is billed; marking
it inactive would hide revenue the practice is owed.

# The Patients dashboard

Patients10 shows a dashboard for admissions and the record room. All counts
cover **active encounters in the last 90 days** unless the card says otherwise.

## Encounters by Source

The number of encounters in the last 90 days, one line per source, largest
first; smaller sources are grouped as *other*. Clicking the card opens the
Encounters grid.

## Encounters missing something

Of the encounters in the last 90 days, how many have:

- **no ICD-10 coded**,
- **no patient signature**,
- **no insurance**, and
- **no eligibility** — insured encounters for which no eligibility response is
  on file. This line appears only at sites that run eligibility checks.

Each is a gap that will hold up the claim later. The card reads "Nothing
outstanding." when all four are zero. Clicking it opens the Encounters grid.

## Patient records

Of the patients seen in the last 90 days, how many have:

- **no HIPAA signature** on file, and
- **incomplete demographics** — no birth date, a missing address, city or ZIP
  code, or no usable telephone number at all.

The third line, **possible duplicate patients**, counts groups of records that
look like the same person across **all** patients on file. Click it to open the
list. Clicking elsewhere on the card opens the Patients grid.

## Incomplete records

The number of open incomplete records, broken down by how long ago the
encounter took place: 0–7 days, 8–30 days, 31–90 days, and over 90 days (in
red). This card has no 90-day limit — an old incomplete matters more than a new
one.

## Incompletes by physician

The five physicians with the most open incomplete records.

Clicking either incompletes card opens the Incompletes grid, for users who have
access to it.

# Quick reference

| You see | Do this |
|---|---|
| Claims with error tags above zero | Click the line, double-click each claim, hover or click each tag, correct, save |
| Unbilled encounters above zero | Click the line; bill the visits that happened, inactivate the no-shows |
| Old "Oldest unbilled since" date | Work that encounter first: its filing deadline is closest |
| Rejected claims above zero | Click the line, read the reason, double-click to correct and resubmit |
| Possible duplicate patients | Click the line and review the records with whoever maintains patient records |
| Unsigned encounters | Collect the signature before the claim goes out |
| Red line in System health | Tell your server administrator the same day |
| "The refresh has stopped" | Close and reopen the module; report it if it recurs |
| A card you expected is missing | Ask your administrator to check your permissions |
