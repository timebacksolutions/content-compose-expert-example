# content-compose-expert-example

A third worked example of a **composed content project** built with
[throughline-compose](https://github.com/rhodium-org/throughline-compose) — the
**expert-and-formal** member of the trio, alongside
[content-compose-example](https://github.com/timebacksolutions/content-compose-example)
(general-and-formal) and
[content-compose-practitioner-example](https://github.com/timebacksolutions/content-compose-practitioner-example)
(practitioner-and-neutral).

Same council-housing domain, a third reader and job. Where the first example is a
tenant-facing **rent-statement help page** and the second is an officer's **rent-arrears
procedure note**, this one is a legal **briefing on the statutory position on rent
arrears and possession**, written for the council's **housing lawyers**. The reader has
full command of housing law — an *expert*, not a lay tenant and not a practitioner
officer. And the job is to **inform**, not to instruct: it states the law, it does not
give steps to follow. So the briefing has to be readable, correctly styled, in the right
register, doing the right communicative job **and** pitched at that reader. Those are
independent concerns, so the project adopts five orthogonal content sources by reference
and cites their clauses side by side on the requirements they satisfy:

| Namespace | Source | Axis |
|---|---|---|
| `plain` | [throughline-plain-language](https://github.com/rhodium-org/throughline-plain-language) `@v2026-07` | readability |
| `conventions` | [throughline-conventions-uk](https://github.com/rhodium-org/throughline-conventions-uk) `@v2026-07` | British-English conventions |
| `tone` | [throughline-tone-formal](https://github.com/rhodium-org/throughline-tone-formal) `@v2026-07` | register (formal) |
| `purpose` | [throughline-purpose-inform](https://github.com/rhodium-org/throughline-purpose-inform) `@v2026-07` | purpose (inform) |
| `audience` | [throughline-audience-expert](https://github.com/rhodium-org/throughline-audience-expert) `@v2026-07` | audience (expert) |

## The sibling-swap payoff, across the trio

Three of the axes are families of **mutually-exclusive sibling sources**: register
(`throughline-tone-formal` / `-neutral` / `-informal`), purpose
(`throughline-purpose-inform` / `-instruct` / `-persuade`) and audience
(`throughline-audience-expert` / `-practitioner` / `-general`).

Each of the three examples picks one sibling per family, and the readability and
conventions axes never move:

| Example | register | purpose | audience |
|---|---|---|---|
| tenant help page | formal | instruct | general |
| officer procedure note | neutral | instruct | practitioner |
| this briefing | formal | **inform** | **expert** |

Moving from the tenant page to this briefing is two one-line `ref` swaps — purpose
`instruct`→`inform` and audience `general`→`expert` — with the register left formal and
readability and conventions untouched. That is composability: each axis moves
independently, and this example is the first of the three to exercise the **inform**
purpose and the **expert** audience.

`plain` and `audience` look similar but are independent: `plain` is *universal* clarity
for any reader, while `audience` tunes how much prior knowledge and field vocabulary the
writing may assume for a *specific* reader — here, a housing lawyer. The briefing composes
both. It still leads with the conclusion and keeps sentences clear (`plain`), even as it
names statutory provisions and grounds for possession without glossing them
(`audience:SR-0002`, `audience:SR-0003`) — the expert calibration.

Each source numbers its items `SR-0001` upward, yet they never collide because each is
imported under its own namespace (`plain:SR-0004`, `conventions:SR-0008`, `tone:SR-0001`,
`purpose:SR-0001`, `audience:SR-0003`) — the same way a security project composes
`asvs` + `gds` + `wcag`.

## The authored briefing

The requirements graph is the *spec*; the briefing it governs is
[`content/arrears-possession-briefing.md`](content/arrears-possession-briefing.md).
throughline does not lint prose, so the artifact is a plain file — but read it against the
graph and each composed axis bites: the formal register (auxiliaries and negatives in
full — "does not", "cannot"), the expert terminology used unglossed (Ground 1 of Schedule
2 to the Housing Act 1985, the section 83 notice, the mandatory Ground 8), the inform
moves (each proposition attributed to its statute, settled law marked off from the
council's reading of an unsettled point, a "what this briefing does not cover" section,
and a stated currency date with a pending-reform flag), the conclusion-first opening, and
the sentence-case headings with GOV.UK number and date style ("£1,240", "1 July 2026").

## How it's wired

- The project's own graph lives under `intents/`, `user-requirements/` and
  `system-requirements/`. Each briefing requirement **grounds** through `implements` →
  `UR-0001` → `derives_from` → `INT-0001` (its own throughline), and **separately**
  `satisfies` the borrowed `plain:`/`conventions:`/`tone:`/`purpose:`/`audience:` clause
  it honours.
- `satisfies` is a traceability link, not a grounding link — so a briefing requirement
  still justifies itself through its own intent, not through a borrowed standard.

## Running it

```sh
tl-compose check --strict     # fetches all five pinned sources, merges, validates
tl-compose trace SR-0002      # show a requirement's throughline across the axes
```

Drive this project with `tl-compose`, never bare `tl`: bare `tl` fails fast the moment
it meets a namespace-qualified reference (`audience:SR-0003`) it cannot resolve, because
only the composition-aware tool fetches and merges the sources.
