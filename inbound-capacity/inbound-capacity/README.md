# Inbound Slot Capacity Check

A one-page dashboard that checks whether each inbound slot (Slot 1, 2, 3) at each warehouse breaks the capacity rules, works out which vendors to move to another slot on the same day, flags vendors that have no caphand slot, and writes a mini analysis you can download as a Word file.

## Data

- The page reads `data/capacity.xlsx` from this folder when it opens, so everyone with the link sees the same data. If the file is not there, upload it inside the page (that only changes your own screen).
- The PO schedule tab needs: Capacity Handler Date, slot name, loading_dock_name, po_number, status, incoming_qty, Vendor ID, vendor, destination_name, max_timeslot_po_qty. Optional: start_at, end_at, destination_id, PO Created Date.
- A historical FR tab (vendor_id, company_name, and one column per month such as `Sep 2026`) can sit in the same Excel file. The right-most month is used.

## Historical FR

- Expected qty = incoming_qty × the vendor's historical FR. All slot checks use expected qty.
- The page ships with FR Sep 2026. Each month, upload or paste the new FR table in the **Historical FR** tab.
- Vendors that are not in the FR table count at 100% by default (changeable in the same tab). FR can also be switched off.

## Rules

- Capacity per slot = `max_timeslot_po_qty` (taken once per date × slot × warehouse, never added up).
- A slot is full when it is above 100%.
- If Slot 1 or Slot 3 is above 100%, Slot 2 must stay at or below 100%. If Slot 2 is above 100%, Slot 1 and Slot 3 must stay at or below 100%.
- So each day uses one option: **Fill Slot 1 and Slot 3** (Slot 1 up to 125%, Slot 3 up to 133%) or **Fill Slot 2 only** (up to 133%).
- Limits and links can be changed in the **Rules** tab.

## How vendors are moved

1. On a day that breaks the rules, every option is worked out and the one that leaves the least qty without a slot is used. Another option can be picked per day in Recommendations.
2. Vendors are taken out from the bottom of each full slot (latest PO Created Date first, then the bottom row). All POs of the same vendor in that slot are taken out together.
3. A taken-out vendor moves to another slot on the same day only if that slot has room within its limit.
4. A vendor with no room anywhere that day stays in its slot and is flagged as **No caphand slot**. That day keeps breaking the rules.

## Mini analysis

The **Analysis** tab has the Data Scope / Key Findings / Recommendations summary, the decision framework, a per-day table, the vendors to move, the vendors without a caphand slot, and before/after calendars. Use **Download mini analysis (.docx)** or **Copy summary text**.

## Sharing

**Copy share link** keeps the warehouse, tab, status filter, rule settings, chosen option per day, an uploaded FR table and every approved move.

## Updating the data

1. Download the Google Sheet as Excel (.xlsx) and rename it to `capacity.xlsx`.
2. In GitHub, open `inbound-capacity/inbound-capacity/data/`, choose **Add file → Upload files**, drop the file in, and commit.
