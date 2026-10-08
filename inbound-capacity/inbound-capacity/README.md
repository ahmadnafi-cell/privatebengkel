# Inbound Slot Capacity Check

A one-page dashboard that checks whether each inbound slot (Slot 1, 2, 3) at each warehouse is over capacity, suggests which vendor to move, and lets you simulate the moves.

## How the shared data works

- The page reads `data/capacity.xlsx` from this folder every time it opens, so everyone with the link sees the same data.
- The file needs two tabs:
  - **Sheet1**: the inbound PO schedule from Capacity Handler (Capacity Handler Date, slot name, start_at, end_at, po_number, status, incoming_qty, Vendor ID, vendor, destination_id, destination_name, max_timeslot_po_qty).
  - **Sheet2**: vendor delivery preference (Vendor ID, inbound days, new_inbound_hours, destination_id).

## How to update the data

1. Download the Google Sheet as Excel (.xlsx).
2. Rename it to `capacity.xlsx`.
3. In GitHub, open `inbound-capacity/data/`, choose **Add file → Upload files**, drop the new file in, and commit.
4. The page shows the new data within about a minute.

Uploading a file inside the page only changes what you see on your own screen. It is not shared.

## How to share approved moves

Approve vendors in the Recommendations or Simulation tab, then click **Copy share link**. The link keeps the warehouse, tab, status filter and every approved move, so whoever opens it sees the same simulation.

## Rules used

- Capacity per slot = `max_timeslot_po_qty` (taken once per date × slot × warehouse, never added up).
- Normal maximum: Slot 1 up to 125%, Slot 2 up to 133%, Slot 3 up to 125%.
- If Slot 1 is over 100%, Slot 2 must stay at or below 100%.
- If Slot 2 is over 100%, Slot 1 and Slot 3 must stay at or below 100%.
- A whole vendor is moved (all its POs in that slot), never a single PO.
- A vendor can only be moved to a day and slot that matches its inbound days and delivery hours, and only if no slot that day ends up breaking the rules.
