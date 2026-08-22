# Design brief: a Beardsley guidebook

Two research questions, one document.

1. **How climbing guidebooks organize information** — the conventions of
   Mountain Project, theCrag, 27 Crags, Kaya, and print publishers, distilled
   into a page hierarchy, record schemas, and navigation patterns for this
   site.
2. **The visual vocabulary of Aubrey Beardsley** — translated into a palette,
   type stack, ornament system, and a discipline for where ornament is allowed
   to appear and where it is forbidden.

Written against the repo as it stands on 2026-08-21: a Zola site, one landing
page, **no JavaScript**, no web fonts, `build_search_index = false`. Every
recommendation below is compatible with those constraints or names explicitly
what it would cost to break one.

---

## 0. Method and its limits

Direct page fetching was blocked for this session by the network egress policy
— `mountainproject.com`, `thecrag.com`, `openbeta.io`, `fonts.google.com`,
`metmuseum.org` and every other destination attempted returned 403 at the
proxy. Web *search* worked until its per-session budget ran out.

So: findings marked **[verified]** come from search results retrieved this
session and the URLs are cited inline. Findings marked **[from familiarity]**
are drawn from working knowledge of these products and books, with the
canonical URL given so a later session can confirm them cheaply. Nothing here
is invented, but the second class deserves a spot-check before anything
load-bearing is built on it — particularly the exact field labels in §2.

---

# PART ONE — Information architecture

## 1. What each source teaches

### Mountain Project

The database convention almost every American climber already has in their
head. Worth matching in vocabulary even where we depart in form.

- **Hierarchy is arbitrary-depth, not fixed.** Areas nest from states and
  countries down to individual crags and boulders, and a state may have any
  number of levels of sub-areas beneath it. [verified —
  [Adding New Climbing Areas & Routes](https://www.mountainproject.com/help/12/adding-new-climbing-areas-routes),
  [Arranging Sub-Areas](https://www.mountainproject.com/forum/topic/119439206/arranging-sub-areas)]
- **A route record carries** name, type (which also encodes pitch count and
  length), difficulty in YDS, `fa` for first-ascent attribution, GPS
  coordinates, a prose description, and a separate `location_description`
  covering the approach. Star ratings are an average of user votes with the
  vote count shown. [verified —
  [FAQ / site features](https://www.mountainproject.com/help/22/overview-of-site-name-features),
  [API field summary](https://parse.bot/marketplace/9be670dd-9f87-4484-a4a1-7e04e17f48a2/mountainproject-com-api)]
- **An area record carries** a list of sub-areas *and* a list of
  `classic_routes` — the "best of" list is a first-class part of the area
  record, not an afterthought. [verified — same API summary]
- **Route page order** [from familiarity —
  https://www.mountainproject.com/route/105748391/the-nose]: breadcrumb of the
  full ancestor chain → route name → grade + safety rating + star average and
  vote count → a one-line spec (`Type: Sport, 2 pitches, 180 ft (55 m)`) →
  `FA:` line → page-view and contributor metadata → **Description** →
  **Location** → **Protection** → photos → comments/ticks. The sidebar holds
  suggested routes, the area map, and weather.
- **Area page order** [from familiarity]: breadcrumb → area name → intro
  description → *Getting There* → access notices → sub-area list → route table
  → sidebar with photos, a routes-by-grade histogram, and the classics list.
- **The route finder** is a separate top-level tool: filter by location, grade
  range, type, star rating, pitches. [verified —
  https://www.mountainproject.com/route-finder]

**Take:** the three-heading body (*Description / Location / Protection*) is the
single most transferable convention on the internet for climbing prose. Adopt
it verbatim. It answers, in order: is this route for me, how do I find it, what
do I bring.

### theCrag

The most rigorously modelled of the lot, and the best guide to *typing* a node.

- Node types are `a` area, `r` route, `w` world, `m` merged, `n` annotation.
  [verified — [API docs](https://www.thecrag.com/en/article/api)]
- **Area *types* are a controlled vocabulary**: Area, Region, Crag, Cliff,
  Sector, Boulder, Feature, Field, Unknown. [verified —
  [Area types](https://www.thecrag.com/article/AreaTypes)]
- The depth is free but the semantics are not: e.g. World → Europe → Greece →
  Aegean Islands (regions) → Kalymnos (typed *crag*). **Any node from "crag"
  downward may hold routes directly**, and **mixing sub-areas and routes at the
  same node is explicitly discouraged**. [verified —
  [Adding and Editing Areas](https://www.thecrag.com/en/article/documentingacrag)]
- Structured tagging supplements the tree for facets the tree can't express
  (style, condition, access). [verified —
  [Structured tagging](https://www.thecrag.com/en/article/tagging)]

**Take:** two rules to steal outright. (1) Type every node — a page is a
*region* or an *area* or a *crag*, and its template follows from its type, not
from its depth. (2) **A node either holds children or holds routes, never
both.** That one rule prevents the mess most homegrown guidebook sites end up
in.

### 27 Crags / The Topo

The phone-at-the-cliff end of the spectrum.

- Verified topos made by locals; filter crags and boulders by **grade, rating,
  and climbing style**; 3D map; in-app GPS navigation to individual routes;
  tick list and to-do list; offline download. Topo lines are drawn on photos
  and restyled for at-a-glance legibility. [verified —
  [App Store listing](https://apps.apple.com/us/app/the-topo-rock-climbing-guide/id1010852143),
  [Google Play](https://play.google.com/store/apps/details?id=com.crags27.crags27)]

**Take:** the filter triple **grade × style × quality** is the universal
minimum. Offline use is the real-world condition: our pages must be small,
static, and readable without network — which a no-JS Zola site is, natively.
That is a genuine competitive advantage worth protecting.

### Kaya Guides

[from familiarity — https://kayaclimb.com] Kaya licenses print guidebooks into
a digital product: guidebook-authored (not crowd-sourced) topos, sold per
guide, with **video beta attached to individual routes** and offline map
packs. The interesting structural idea is that the *guidebook* is itself an
entity — a bounded, authored, credited work with a table of contents — sitting
above the area tree rather than being dissolved into it.

**Take:** this site is an authored guidebook, not a database. Keep the
guidebook frame: a front matter section, chapters, a colophon, an index. That
framing also happens to be exactly what the Beardsley half of this brief wants.

### Print guidebooks — Wolverine, Sharp End, Fixed Pin

[from familiarity — [Wolverine Publishing](https://www.wolverinepublishing.com),
[Sharp End Publishing](https://www.sharpendbooks.com),
[Fixed Pin Publishing](https://www.fixedpin.com)]

The shared skeleton of a modern American guidebook:

**Front matter**
1. Title page, acknowledgements, dedication.
2. **How to use this book** — the legend: rating systems, star system, symbol
   key, how route numbers map to topo lines.
3. **Grades & ratings explained** — YDS, danger ratings (PG13/R/X), commitment
   grades (I–VII), V-scale.
4. **Access & ethics** — usually a full page, often co-written with or credited
   to the [Access Fund](https://www.accessfund.org): land manager, closures,
   parking, human waste, fixed anchors, chalk, wet rock, cultural resources.
5. Seasons/weather table, camping and services, geology, a short history essay.
6. Overview map of the whole book.

**Body — repeated per chapter (area)**
7. Chapter opener: a full-bleed photograph, the area name, a paragraph of
   character and history.
8. Area overview map with numbered crags and parking.
9. Approach: driving directions, then the walk — **time, distance, gain**.
10. Per crag: a short intro; a one-line condition strip (aspect, sun, season,
    approach time); the photo-topo with numbered overlay lines; the numbered
    route list **ordered left to right as you face the rock** — a convention so
    strong that violating it is a defect.
11. Route list rows: number, name, grade, stars, a one- or two-sentence
    description, gear note, FA and year.

**Back matter**
12. Route index A–Z, index by grade, "best of / classics" lists broken out by
    grade tier, advertisements, about the author.

**Icon systems.** Publishers vary but the recurring set is: sun aspect (morning
sun / afternoon sun / shade all day), approach time, walk difficulty, sport vs
trad vs mixed vs boulder vs toprope, rappel vs walk-off descent, kid-friendly,
loose rock, seasonal closure, no dogs. Stars are 0–4 or 1–3 depending on house
style; Mountain Project's 0–4 is the one most readers can convert without
thinking.

**Take:** the *condition strip* (aspect · sun · season · approach) is the
highest-value component in the entire genre and has no equivalent on Mountain
Project. It answers "should I drive there today". Build it as a first-class
component.

---

## 2. Recommended page hierarchy

Four levels, typed, with an optional fifth for big cliffs.

```
Home  (frontispiece / table of contents)
└── Region        /truth-or-consequences/
    └── Area      /truth-or-consequences/box-canyon/
        └── Crag  /truth-or-consequences/box-canyon/the-organ-wall/
            └── Route
                   /truth-or-consequences/box-canyon/the-organ-wall/the-flute/
            (└── Sector — only when a crag has >25 routes or physically
                 separated walls; inserts between Crag and Route)
```

**Rules**

- **Type every node** (theCrag's discipline). Zola front matter carries
  `extra.kind = "region" | "area" | "crag" | "sector" | "route"`, and the
  template dispatches on `kind`, not on depth.
- **A node holds children or routes, never both.** If a crag has three routes
  hanging off it *and* two sub-sectors, promote the three into a sector named
  for the main wall.
- **Four levels is the target ceiling.** Deep URLs are unmemorable and
  breadcrumbs stop fitting on a phone. If a fifth is needed, it is Sector, and
  it is a symptom that the Area above it should have been split.
- **Region is the chapter.** Given `config.toml`'s stated scope — TorC plus the
  moderate classics from the Rio Grande to the Missouri — Region is where the
  book's ambition lives and where the frontispiece ornament belongs.

### Zola mapping

```
content/
  _index.md                              # home / frontispiece
  truth-or-consequences/
    _index.md                            # kind = "region"
    box-canyon/
      _index.md                          # kind = "area"
      the-organ-wall/
        _index.md                        # kind = "crag"
        the-flute.md                     # kind = "route"
        black-mass.md
  access-and-ethics.md
  how-to-use-this-guide.md
  classics/_index.md
  index/_index.md                        # A–Z route index
```

**Routes get their own pages.** This is a real decision with a real cost —
one file per route — and it is worth paying, for three reasons:

1. **Zola taxonomies only apply to pages.** Making each route a page buys
   `/grade/5.10/`, `/style/trad/`, `/fa/bob-murray/` index pages generated at
   build time, **with zero JavaScript**. That is the entire route-finder
   feature set, for free, on a site that has sworn off JS. Routes stored as
   front-matter arrays inside a crag page get none of it.
2. Stable deep links. A route URL is the thing people text each other.
3. Room to grow: history, a topo plate, pitch-by-pitch tables, a photograph.

The crag page **still renders the full route list inline**, print-style, from
its child pages (`section.pages`), so a reader never has to click through to
plan a day. The route page is for the reader who wants more, not a toll gate.

Add to `config.toml`:

```toml
taxonomies = [
  { name = "grade",  feed = false, paginate_by = 0 },
  { name = "style",  feed = false },
  { name = "fa",     feed = false },
]
```

Populate `grade` with a *band* (`5.7`, `5.10`, `V2`), not the exact grade —
`/grade/5.10/` should hold 5.10a through 5.10d or the pages are one route long.
Keep the exact grade in `extra.grade`.

### Page inventory

| Page | Purpose | Ornament tier (§9) |
|---|---|---|
| Home / frontispiece | Title plate, what this book is, chapter list | 1 |
| Region | Chapter opener: character, history, map, area list | 1 |
| Area | Approach, overview map, crag list, grade histogram | 2 |
| Crag | Condition strip, approach, topo plates, route list | 2 |
| Route | Description / Location / Protection, FA, gear | 3 |
| Grade · Style · FA indices | Generated filter pages | 3 |
| A–Z route index | Every route, alphabetical | 3 |
| Classics | Best-of lists by grade tier | 2 |
| How to use this guide | Legend, symbols, grade systems | 2 |
| Access & ethics | Land managers, closures, ethics | 2 (no grotesques) |
| Colophon / about | Credits, sources, the drawn wordmark | 1 |
| 404 | A grotesque | 1 |

---

## 3. The route record

Front matter schema. Required fields are marked ●; the rest are omitted when
unknown, and templates must render cleanly with only the required set present —
a half-known route is normal and should not look broken.

```toml
+++
title = "The Flute"                        # ● name as climbers say it
weight = 7                                 # ● position L→R on the wall
date = 2026-08-21                          # last substantive edit

[taxonomies]
grade = ["5.10"]                           # band, for /grade/ pages
style = ["trad"]
fa    = ["bob-murray"]

[extra]
kind = "route"                             # ●
number = 7                                 # ● matches the topo line
grade = "5.10b"                            # ● exact, YDS
grade_system = "yds"                       # yds | v | aid | ice | mixed
danger = "PG13"                            # G | PG13 | R | X  (omit if G)
commitment = ""                            # I–VII, multipitch only
stars = 3                                  # ● 0–4, editorial, not voted
classic = true                             # appears in Classics lists
type = ["trad"]                            # ● sport|trad|boulder|toprope|
                                           #   aid|ice|mixed|alpine
pitches = 1                                # ●
length_ft = 90                             # ●
bolts = 0
anchor = "two-bolt lower-off"
descent = "walk off left"
rack = "singles to #3, two #1s, long slings"
fa_text = "Bob Murray, 1978"               # ● as printed; display verbatim
ffa_text = ""
aka = ["The Organ Pipe"]
tags = ["crimpy", "sustained", "chimney"]  # free vocabulary, for prose
season = "Oct–Apr"
first_hand = true                          # has the author climbed it?
verified = 2026-03-14                      # when last checked on the rock
plate = ""                                 # topo plate slug, see §11
photo = ""                                 # photo slug
+++

## Description
…

## Location
…

## Protection
…
```

Notes on specific fields:

- **`weight` is left-to-right order.** Zola sorts `section.pages` by weight;
  print convention demands the route list read the way the wall reads. Do not
  sort routes alphabetically or by grade on a crag page. Ever.
- **`number` is duplicated from `weight` deliberately** — the printed number on
  a topo must survive an insertion. When a new route goes in between 6 and 7,
  it becomes `6a` in `number` while weights get renumbered.
- **`stars` is editorial, not an average.** This is an authored guidebook; a
  vote count would be a lie. Say so in *How to use this guide*. Keeping the
  0–4 scale means readers can convert to and from Mountain Project without
  thinking.
- **`danger` is never conveyed by colour alone** and never suppressed for
  aesthetics. An R rating is safety information wearing an ornament's clothes.
- **`fa_text` is free text, deliberately.** FA credits are historically messy
  ("Murray/Ortiz, 1978, second pitch added 1983"). Keep the string; put a
  slugged name in the `fa` taxonomy for the index page.
- **`first_hand` / `verified`** encode the honesty Mountain Project asks for —
  their guidance is to enter only complete routes you have climbed and can
  describe accurately [verified —
  [help/12](https://www.mountainproject.com/help/12/adding-new-climbing-areas-routes)].
  Rendering a small "unclimbed by the author" note is more trustworthy than
  silence, and this is a guidebook whose credibility is the whole product.

### Pitch table (multipitch only)

```toml
[[extra.pitch]]
n = 1
grade = "5.9"
length_ft = 120
note = "Left-facing corner to a bolted belay."
```

---

## 4. The crag record

```toml
+++
title = "The Organ Wall"
weight = 3
sort_by = "weight"                         # route order = wall order
template = "crag.html"
page_template = "route.html"

[extra]
kind = "crag"                              # ●
summary = "Ninety feet of grey rhyolite…"  # ● one sentence, used in lists
rock = "rhyolite"                          # ●
height_ft = 90
aspect = "NE"                              # ● compass direction faced
sun = "morning sun, shade after 1pm"       # ● human phrasing, not a code
season = "Oct–Apr"                         # ●
approach_min = 25                          # ●
approach_ft_gain = 300
approach_text = "From the parking pull-out…"   # ●
drive_text = "8.4 mi south on NM-187…"
parking = [33.0000, -107.2500]             # ● lat, lon — the trailhead,
                                           #   not the cliff
vehicle = "2wd"                            # 2wd | high-clearance | 4wd
descent = "Walk off climber's left…"
hazards = ["loose rock on the upper wall", "rattlesnakes", "bees in spring"]
land_manager = "BLM — Las Cruces District"     # ●
access_note = "Raptor closure Feb 1 – Jul 15 on the main wall."
access_status = "open"                     # open | seasonal | sensitive |
                                           #   closed | private
amenities = "No water. Nearest gas in T or C, 20 min."
plates = ["organ-wall-main", "organ-wall-right"]
+++
```

Region and Area records are the same shape, minus the physical fields
(`rock`, `aspect`, `height_ft`) and plus `intro` prose and an overview map
plate. The **condition strip** component reads `aspect · sun · season ·
approach_min · vehicle` and renders as one line of small caps under the crag
title — the single most useful sixty characters on the page.

**Access is a box, not a sentence buried in prose.** `access_status` drives a
bordered block at the top of the crag page, before the approach: black rule
above and below, no ornament, `land_manager` and `access_note` in full. When
`access_status` is `closed` or `seasonal`, the box takes the vermilion accent
(§7) — the only place on the site that colour is permitted.

---

## 5. Navigation and wayfinding

1. **Breadcrumbs on every page below Home**, matching the tree exactly, last
   crumb unlinked and marked `aria-current="page"`. In Zola, walk
   `section.ancestors` (pages: `page.ancestors`) and `get_section()` each one
   for its title. Separator: a small ornamental lozenge, not `>`.
2. **Crag page carries the whole route list inline.** Numbered, in wall order,
   with grade, stars, one-line note, FA. A table at desktop widths; a
   definition-list card stack below ~40rem. Each row's number links to the
   route page and the topo plate's line number matches.
3. **Prev / next siblings** at the foot of every crag and route page ("← Left:
   Black Mass · Right: The Fissure →"). This is the page-turn of a print book
   and it is the navigation people actually use at the cliff.
4. **Grade histogram** on area and crag pages: an inline SVG bar chart of route
   counts per grade band, each bar an `<a>` to `/grade/5.10/`. No JS, no chart
   library, ~30 lines of Tera. It is simultaneously a data summary and the
   filter UI, and it tells a visitor in one second whether the crag is for
   them.
5. **Classics list** per region and site-wide, broken out by grade tier
   (5.easy / 5.9–5.10 / 5.11 / 5.12+), sourced from `extra.classic = true`.
   Mountain Project models this as part of the area record; so should we.
6. **Filter pages instead of a filter UI.** `/grade/`, `/style/`, `/fa/` are
   real pages, generated at build. Link them from *How to use this guide* and
   from the histogram. This is the no-JS route finder.
7. **A–Z route index** — every route in the book, one line each, with grade and
   crag. The back-of-the-book index. Also the cheapest search substitute.
8. **Search: leave it off for now.** `build_search_index = false` stands. The
   A–Z index plus the grade pages cover the realistic queries, and switching it
   on means shipping JS and a JSON index to a phone with one bar. Revisit when
   route count passes ~300.
9. **Skip the community features.** Ticks, votes, comments, to-do lists are
   what Mountain Project and 27 Crags exist for. A static authored guidebook
   that pretends to have a community has an empty comment section on every
   page. Offer one email address instead — the site already has the pattern.
10. **`@media print` stylesheet.** A guidebook that prints one clean page per
    crag, route list intact, is both genuinely useful and dead in the spirit of
    the thing. Cheap to build, and the design is already black-on-cream.

---

# PART TWO — Beardsley

## 6. What the style actually is

Aubrey Beardsley (1872–1898): *Le Morte d'Arthur* (1893–94), *Salome* (1894),
*The Yellow Book* (1894–95), *The Savoy* (1896). References:
[Tate](https://www.tate.org.uk/art/artists/aubrey-beardsley-702),
[V&A](https://www.vam.ac.uk/articles/aubrey-beardsley-a-life-in-illustration),
[Salome plates at Gutenberg](https://www.gutenberg.org/ebooks/42704).
[from familiarity — the fetches were blocked; the observations below are
stylistic analysis, verifiable against any plate.]

Eight properties, and what each one means for a stylesheet:

1. **One-bit ink.** Pure black, pure paper, no grey, no halftone, no
   modelling. This was not a preference so much as a consequence: Beardsley
   drew for the photomechanical **line block**, which could reproduce solid
   black and white and nothing between. → *The web is the same medium. Vector,
   crisp at any scale, tiny files, and it survives a phone screen in the sun.
   Never introduce a grey ornament; grey is the one thing the style cannot do.*
2. **Large flat black masses against emptiness.** A dress, a curtain, a
   cloak — one enormous unbroken black shape, and then a third of the plate
   left entirely bare. → *One solid black element per page, maximum. Whitespace
   is not leftover; it is the other half of the composition, and it should be
   uncomfortably generous.*
3. **The whiplash line.** Long, slow, uninterrupted contours that swell
   slightly and terminate in a hook or a spiral. Continuous — never sketchy,
   never hatched. → *Ornament paths are few and long. Resist adding detail;
   Beardsley's economy is the effect.*
4. **Asymmetry.** The subject shoved to one edge, the weight off-centre, the
   frame broken by a figure that leans out of it. → *Off-centre the text
   column at wide viewports and let the ornament live in the wide margin. Do
   not centre everything.*
5. **Borders and frames that are not symmetrical.** *Le Morte d'Arthur* has
   full Kelmscott-style borders; *Salome* often has a single heavy rule down
   one side, or a border that thickens on one edge and vanishes on another. →
   *A one-sided rule is more Beardsley than a box, and far cheaper to make
   responsive.*
6. **Stylised organic motifs.** The peacock feather and its eye (*The Peacock
   Skirt*), roses, thorny briar stems, tapers and candle flames, clustered dots
   as texture, stylised cloud and wave forms borrowed from Japanese woodblock
   (Utamaro). → *A small, fixed motif vocabulary — six or eight shapes reused
   everywhere — reads as a designed book. Twenty motifs read as clip art.*
7. **Grotesque elegance.** Satyrs, foetal grotesques, leering putti, hermae —
   beauty and unease in the same line. → *Permit exactly one grotesque, on the
   colophon and the 404. Keep grotesques away from anything a person reads to
   stay safe.*
8. **Hand-lettered title plates and initials.** Drawn titles inside cartouches;
   historiated initials in solid black panels; the AB monogram of three
   candlestick strokes. → *Draw the wordmark once as SVG. Everything else is
   type — faked hand-lettering with a display font is worse than good type.*

**The Yellow Book gives the accent.** Its black-on-yellow covers are the one
sanctioned colour in the whole vocabulary, and the reason the palette below
gets exactly one ochre.

---

## 7. Palette

The current `style.css` is already close — cream and near-black with a Caballo
red. Keep the bones; push the contrast further and re-assign the accents so
each has exactly one job.

```css
:root {
  --paper:      #F2EADA;  /* cream, never #fff — this is laid paper       */
  --paper-deep: #E8DEC9;  /* plate grounds, table zebra, hairline fills   */
  --ink:        #100D0A;  /* the black mass. Reads as black, not flat 000 */
  --ink-soft:   #4A4038;  /* secondary text only — never for ornament     */
  --rule:       #C9BCA4;  /* hairlines                                    */
  --ochre:      #C9A227;  /* THE YELLOW BOOK. Ornament + current state.   */
  --vermilion:  #9A3B26;  /* closures and hazards ONLY. Never decorative. */
}

@media (prefers-color-scheme: dark) {
  :root {
    --paper:      #100D0A;
    --paper-deep: #191410;
    --ink:        #EDE3D0;
    --ink-soft:   #A2947F;
    --rule:       #3A2F26;
    --ochre:      #D9B443;
    --vermilion:  #D4744F;
  }
}
```

**Discipline**

- Ochre covers **under 5% of any page**. Rules, the drop-cap panel, star
  glyphs, the current breadcrumb, link underlines on hover. Never a background
  fill behind text, never a button, never body copy.
- Vermilion is **reserved for `access_status` in {closed, seasonal} and
  `danger` in {R, X}**. If it starts appearing anywhere else, the safety
  signal is dead. Always paired with words, never colour alone.
- Body text `--ink` on `--paper` measures roughly 15:1 — well past WCAG AAA,
  which is the right target for a page read outdoors at midday.
- **Dark mode is Beardsley in negative** — legitimate, since his black masses
  simply become white ones. Two cautions: drop large ornaments to `opacity:
  .85` in dark mode (inverted line art glares), and give hairlines a hair more
  weight, since thin light-on-dark strokes disappear.

---

## 8. Typography

**Recommended stack**

| Role | Face | Why |
|---|---|---|
| Display — titles, chapter openers, route names | **Playfair Display** | High-contrast, hairline serifs, Didone-adjacent — genuinely close to 1890s English magazine display type. Ships as a variable font, real italic, huge language coverage. Reads at 3rem the way a title plate should. |
| Body — all prose | **EB Garamond** | Warm, period-plausible, one of the best-drawn families on Google Fonts. Real italic and small caps, and oldstyle figures that make prose look set rather than typed. |
| Labels, condition strips, breadcrumbs | **EB Garamond small caps**, letterspaced ~0.14em | Replaces the monospace eyebrow currently in `style.css`. Monospace is the one thing that will never be fin-de-siècle. |

**Evaluated and rejected (with reasons, so this isn't relitigated)**

- **Cormorant / Cormorant Garamond** — beautiful and *very* Beardsley-adjacent
  at display sizes: spidery, decadent, high contrast. Rejected as body text
  (far too light below 20px, hairlines vanish on a phone in sunlight), but a
  legitimate **swap for Playfair at the display role** if the site wants a
  lighter, more languid feel. Choose one or the other, not both.
- **IM Fell English** — the Fell types are c. 1680, not 1890, and their
  deliberate inking irregularities read as *seventeenth-century pamphlet*
  rather than *aesthetic movement*. Tempting for the letterpress texture;
  genuinely hard to read at body sizes. Use at most on the colophon, as a
  period joke, or not at all.
- **Della Respira** — closer to the mark than its obscurity suggests: an
  Art-Nouveau-inflected display serif with the right slightly awkward
  elegance. Single weight, single style, no italic. Viable **alternate for
  chapter titles only**; too limited to carry the system.
- **Cinzel Decorative** — Trajan-derived Roman capitals with flourishes. Wrong
  century, wrong civilisation, and it has become the wedding-invitation
  default. Reject.
- **Crimson Pro** — an excellent, sturdier body serif and the right call if EB
  Garamond proves too delicate on low-DPI screens. Less period character. Keep
  as the fallback decision, not the first one.

**Mechanics**

- **Self-host.** No Google Fonts CDN: it adds a third-party connection, a
  privacy footnote, and a render-blocking dependency to a site whose entire
  value proposition is that it works on one bar of signal. Drop `.woff2` files
  in `static/fonts/`, `@font-face` with `font-display: swap`, `unicode-range`
  latin subset. Variable versions of both families exist — two files, ~120KB
  total.
- **Fallback stack that doesn't lurch:** `"EB Garamond", ui-serif, Georgia,
  "Iowan Old Style", "Times New Roman", serif` — which is what the site already
  ships, so an unstyled first paint stays presentable.
- **Figures are the detail that makes or breaks this.** EB Garamond defaults to
  oldstyle figures, which is correct and lovely in prose ("first climbed in
  1978"). It is *wrong* in the route table, where grades and lengths must
  align. Set on tables and any numeric cell:
  ```css
  .route-table, .grade, .length { font-variant-numeric: lining-nums tabular-nums; }
  ```
  A grade of `5.10a` set in oldstyle figures inside a column of numbers looks
  like a mistake, because it is one.
- **Scale.** Body 19px / 1.62. Measure 62–68 characters — narrow. Headings at
  `clamp()` as the site already does. Ample space above headings, little below:
  the section should look attached to its text and detached from what precedes.
- **Asymmetric column at ≥64rem.** Content column at ~34rem, pushed off centre
  (roughly 40/60), with the wide margin free for ornament, marginal notes, and
  plate captions. This is where Beardsley's composition actually enters the
  layout rather than sitting on top of it.
- **Drop caps** on the first paragraph of Tier 1 and Tier 2 pages only — a
  solid `--ink` square with a reversed `--paper` initial. Pure CSS
  (`::first-letter` with a background, or a small floated block); no SVG, no
  image, and it reproduces a genuine *Morte d'Arthur* device.

---

## 9. Ornament: a three-tier budget

The central tension in this brief is that 1890s decadence and a functional data
table want opposite things. Resolve it with a **rule, not taste** — ornament is
a budget assigned per page type, and the budget is enforced by which template
includes which macro.

**Tier 1 — Frontispiece.** Home, Region openers, colophon, 404.
Permitted: a full or three-sided drawn border; the drawn wordmark or a title
inside a cartouche; a drop cap; one large solid black mass; a tailpiece. This
is where the book announces itself and where a visitor decides it is
beautiful. Spend everything here.

**Tier 2 — Chapter.** Area, Crag, Classics, How to use this guide, Access &
ethics.
Permitted: **one** corner ornament or a single vertical rule down one margin; a
whiplash divider beneath the H1; a drop cap on the intro paragraph; a tailpiece
at the foot. Forbidden: any frame around data, ornament between the condition
strip and the route list, grotesques on the access page.

**Tier 3 — Data.** Route pages, grade/style/FA indices, A–Z index.
Permitted: the star glyph, the hazard badge, hairline rules, and a single small
tailpiece at the very bottom of the page. Nothing else. A route page is read by
someone standing under the route with cold hands.

**Absolute prohibitions, at every tier**

- No ornament inside a table cell, or as a table background.
- No ornament behind text, at any opacity.
- No ornament framing a topo — the topo is the information; a border competes
  with the drawn lines on it.
- No ornament in the first third of a route page on mobile.
- No animation. If a line-drawing animation is ever added, gate it behind
  `prefers-reduced-motion: no-preference` — but the honest answer is that a
  line block does not move.
- Below 30rem viewport width, frames and corner pieces `display: none`. The
  dividers and glyphs stay.

---

## 10. Building the ornament in inline SVG

Everything is **inline SVG rendered by a Zola macro**, with
`fill="currentColor"` so it inherits `--ink` and flips in dark mode for free —
no image requests, no second colour scheme to maintain, no flash of unstyled
ornament.

```
templates/macros/ornament.html
  frame(sides)        # 1,2,3 or 4-sided drawn border for Tier 1
  corner()            # ONE corner piece; CSS transform makes the other three
  divider(kind)       # whiplash vine | thorn | plain double rule
  stars(n)            # n peacock eyes of 4
  tailpiece()         # small device closing a page
  badge(text, tone)   # R/X, closures — text + shape, never colour alone
  plate(...)          # see §11
```

**Technique**

- One `corner()` path, placed four times, flipped with
  `transform: scaleX(-1) / scaleY(-1)`. This is the single biggest economy
  available: one drawing yields a complete frame.
- `viewBox` on everything; size in CSS, never with `width`/`height`
  attributes.
- `vector-effect="non-scaling-stroke"` on hairlines so a scaled frame doesn't
  thicken.
- Prefer **filled paths over strokes** for the whiplash line — a stroke has
  uniform width; Beardsley's line swells and tapers, which only a filled
  outline gives you. This is the difference between "art nouveau clip art" and
  the real thing, and it is worth the extra path points.
- `aria-hidden="true"` and `focusable="false"` on every decorative SVG.
  Meaningful ones (`stars`, `badge`) get `role="img"` and a `<title>` —
  "3 of 4 stars", "R: serious fall potential".
- Motif vocabulary, closed at eight: peacock eye, peacock frond, rose,
  thorn-briar, taper flame, dot cluster, stylised cloud, lozenge. Everything on
  the site is assembled from those. If a ninth is needed, replace one.
- Keep total inline ornament under ~8KB per page. Ornament that costs more
  than the prose has stopped being ornament.
- No sprite sheet, no external `.svg` files: an extra request for 400 bytes of
  path data is a worse trade than the gzip cost of inlining it.

---

## 11. Placeholder treatment for art that isn't drawn yet

Hand-drawn maps and photographs come later. The design must look **complete and
intentional without them** — not like a page with holes.

**The plate.** Every piece of art on this site is a *plate*, in the
nineteenth-century book sense: a framed, captioned, numbered rectangle. Because
plates are ornamental objects in their own right, **an empty plate reads as a
book awaiting its engraving rather than as a broken image.** That is the whole
trick.

An empty plate renders:

- a thin double rule in `--rule` around a `--paper-deep` ground;
- the correct aspect ratio held open by `aspect-ratio` (topo 4:5, photo 3:2,
  map 1:1), so nothing reflows when the art lands;
- a centred device sized ~15% of the plate — the peacock eye for a topo, a
  compass rose for a map, a lozenge for a photo — at `--rule` weight;
- a small-caps caption beneath: `PLATE VII · THE ORGAN WALL, EAST FACE`, then a
  lighter line: `to come`.

**Macro contract**

```jinja
{{ ornament::plate(
     slug="organ-wall-main",
     caption="The Organ Wall, east face",
     kind="topo",            {# topo | photo | map #}
     src=""                  {# empty ⇒ render the empty plate #}
) }}
```

Presence of `src` is the switch — declared in front matter, no filesystem
probing, no build-time failure when a file is missing. Filename convention:
`static/plates/<crag-slug>-<n>.<ext>`. When the drawing arrives, one front
matter line changes and the caption, number, frame and reserved space are
already right.

**When the real art arrives**

- Hand-drawn line maps and topos: deliver as **SVG** if drawn digitally, or as
  a **thresholded 1-bit PNG** if scanned from paper. Grey scans will fight the
  palette; threshold them properly.
- Because pure line art is one-bit, a single asset can serve both themes:
  `.plate--line img { filter: invert(1); }` inside the dark-mode block. Opt-in
  by class, since it must not be applied to photographs.
- Photographs are the one place real greys enter the design. Contain them: keep
  them inside the plate frame, consider a duotone (`--ink` → `--paper`) to hold
  the two-colour discipline, and never full-bleed except on a Tier 1 chapter
  opener.
- Always `width`/`height` attributes plus `loading="lazy"` below the fold, and
  real alt text — a topo's alt text should say what the line shows, since a
  reader who can't see it still needs the route.

---

## 12. Marrying the two halves

The failure mode is a beautiful home page attached to route pages nobody can
read at the cliff. Five rules that prevent it:

1. **Ornament may never cost information.** If a frame pushes the condition
   strip below the fold on a phone, the frame goes.
2. **The ornament budget (§9) is enforced in templates, not by judgment.**
   Tier 3 templates simply do not import the frame macros.
3. **Every page must be usable with CSS disabled.** No-JS is already a
   principle here; this is its sibling, and it is what makes the site work on
   one bar of signal.
4. **Safety information is never styled decoratively.** Closures, R/X ratings,
   land-manager notes: plain type, black rules, one vermilion accent, top of
   page.
5. **Decadence lives in the pacing, not the density.** A reader should move
   from an ornate frontispiece, through a lightly-ornamented chapter, to a bare
   route page — and feel that as descent into the canyon rather than as
   inconsistency. That gradient *is* the design.

---

## 13. Suggested build order

1. `style.css` → tokens from §7, self-hosted fonts from §8, asymmetric column.
2. `macros/ornament.html` with `divider`, `stars`, `badge`, `plate` — the four
   that Tier 2 and Tier 3 need. Frames and corners can wait.
3. `crag.html` and `route.html`, plus the condition strip and access box. One
   real crag, fully entered, before any second crag — the schema will be wrong
   in ways only real content reveals.
4. Taxonomies in `config.toml`; `grade`/`style`/`fa` index templates.
5. Breadcrumbs, prev/next siblings, the A–Z index, the grade histogram.
6. `region.html` and the Tier 1 ornament: frame, corner, wordmark, drop cap.
7. *How to use this guide* and *Access & ethics* — the front matter that turns
   a set of pages into a book.
8. `@media print`.

---

## Sources

**Retrieved this session**

- [Mountain Project — Adding New Climbing Areas & Routes](https://www.mountainproject.com/help/12/adding-new-climbing-areas-routes)
- [Mountain Project — Overview of site features](https://www.mountainproject.com/help/22/overview-of-site-name-features)
- [Mountain Project — Regional Admins](https://www.mountainproject.com/help/14/regional-admins)
- [Mountain Project — Route Finder](https://www.mountainproject.com/route-finder)
- [Mountain Project forum — Arranging Sub-Areas](https://www.mountainproject.com/forum/topic/119439206/arranging-sub-areas)
- [Mountain Project API field summary (Parse.bot)](https://parse.bot/marketplace/9be670dd-9f87-4484-a4a1-7e04e17f48a2/mountainproject-com-api)
- [Wikipedia — Mountain Project](https://en.wikipedia.org/wiki/Mountain_Project)
- [theCrag — Sharing and embedding (API)](https://www.thecrag.com/en/article/api)
- [theCrag — Area types](https://www.thecrag.com/article/AreaTypes)
- [theCrag — Adding and Editing Areas](https://www.thecrag.com/en/article/documentingacrag)
- [theCrag — Maps and geolocations](https://www.thecrag.com/en/article/geolocation)
- [theCrag — Structured tagging](https://www.thecrag.com/en/article/tagging)
- [theCrag — Moving and sorting](https://www.thecrag.com/en/article/moving)
- [Gabe Grayum — theCrag product design case study](https://gabegrayum.com/design-theCrag.php)
- [The Topo / 27 Crags — App Store](https://apps.apple.com/us/app/the-topo-rock-climbing-guide/id1010852143)
- [The Topo / 27 Crags — Google Play](https://play.google.com/store/apps/details?id=com.crags27.crags27)
- [Climbing — The Best Climbing Apps](https://www.climbing.com/gear/6-must-have-climbing-apps/)

**Cited from familiarity; fetch blocked by egress policy — verify before
relying on exact labels**

- Mountain Project route page: https://www.mountainproject.com/route/105748391/the-nose
- OpenBeta climbing data schema: https://docs.openbeta.io
- Kaya Guides: https://kayaclimb.com
- Wolverine Publishing: https://www.wolverinepublishing.com
- Sharp End Publishing: https://www.sharpendbooks.com
- Fixed Pin Publishing: https://www.fixedpin.com
- Access Fund (access & ethics language): https://www.accessfund.org
- Tate — Aubrey Beardsley: https://www.tate.org.uk/art/artists/aubrey-beardsley-702
- V&A — Beardsley, a life in illustration: https://www.vam.ac.uk/articles/aubrey-beardsley-a-life-in-illustration
- *Salome* plates, Project Gutenberg: https://www.gutenberg.org/ebooks/42704
- Google Fonts specimens: EB Garamond, Playfair Display, Cormorant Garamond,
  IM Fell English, Della Respira, Cinzel Decorative, Crimson Pro —
  https://fonts.google.com/specimen/EB+Garamond (etc.)
