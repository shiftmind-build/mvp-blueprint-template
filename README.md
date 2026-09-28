# The MVP Blueprint

A nine-section template for specifying software that somebody actually has to build in
a fixed amount of time.

It exists because most "specs" are sales documents. They describe what the product will
be like and skip the three things that decide whether it ships: who is allowed to read
which row, what states the main object moves through, and what you are deliberately not
building. This template makes those three compulsory.

We use it to scope every ten-day build we run. It is public because a template nobody
can read is not a method, it is a claim.

---

## How to use it

Copy [`BLUEPRINT.md`](BLUEPRINT.md), fill it in, delete nothing.

An empty section is information: it means that part is not decided yet, and undecided
parts are where fixed-price builds go wrong. If you genuinely have no integrations,
write "none" — do not remove section 05.

Read [`EXAMPLE.md`](EXAMPLE.md) first. It is the same template filled in completely for
an invented delivery company, and it is the fastest way to see the level of detail the
thing is asking for.

## The nine sections

| # | Section | The question it answers |
|---|---|---|
| 01 | What we understood | What is the one thing that has to work for this to be worth launching? |
| 02 | Roles | Who uses it, and what can each of them do? |
| 03 | Screens | What does each screen do, and for whom? |
| 04 | What it stores | What are the tables, the field types, and **who can read a given row**? |
| 05 | What it talks to | Which third parties, what they cost, and whose account they are on |
| 06 | Acceptance criteria | What must be true for this to be finished? |
| 07 | The plan | What happens on which day |
| 08 | What we are leaving out | What is **not** in version one, why, and when it would make sense |
| 09 | The price | One number, and what changes it |

## The three that people skip

**Access, per row, in the database.** Section 04 asks who can read a given row, not who
can open a given screen. If the answer only exists in the query that builds the page,
then changing an id in the URL returns somebody else's data. The test that proves it:
sign in as A, request B's row, and confirm it does not come back. That test should block
a release.

**States.** Section 04 asks what states the main object moves through. Most scope
arguments three weeks into a build are really arguments about a state nobody named at
the start — "cancelled by whom?", "does expired mean deleted?".

**The cut list.** Section 08 is the one that makes a fixed price possible. Without a
written list of what is out, every conversation quietly adds to what is in, and the
date moves. It is also the section that earns the most trust, because it is the only
place in the document where somebody says no.

## Field types are part of the spec

Section 04 asks for real types, not labels. `date` and `timestamptz` are different
decisions and the difference shows up the first time two time zones are involved. Money
is integer cents or `numeric`, never a float. An enum with its values written out is a
decision; a `status text` column is a deferral.

## Licence

MIT. Use it, change it, sell work made with it. No attribution required, though a link
is welcome.

---

Maintained by [Shiftmind Build](https://shiftmindbuild.com), who build fixed-price MVPs
in ten working days. There is a [free generator](https://shiftmindbuild.com/tools/blueprint-generator)
that drafts sections 03, 04 and 08 from a description of your product, and a
[complete worked example](https://shiftmindbuild.com/blueprint-example) on the site.
