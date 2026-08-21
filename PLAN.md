# Project Plan — Climbing Truth or Consequences

A guidebook website for the rock around Truth or Consequences, New
Mexico, that also carries a hand-picked "best moderates" tour of the
Rockies from New Mexico to Montana, and an exhaustive census of every
climbing area in between. This file is the working plan; `research/`
holds the raw findings; `content/` is the published book.

## What the site is

Four parts, mirroring a print guidebook:

| Part | URL | Scope |
|---|---|---|
| I — Truth or Consequences | `/torc/` | Exhaustive home-area coverage: every crag, canyon and boulder we can document in Sierra County. Location → crag → route pages. |
| II — The Corridor | `/corridor/` | NOT exhaustive: our chosen areas up the spine of the Rockies, each with story, logistics, a selection of moderate classics (mostly 5.8–5.10), and pointers to the proper local guidebooks. |
| III — The Gazetteer | `/gazetteer/` | Exhaustive area-level census of climbing in NM, CO, WY, MT: rock, style, size, access, guidebook. No route lists. |
| IV — Using this Guide | `/guide/` | Grades, stars, symbols, sources, honesty about what is verified. |

Corridor areas named by the author: Ten Sleep WY, Lander WY, Monitor
Rock & Independence Pass CO, Tres Piedras NM, Penitente Canyon CO, the
Bridgers MT, Avalanche Gulch MT, White Sulphur Springs MT — plus others
we choose to add later.

## Design

Influenced by Aubrey Beardsley: black ink on cream paper, large flat
black masses (masthead, footer), sinuous whiplash-line SVG ornaments,
a stylised rose divider, drop caps, one restrained accent (the mustard
of The Yellow Book). Fin-de-siècle typography: Cormorant Garamond
display, EB Garamond text. Ornament lives at chapter heads and
frontispieces; the route tables stay austere. No JavaScript.

Hand-drawn maps, topos and photographs come later from the author;
until then every illustration slot is a framed "reserved plate"
(`{{ plate(caption="…") }}` shortcode) so the layout is honest about
what's coming.

## Content pipeline

1. **Research** (subagents, WebSearch-grounded) → `research/*.md`, one
   file per area, with sources and explicit `[unverified]` marks.
   Environment note: the egress proxy blocks direct fetches of
   Mountain Project / theCrag; only web search works, ~200 calls per
   agent. So research is snippet-grounded and honest about gaps.
2. **Agent log** — every subagent's reported findings are summarized
   in `research/agent-log.md` as project history.
3. **Content** — research is rewritten into `content/` pages with
   guidebook front matter (`extra.rock`, `extra.grades`,
   `extra.season`, `extra.approach`, …) that renders as a spec card.
4. **Build & ship** — `zola build` must pass; push to the working
   branch (auto-opens the draft PR).

## Operating rules

- Subagents: **at most one or two running at a time** (session-unit
  limits). The first wave of ten was launched before this rule; no new
  wave starts until it drains.
- Never invent route data. Thin records stay thin, marked as such —
  this scaffold gets corrected by the authors' own fieldwork.
- Commit and push often; the PR is the inbox.

## Status / remaining work

- [x] Site skeleton: templates, stylesheet, four-part structure
- [x] Research: Tres Piedras, Independence Pass, Penitente Canyon,
      Bridgers, Avalanche Gulch, White Sulphur Springs
- [x] Research: censuses for NM, WY, MT
- [ ] Research in flight: T-or-C deep dive, Ten Sleep, Lander,
      Colorado census, design brief
- [ ] Agent-findings log (`research/agent-log.md`), kept current
- [ ] Targeted follow-up searches on the thinnest route tables
      (one–two agents at a time)
- [ ] Write Part I content pages (T-or-C: area → crag → routes)
- [ ] Write Part II content pages (eight corridor areas)
- [ ] Write Part III content pages (four state gazetteers)
- [ ] Final pass: build, internal links, prose polish, push

## Later (author's work / future sessions)

- Hand-drawn maps and topos into the reserved plates
- Photography
- Field verification of every T-or-C route
- More corridor areas as we climb them
