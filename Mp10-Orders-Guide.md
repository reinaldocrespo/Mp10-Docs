---
title: "Mp10 Orders — User's Guide"
subtitle: "Recording what the patient came for, and everything that follows from it"
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

An **order** records what a patient is in for: the study, test or procedure,
picked from your practice's catalogue and attached to the patient's
encounter. An encounter can carry one order or several.

This guide is for everyone an order touches: the front desk or admissions
staff who enter them, the billers who use them, and the administrator who
sets up the catalogue and the interfaces. It explains why orders are worth
entering, how to enter them, how they reach the biller, how they print as a
label, how they can be sent by HL7 to other systems, and how results coming
back are matched to them.

Some practices enter orders on every encounter because they must: their
imaging or reporting systems learn about each study from Mp10's orders.
Others do not enter them at all. The next chapter makes the case that every
practice should.

# Why enter orders

Entering an order takes a few seconds at registration. It can feel like extra
typing nobody asked for. It is not, because of what happens to it afterwards.

- **The claim starts itself.** When a biller creates a claim for an encounter
  that has orders, in the professional billing module or in MpWeb, the
  claim's procedure lines are already filled in from them: the procedure
  codes and the date of the visit. Without orders, every line is typed from
  scratch, from whatever paperwork reached the biller.
- **The biller knows what the patient came for.** The order is the record of
  the visit's purpose, available to whoever bills it, wherever they sit. A
  biller working remotely, in MpWeb or in the desktop modules, has the same
  information the front desk had, without a phone call or a scanned sheet.
- **Unbilled visits say what is waiting to be billed.** The unbilled
  encounters list shows, beside each patient, what they came for, so the
  billing backlog reads as work, not as a list of names.
- **A label prints from it.** Each order, or the encounter as a whole, prints
  a label naming the patient and what they are having done: for envelopes,
  specimen bags, film jackets or folders.
- **Mistakes are caught early.** When an order is saved, Mp10 warns if the
  same study was ordered twice, or if the patient's insurance has already
  refused that procedure for this patient. Both are far cheaper to fix at
  the front desk than as a rejected claim weeks later.
- **Other systems can be told.** Orders can be sent by HL7 to an imaging or
  reporting system, and listed on Mp10's own modality worklist so the
  scanners know who is next. Results sent back are filed against the order
  they answer.

None of this happens for an encounter without orders. Billing it still works
exactly as before; it is simply slower, and blind to everything above.

# Setting up the order catalogue

Orders are picked from a catalogue of **order items**, which the
administrator maintains in **Admin10 → Order Items**. Each item has:

- an **item code** — what the front desk types or picks. It can be the
  procedure code itself (`72040`) or a code of your own (`IT18-00000136`).
- a **description** — what everyone sees, for example `NECK SONOGRAM`.
- a **CPT**, with modifiers when the item is side-specific (`73718 LT`).
  This is what the claim is built from, so it must be right.
- a **body part**, used to tell apart studies that share a CPT.
- a **revenue code**, which also decides the modality on the worklist (US,
  CT, MR and so on).

Several items may share one CPT when they are different studies: a neck and
a thyroid sonogram are both 76536, and a wrist, elbow and biceps ultrasound
are all 76881 RT. Give each its own body part, and Mp10 will not mistake
them for duplicates.

# Creating orders

Orders can be entered in three places. All three write to the same place, so
an order entered in one shows in the others.

## In the Patients module (Patients10)

This is where orders are normally entered, as the patient is registered.

1. Open the patient's **encounter** and save it if it is new. Orders can only
   be added to an encounter that has been saved.
2. Choose the **Orders** tab.
3. Choose **New Order** (or press **Insert**). A new row opens.
4. In the **Item** cell, type the item code, or choose the button in the
   cell to pick from the catalogue. The pick list shows the item, its
   description, body part and modality; typing a code that is not in the
   catalogue opens the same list so you can choose.
5. Repeat for each study the patient is having.
6. **Save the encounter.** The orders are saved with it.

To take an order off, select it and choose **Delete Order** (or press
**Delete**), then save the encounter.

When the encounter is saved, Mp10 may show one of two messages about the
orders you just added. Neither stops the save; the orders are already
written.

- **Duplicate Order** — the same study is now ordered more than once on this
  encounter: the same item twice, or two items with the same procedure code
  on the same body part (for example LEFT HIP 2-3 VIEWS and LEFT HIP AP AND
  FROG). Right and left are different studies, and so are different body
  parts sharing a code, so those never warn. If one order is extra, delete
  it and save again.
- **Refused For This Patient** — the patient's insurance has already refused
  this procedure **for this same patient**, and has not paid it since. The
  message names the date and the payer's own reason, such as a missing
  authorization or credentialing. Check what the reason names before the
  service is billed. The claim created later carries the same warning as a
  tag.

The same two messages appear wherever orders are entered: on this tab, in
the Billing module's orders window, and in MpWeb.

## From the Billing module

A biller can add or correct orders without leaving billing. In the
**Unbilled Encounters** grid, an encounter that has orders shows an orders
icon beside its date; choose the icon, or **Orders** in the task panel, to
open the encounter's orders.

The window lists the catalogue on the left and the encounter's orders on the
right. Select an item and choose the right arrow to add it, or select an
order and choose the left arrow to remove it, then save. The filter button
narrows a long catalogue. Saving gives the same duplicate-order and
refused-for-this-patient messages as the encounter's Orders tab.

## In MpWeb

Billers and staff working remotely can manage orders in the browser. Open
the **Orders** screen from the patient's Encounters tab, the Encounters
list, or **Manage orders** on the encounter itself. The catalogue is on the
left and the encounter's orders on the right; the arrows move items across,
and nothing is saved until you choose **Save orders**. If the save added a
duplicate study, or a code the payer has refused for this patient, the
dialog says so in an amber box above the lists; the orders are saved either
way. The **Mp10 Web — User Guide** describes the screen in full, including
why an order that already has a result cannot be removed.

# How orders reach the biller

## The claim is pre-filled

When a biller creates a **new claim** for an encounter in the professional
billing module (PrBill10), Mp10 adds one procedure line for each code the
encounter's orders carry, with the visit date as the service dates and one
unit. The biller completes the rest: diagnoses, pointers, modifiers the order
did not carry, and the charges. A code ordered twice still makes one line.
The institutional billing module (MpBill10) does not pre-fill claim lines
from orders; its billers still see the orders on the encounter.

MpWeb's new-claim screen does the same, and also prices the lines from the
payer's contract. It leaves out any order already billed on another claim of
the same encounter, so a second claim never repeats the first one's lines.

## Unbilled encounters show what was ordered

- The **Unbilled Encounters** grid has an **Ordered** column, right after the
  patient's name, listing what each patient came for. A study ordered twice
  shows as, for example, `CERVICAL SPINE XR x2`, so a duplicate is visible at
  a glance. The orders icon beside the date opens the encounter's orders.
- MpWeb's **Encounters** list has the same **Ordered** column. If you had
  arranged that grid's columns before, the new column may appear at the far
  right; drag it where you want it, or use **Reset layout**.
- The **Unbilled** report in the Billing module's reports lists, for each
  unbilled encounter, every item ordered with its procedure code and
  description, and the first lines of any transcription.
- The dashboard's **Today** card counts the orders entered on today's
  encounters, so a day with visits and no orders stands out.

# Printing labels

Two labels print from orders:

- **The order label.** On the encounter's **Orders** tab, select a saved
  order and choose **Print Label**. It prints the patient, the encounter,
  the doctor and that order, and lists every test ordered on the encounter.
  The Billing module's orders window has the same button, **Print Order
  Label**. An order must be saved before its label can print.
- **The encounter label.** On the encounter window, **Print label with
  encounter** prints the encounter label, which also lists the tests
  ordered.

Use them on envelopes, specimen bags, film jackets or folders: anywhere the
patient's name and what they are having done must travel together.

Each label opens as a preview; print it from there to the label printer.
The layouts are report forms your reseller can adapt.

# Sending orders out by HL7

Mp10 can tell another system — an imaging system, a reporting service, an
interface engine — about each order as it is entered, using standard HL7
**ORM** (order) messages. When an order is deleted, a cancel message goes
out instead.

## How it works

1. When an order is saved, a database trigger queues a message for it. When
   an order is deleted, the trigger queues a cancel, unless a result has
   already come back for that order; then the delete is refused, because
   the order is part of the clinical record.
2. The **HL7 client service** picks up the queue, builds each message from
   the patient, the encounter, the doctor and the order item, and writes it
   as a file into a folder.
3. The receiving system, or an interface engine in front of it, collects the
   files from that folder.

The message carries Mp10's **order number** (for example `OR26-00052073`)
in the ORC segment. That same number is how results coming back are
matched to the order; see the next chapter.

> **Files, not a network connection.** The service writes each message to
> a file in the folder named below. Sending straight to a network address is
> not active in the current build, so the receiving side must collect the
> files. Without a folder set, nothing is delivered.

## Turning it on

All settings are in **Admin10 → Global System Registry**. Note that the two
sections are spelled differently: one has a dash after HL7, the other does
not. Copy them exactly.

| Section | Entry | Value |
|---|---|---|
| HL7ORM-TRANSMIT | ACTIVATE | YES |
| HL7-ORM-TRANSMIT | FILEPATH | The folder the receiving system collects from, for example `D:\HL7\Outbound\` |

Then:

1. **Create the folder** and make sure the account the HL7 client service
   runs as can write to it, and the receiving system can read from it.
2. **Install the triggers.** In Admin10, run **Update Data Dictionary**
   (in the **Actions** panel). Among its steps it installs the two triggers
   that queue a message whenever an order is added or deleted. Run it with
   the other modules closed. Orders entered before this step are not
   queued.
3. **Install and start the HL7 client service** on the server — an
   **IT task**. From a command prompt in the Mp10 folder, run
   `MpHl7.exe I` once to install it, then start **Structured Systems HL7
   Client Service** from Windows Services. `MpHl7.exe U` uninstalls it.
4. **Restart the service** after changing any of the settings above; it
   reads them when it starts.

## Checking it is working

1. Add an order to a test encounter and save it.
2. Within a couple of minutes a new message file appears in the folder.
3. The service's log, `MpHl7.out` beside `MpHl7.exe`, records each message
   written and the file it went to.

If no file appears, check in this order: the service is running; ACTIVATE is
YES under **HL7ORM-TRANSMIT**; FILEPATH is set under **HL7-ORM-TRANSMIT** and
the folder exists; Update Data Dictionary has been run since ACTIVATE was set.

# Results coming back

When the receiving system finishes a study — the radiologist reads the
images, the report is signed — it can send the result back to Mp10 as an HL7
**ORU** (result) message. Mp10's **HL7 server**, the **MpSocketServ**
service, receives it and files it against the order.

## How a result finds its order

1. The sending system connects to MpSocketServ on the address and port
   configured for it, and sends the result.
2. MpSocketServ stores the message as it arrives, then acknowledges it, so
   nothing received is ever lost. A message sent twice with the same
   message id is acknowledged again but stored once.
3. It reads the **order number** from the message's ORC segment — the same
   Mp10 order number that went out in the order message — and matches it
   to the order.
4. The result becomes a **transcription** on that order's encounter, with
   the ordering and performing doctors, and the order is now part of the
   clinical record: it can no longer be removed from the encounter.

The order number is the only link. A result whose order number Mp10 does not
recognise, or that carries none, is stored but not filed, and waits; after
repeated attempts it is set aside as unprocessable, so it does not retry
forever.

## Where results appear

- In **MpWeb**, select the encounter and choose **Results** to view or
  print it.
- In the desktop modules, the transcription is on the encounter, and the
  Unbilled Encounters grid shows an icon beside the patient's name on
  encounters that have one.

## The settings

The HL7 server's settings are in the **HL7-SERVER** section of the System
Registry. Setting it up is an **IT task**, done once with whoever runs the
sending system.

| Entry | Default | Meaning |
|---|---|---|
| IP | 0.0.0.0 | The address MpSocketServ listens on; 0.0.0.0 means every address of the server |
| PORT | 6261 | The port the sending system connects to |
| ACK_EXPECTED | YES | Reply to each message with an HL7 acknowledgement |
| ACK_SENDING_APP | MP10 | The name Mp10 gives itself in its acknowledgements |
| ACK_SENDING_FACILITY | empty | The facility name in its acknowledgements |
| MAX_PROCESS_ATTEMPTS | 5 | Attempts to file a result before it is set aside |
| RAW_JOURNAL_DAYS | 7 | Days the raw incoming messages are kept. They contain patient data |

# The modality worklist

Imaging scanners can ask Mp10 who is next. AutoTasks turns the day's imaging
orders into a **modality worklist** that the scanners query, each seeing its
own studies. The modality comes from the order item's revenue code.

There is nothing extra to enter per order: an order on an open, active
encounter within the worklist's date window is on the list. Setting up the
worklist server and the scanners is described in the **Mp10 Modality
Worklist** guide, and its AutoTasks settings in the **Mp10 AutoTasks** guide.

# Quick reference

| You want to | Do this |
|---|---|
| Add an order at registration | Encounter → **Orders** tab → **New Order** → pick the item → save the encounter |
| Add an order while billing | Unbilled Encounters → orders icon or **Orders** → move the item across → save |
| Add an order remotely | MpWeb → encounter → **Manage orders** |
| Remove an order | Select it → **Delete Order** → save. An order with a result cannot be removed |
| Print what the patient is having done | **Print Label** on the Orders tab, or **Print label with encounter** |
| Get a claim pre-filled | Enter orders before the claim is created (PrBill10 and MpWeb) |
| Send orders to another system | Set ACTIVATE and FILEPATH, run Update Data Dictionary, install and start the HL7 client service |
| Receive results | Set up MpSocketServ with the sending system; results file against their order number |

Structured Systems · <reinaldocrespo@structuredsystems.com> · 954-744-0286
