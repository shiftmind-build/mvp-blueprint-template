# Blueprint — [product name]

> Fill every section. An empty one is information: it means that part is not decided
> yet, and undecided parts are where fixed-price builds go wrong. If a section genuinely
> does not apply, write "none" and why — do not delete it.

---

## 01 · What we understood

*One paragraph in the client's own words: what exists today and what is wrong with it.*

**The one thing that has to work:**

> [A single sentence. If this sentence is true, the product is worth launching. If it is
> false, nothing else in this document matters. Everything below exists to support it —
> and anything that does not belongs in section 08.]

---

## 02 · Roles

| Role | How many | What they can do |
|---|---|---|
| | | |

*State the boundary explicitly: what must one role never see or do? That sentence
becomes a test in section 06.*

---

## 03 · Screens

| Screen | Who opens it | What happens there | What must be instant |
|---|---|---|---|
| | | | |

*Anything not on this list is not in version one, and the price in section 09 assumes
exactly these.*

---

## 04 · What it stores

For each thing the product stores:

### [entity name]

| Field | Type | Notes |
|---|---|---|
| id | uuid | |
| | | |

**Who can read a row:** [the condition, not the screen. "The driver it is assigned to,
and any dispatcher" — not "the route page".]

**Who can write it:** [same]

**States:** [`draft → scheduled → in progress → done | failed(reason)`. Leave blank only
if this thing genuinely has no lifecycle.]

> Types are decisions. `timestamptz` for a moment in time, `date` for a calendar day.
> Money as integer cents or `numeric`, never a float. An enum with its values written
> out, not a loose `text`.

---

## 05 · What it talks to

| Service | What for | What it costs | Whose account |
|---|---|---|---|
| | | | |

*Every third-party account is opened in the client's name. Say so here, and say when
access is handed over.*

---

## 06 · Acceptance criteria

*Each one becomes an automated test that has to pass before the client sees anything.
Write them so that a person who has never seen the product can run them.*

- [ ]
- [ ]
- [ ] **Isolation:** signed in as one [role], requesting another [role]'s [entity] by id
      does not return it.

---

## 07 · The plan

| Days | What happens | What the client does |
|---|---|---|
| | | |

*Include the day they see it running and the day they decide.*

---

## 08 · What we are deliberately leaving out

| Not in version one | Why | When it would make sense |
|---|---|---|
| | | |

*This is the section that protects the date. Without it, every conversation quietly adds
to what is in. It is also the only place in the document where somebody says no, which
is why clients trust it.*

---

## 09 · The price

**[amount] total for everything above.** Not an estimate and not a range.

If sections 01 to 08 do not change, this number does not change. Anything from section
08 is priced as separate work before it starts, so the whole number is always known.

**Payment:** [when, and what happens if they walk away]
