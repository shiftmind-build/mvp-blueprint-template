# Blueprint — Northgate Logistics delivery app

> Northgate Logistics does not exist. This is the template filled in completely for an
> invented company, at the level of detail the template is asking for. A real blueprint
> belongs to the client who paid for it, even with the names taken out.

---

## 01 · What we understood

Northgate runs 18 delivery vans out of two depots. Today the route for each van is
printed the night before, and drivers phone the office when something cannot be
delivered. The office copies that into a spreadsheet, and customers are told "sometime
tomorrow".

**The one thing that has to work:**

> A driver opens the app, sees today's stops in order, and marks each one delivered or
> failed without phoning anyone.

---

## 02 · Roles

| Role | How many | What they can do |
|---|---|---|
| Driver | 18 | See only today's own route. Mark delivered or failed, add a photo and a note. |
| Dispatcher | 3 | Build tomorrow's routes, reassign a stop, see the live board. |
| Depot manager | 2 | Everything a dispatcher can, plus the weekly export. |
| Customer | — | No login. Receives a tracking link by SMS. |

**The boundary:** a driver must never see another driver's route, including by changing
an id in the URL. Enforced in the database, not in the screens. Tested in section 06.

---

## 03 · Screens

| Screen | Who opens it | What happens there | What must be instant |
|---|---|---|---|
| Sign in | All | Email and a six-digit code. No passwords to forget on a van. | — |
| Today's route | Driver | Ordered list of stops, distance to next, one tap to open maps. | The list |
| Stop detail | Driver | Address, contact, notes. Delivered / Failed, photo, reason. | Marking it done |
| Live board | Dispatcher | All 18 vans, stops done and remaining, anything failed in red. | Status changes |
| Route builder | Dispatcher | Drag stops between vans for tomorrow. Publish at 18:00. | — |
| Stop history | Manager | Search by address, date or driver. Photo and timestamp attached. | — |
| Weekly export | Manager | CSV of every stop, for the invoicing they already do. | — |

Seven screens. Anything not on this list is not in version one.

---

## 04 · What it stores

### driver

| Field | Type | Notes |
|---|---|---|
| id | uuid | |
| name | text | |
| phone | text | |
| depot_id | uuid | |
| active | boolean | Inactive drivers keep their history |

**Who can read a row:** any dispatcher or manager; a driver can read their own.
**Who can write it:** managers only.
**States:** none.

### route

| Field | Type | Notes |
|---|---|---|
| id | uuid | |
| date | date | A calendar day, not a moment — deliberately not timestamptz |
| van_id | uuid | |
| driver_id | uuid | |
| published_at | timestamptz | Null until the dispatcher publishes it |

**Who can read a row:** the driver it is assigned to, and any dispatcher.
**Who can write it:** dispatchers, and only before `published_at`.
**States:** `draft → published → closed`

### stop

| Field | Type | Notes |
|---|---|---|
| id | uuid | |
| route_id | uuid | |
| address | text | |
| window_start / window_end | timestamptz | |
| contact_phone | text | |
| position | int | Order within the route |
| status | enum | `pending / delivered / failed` |

**Who can read a row:** the driver on its route, and any dispatcher.
**Who can write it:** the driver on its route (status only), dispatchers (everything).
**States:** `pending → delivered | failed(reason)`

### delivery_proof

| Field | Type | Notes |
|---|---|---|
| id | uuid | |
| stop_id | uuid | |
| photo_url | text | |
| taken_at | timestamptz | |
| gps_point | geography(point) | Recorded at the moment of marking, not continuously |
| note | text | |

**Who can read a row:** dispatchers and managers.
**Who can write it:** the driver on the stop's route, once.
**States:** none.

> Drivers are not tracked between stops. That is a deliberate decision and it is written
> here so nobody discovers it later.

---

## 05 · What it talks to

| Service | What for | What it costs | Whose account |
|---|---|---|---|
| Twilio | The tracking SMS to the customer | ~$0.008 per message | Client's |
| Google Maps | Open navigation to the next stop | Free at this volume | Client's API key |
| Accounting tool | Nothing in v1 — the CSV export covers it | — | — |

Every account is opened in the client's name, with us added as a collaborator and
removed on handover. We are never the account holder for anything they depend on.

---

## 06 · Acceptance criteria

- [ ] A driver signs in and sees only their own route for today.
- [ ] A driver marks a stop delivered with a photo; it appears on the dispatcher board
      within five seconds.
- [ ] A driver marks a stop failed with a reason; the customer is not sent a delivery SMS.
- [ ] A dispatcher moves a stop between two vans and publishes; both drivers see the change.
- [ ] **Isolation:** signed in as one driver, requesting another driver's route by id does
      not return it.
- [ ] The weekly export opens in Excel with one row per stop.

---

## 07 · The plan

| Days | What happens | What the client does |
|---|---|---|
| 1–2 | Data model, roles, sign-in. The isolation test is written first. | Nothing |
| 3–5 | Driver screens end to end. Route, stop, proof, failure. | Nothing |
| 5 | They get a link and use it on their own phone. | Half an hour, say what is wrong |
| 6–8 | Dispatcher board and route builder. The SMS. | Answer questions same day |
| 9 | The six acceptance criteria run green. Load test at 50 drivers. | Nothing |
| 10 | Handover: code, accounts, a written manual. | Decide: keep it or walk |

---

## 08 · What we are deliberately leaving out

| Not in version one | Why | When it would make sense |
|---|---|---|
| Automatic route optimisation | Their dispatchers already do this well, and beating them is weeks of work | Once there is real stop data to optimise against |
| Native app in the stores | A web app on the phone does everything above; store review alone can take the ten days | When they need offline in dead zones |
| Customer accounts | Customers want a link, not a password | Only if they start asking for history |
| Live van tracking on a map | Nice to look at, changes no decision today | When there are more depots than they can phone |
| Invoicing | They have a system that works, and two systems is worse than one | Probably never |

---

## 09 · The price

**$2,490 total for everything above.** Not an estimate and not a range.

If sections 01 to 08 do not change, this number does not change either. Anything from
section 08 is priced as a second sprint before starting, so the whole number is always
known.

**Payment:** $150 for this document, paid up front and deducted from the total. The
remaining $2,340 is due on day 10, after they have used the product — and only if they
decide to keep it.
