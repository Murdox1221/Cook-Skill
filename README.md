# Cook-Skill — meal-planner

A Claude Code skill that **suggests** a week of dinners in the household's
established style and generates a consolidated grocery list. The example rotation
and dish list are a taste baseline to draw from and extend — not a fixed schedule.
Backbone proteins: ground beef and chicken breast. Fixed exclusions: no
whole/chunked tomatoes (smooth sauce/paste is fine), no mushrooms or cream of
mushroom soup, no seafood.

## Layout

```
SKILL.md                        the workflow Claude follows
references/kitchen-profile.md    proteins in stock, exclusions, staples, FFY — EDIT THIS
references/dinner-rotation.md    5 example weeks showing the household's style (baseline)
references/dinner-options.md     dishes they like — a pool to draw from and grow
references/dish-notes.md         per-dish shopping ingredients (some are best guesses)
templates/weekly-plan.md         output format for the suggestions + grocery list
plans/                           saved weekly plans (plans/YYYY-MM-DD.md)
```

## Install

```bash
# personal (all projects)
ln -s /home/citadel/Projects/Cook-Skill ~/.claude/skills/meal-planner
```

A copy works too if you'd rather not symlink.

## Use

- "Suggest dinners for this week and make a grocery list"
- "Give me a week, mostly the usual plus a couple new ideas"
- "Swap Wednesday, keep the rest, redo the list"
- "Add taco soup to the options"

Claude asks a couple of quick questions (nights, servings, familiar vs. new mix,
anything to feature or avoid), drafts the week — reusing favorites and proposing
new-but-similar dishes — then builds an aisle-grouped grocery list from
`dish-notes.md`, skipping FFY nights and assumed staples.

### Avoiding week-to-week repeats

Each finalized week is saved to `plans/YYYY-MM-DD.md`. On the next run Claude reads
the 4 most recent of those: nothing from the last 2 weeks can reappear, at most 2
favorites may repeat across the last 4 weeks, and the recurring slots (Mexican
night, pasta night, breakfast-for-dinner) rotate. Keeping the saved plans is what
makes this work — if you don't save a week, it's invisible to the variety check.

## Tuning it

- **Shift the style / habits:** edit `references/dinner-rotation.md` and
  `references/dinner-options.md`.
- **Change what's assumed in stock or banned:** edit
  `references/kitchen-profile.md`.
- **Fix a wrong ingredient list:** edit `references/dish-notes.md` — `[confirm]`
  lines are guesses (the family-specific casseroles especially), `[sub]` lines
  are where an excluded ingredient was swapped out.
