---
name: meal-planner
description: >-
  Suggest a week of dinners in the household's established style and produce a
  consolidated grocery list. Uses their example rotation and dish list as a
  baseline to draw from and extend with similar new ideas. Backbone proteins are
  ground beef and chicken breast. Honors fixed exclusions: no whole/diced/chunked
  tomatoes (smooth tomato sauce/paste is fine), no mushrooms or cream of mushroom
  soup, no seafood. Use whenever the user asks to plan dinners, get meal
  suggestions, swap a night, add a dinner idea, or build a weekly grocery list.
---

# Meal Planner

Proposes a week of dinners that fit how this household already eats, then builds
the grocery list. The reference files are a **baseline, not a script** — reuse
their dishes freely and mix in new ones in the same style.

## Read first, every run

- `references/kitchen-profile.md` — proteins in stock, exclusions, staples, FFY.
- `references/dinner-rotation.md` — example weeks that show the household's style
  (cadence, side dishes, how often Mexican / pasta / breakfast-for-dinner / FFY
  come up). Not a fixed schedule.
- `references/dinner-options.md` — dishes they like and have eaten. A pool to pull
  from and grow, not a closed list.
- `references/dish-notes.md` — per-dish shopping ingredients, for the grocery list.
- `plans/` — every past week that was saved. **Read the most recent 4 files** (by
  date in the filename) and list every dish they contain. This is the
  don't-repeat memory; see step 2.

The user edits these over time; trust the files over anything remembered.

## What "their style" means

From the examples: American comfort food and Tex-Mex, kid-friendly, simple
ingredients. Ground beef and chicken breast most nights; also sausage, hot dogs,
pork, steak. Common sides: fries, mashed potatoes, green beans, rice, garlic
toast. Recurring shapes: a taco/burrito/nacho night, a pasta or mac night,
occasional breakfast-for-dinner, and **1–2 FFY ("fend for yourself") nights** a
week with nothing cooked or bought.

New suggestions should sit comfortably next to that list (e.g. sloppy joes, BBQ
chicken, chicken tenders & fries, taco soup, baked ziti, French dip, hamburger
steak, pot pie) — not a cuisine or technique jump.

## Workflow

### 1. Gather inputs

Ask for whatever isn't given; all have defaults:

| Input | Default |
|---|---|
| Nights to plan | 7 |
| Servings | 2 with leftovers |
| FFY nights | 1–2, user's choice of which |
| Mix of familiar vs. new | mostly familiar + 1–2 fresh ideas |
| Anything to feature or avoid this week | none (recent repeats are handled automatically from `plans/`) |
| Proteins/produce already on hand | none |

### 2. Suggest the week

Draft the days: pull from `dinner-options.md`, echo the cadence in
`dinner-rotation.md`, and add new-but-similar ideas per the requested mix. Keep
ground beef and chicken breast on most nights, don't repeat a protein
back-to-back, vary the method/cuisine across the week, and place the FFY nights.
Apply exclusions to everything (swap cream of mushroom → cream of chicken, no
diced tomato, etc.) and note any such swap.

**Don't-repeat rules.** First build the working set explicitly, then draft:

1. From the recent `plans/` files, write out **RECENT** = every dish in the last 2
   saved weeks, and **WINDOW** = every dish in the last 4 saved weeks.
2. Go through the **entire `dinner-options.md` list** (plus any new ideas) and
   mark each dish `available` or `blocked`. Do this by scanning the list against
   RECENT/WINDOW — do not rely on memory of what's been served.
   - In RECENT → blocked.
   - In WINDOW but not RECENT → blocked, **unless** it's a household favorite
     (tacos, fajitas, spaghetti, quesadillas, nachos and the like) — those are
     available, but no more than **2** such favorite-repeats may be used this week.
   - Not in WINDOW → available.
3. Draft only from `available` dishes.

Then:

- Rotate the recurring slots: if last week's Mexican night was fajitas, make this
  one a *different* available Mexican dish (tacos / enchiladas / nachos /
  quesadillas / crunch wrap / taco salad / burritos). Same for the pasta/mac
  night and the breakfast-for-dinner night. If every dish for a slot is blocked,
  use a new-but-similar idea rather than skipping the slot.
- Aim for at least 2 dishes this week that are not in WINDOW — favorites you're
  overdue for, or new ideas.
- If `plans/` is empty or has fewer than 2 weeks, just favor variety and say the
  history is thin.

Show the `blocked` list (grouped as "last 2 weeks" vs "weeks 3–4") so the user
can override.

Present it and invite changes: swap any night, lock nights they like and
regenerate the rest, add a brand-new dish (offer to save it to
`dinner-options.md`, and to `dish-notes.md` if they give ingredients).

### 3. Build the grocery list

For each cooking night (skip FFY):

- Pull shopping ingredients from `dish-notes.md`, scaled to servings. If a dish
  has no note, ask for its ingredients or make a clearly-flagged best guess.
- Omit pantry staples and the backbone proteins (ground beef, chicken breast)
  unless the user says stock is low. Include the other proteins.
- Merge duplicates across nights into one quantity.

Output per `templates/weekly-plan.md`:

1. **Dinners** — table: Day | Dinner | Main protein | Notes.
2. **Grocery list** — grouped by aisle (Produce, Meat & poultry, Dairy & eggs,
   Dry & canned, Frozen, Bakery, Other) with quantities, plus a short
   **"Check you have"** line for perishable staples the week leans on.

### 4. Save and follow-ups

**Save the finalized plan to `plans/YYYY-MM-DD.md`** (today's date) once the user
is happy with it — this is what makes the don't-repeat rules work next time, so do
it by default rather than only on request. If the user declines, warn that
variety tracking won't see this week.

Then offer to: write full recipes for any night; re-run the grocery list after
more swaps.

## Keeping the files current

When the user states a new standing fact ("we love this, add it", "we're bored of
chili mac", "we always have flour tortillas now"), offer to update the relevant
reference file so future suggestions reflect it.
