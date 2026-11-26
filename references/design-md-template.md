# DESIGN.md template

Copy this structure into the project's `DESIGN.md` and fill every section with decisions for this project. Delete the guidance in italics. Every rule should be checkable by looking at the page or the code; "feel premium" is not a rule, "body text 17px on 28px line height" is.

The `tokens` block is read by `scripts/slop_lint.py`. Keep it in sync with the prose: one `key value` pair per line, `#` starts a comment.

````markdown
# Design

*One paragraph: what this product is, who uses it, the one thing the design must make easy, and where the look comes from (the reference, or the subject's own world).*

## Direction

*Name of the chosen direction and two or three sentences on it. What is the one memorable thing? Everything else stays quiet. Don't describe it as clean, modern, sleek, minimal or premium; those words fit every template.*

- **Moments:** *one or two places that get rich colour, material or detail, chosen from what the product shows. Everything else stays quiet.*
- **Signature:** *one small detail only this product would have, and where it is used. One, not five; use it the same way everywhere.*

### Directions considered

*All three directions from Step 1, the chosen one included, one short block each: name, palette (background hex and its kind, such as white, tinted paper, dark or colour field, plus accent hex), type genre, layout structure, and why it was or wasn't picked. A later session uses this to see what was already ruled out.*

## Tokens

```tokens
# colours: name, hex, role
color.paper      #F6F3EE   page background
color.ink        #1D2421   body text, icons
color.muted      #5C6660   secondary text (check 4.5:1 on paper)
color.accent     #2F6B4F   primary actions, links, focus ring
color.signal     #B4412F   errors and destructive actions only
color.line       #D8D2C7   borders, dividers
# utility shades derived from the core colours (input fill, hover tint); keep them few
color.util.field #FFFDF9   input background

# cursor: pointer on actionable controls | links-only
cursor           pointer

# type: quoted family names as they appear in CSS
font.display     "Fraunces"
font.body        "Source Sans 3"

# shapes
radius           8px 14px 22px 999px
# surfaces by role, and the layered shadow paired with each raised one (see craft.md, "Depth")
color.sunken     #EFE9DD
color.raised     #FFFDF9
color.overlay    #FFFFFF
shadow.raised    0 0 0 1px rgb(29 36 33 / .05), 0 2px 4px rgb(29 36 33 / .05), 0 18px 36px -12px rgb(29 36 33 / .16)
shadow.overlay   0 0 0 1px rgb(29 36 33 / .05), 0 2px 4px rgb(29 36 33 / .05), 0 24px 48px -12px rgb(29 36 33 / .22)
# the main visual on a marketing page: raised, plus a wide ambient layer in a dark shade of the accent
shadow.visual    0 0 0 1px rgb(29 36 33 / .05), 0 18px 36px -12px rgb(29 36 33 / .16), 0 60px 100px -30px rgb(31 74 54 / .38)
# dark theme: the same roles, lighter as they rise
color.dark.page    #121614
color.dark.raised  #1D2320
color.dark.ink     #EEF2EF
color.dark.accent  #5FB98C

# lint switches: rule ids from slop_lint.py
allow            # e.g. uppercase-eyebrow if small caps labels are a deliberate choice
ban              # rule ids to treat as errors in this project
```

## Type

*Roles and the scale. Make the steps clearly different: a display size several times the body size, weights far apart (300 vs 700, not 400 vs 500).*

| Role | Family | Size / line height | Weight |
|---|---|---|---|
| Display | Fraunces | 80/80, -0.03em | 700 |
| Heading | Fraunces | 28/34 | 600 |
| Body | Source Sans 3 | 17/28 | 400 |
| Small | Source Sans 3 | 14/20 | 500 |

## Space, radius, elevation

*Spacing scale (e.g. 4, 8, 12, 16, 24, 40, 64). Where each radius applies: inputs and buttons get the small one, panels the medium one, pills the round one. The surface roles (sunken, page, raised, overlay), which components sit on each, the shadow paired with raised and with overlay, how the page ground is lit (the top-to-bottom shift and the pool of light behind the main visual), and the dark theme's surfaces, edge highlight and glow. Both themes are built unless the product fixes one. Say which one or two things on a typical screen are raised.*

## Motion

*One orchestrated moment at most (a page-load sequence or one reveal), and motion that answers user actions. Durations and easing. Everything respects prefers-reduced-motion.*

## Components

*Every component the product uses, where it comes from (the library path, or "custom"), and the states it must have. Point at the library; don't re-document its API here. Keep the whole file short enough to read in one go: a long DESIGN.md that re-describes a component library makes agents rebuild components instead of importing them.*

| Component | Source | States |
|---|---|---|
| Button | components/ui/button | default, hover (under `@media (hover: hover)`), focus-visible, active, disabled (no pointer), loading (label kept or names the action; indicator after 150-300ms); primary and secondary only, one primary per view; at least 24px tall, 44px on touch |
| Text field | components/ui/input | default, focus-visible, disabled, error with message, filled |
| Dialog | components/ui/dialog | open, closing; traps focus, Escape closes, returns focus |
| List | custom | loading, empty (says what to do next), error (says how to retry), filled |

## Voice

*How the interface talks. Sentence case. Buttons name the action. Errors say what happened and what to do, without apology. Empty states invite the next step. Three example strings in this voice.*

## Don't

*The project's own ban list. Seed it from references/tells.md, but keep only what is a risk here and never ban what the brand uses. Add a line whenever the same correction comes up twice.*

- 
````
