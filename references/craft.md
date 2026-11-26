# Craft: the judgement calls, with numbers

These are the rules experienced product designers apply without thinking. The common thread is subtraction: to make one thing stand out, quiet everything else rather than enlarging it. Use them while planning a screen and as the yardstick in the critique. DESIGN.md overrides any number here when it says otherwise.

## Hierarchy

- One focal point and one primary action per view. If two things compete, demote one.
- At most two large elements on a screen. Emphasise with weight and contrast before size.
- Demote secondary content with a lighter grey or smaller weight before adding a new colour or a border.
- **Blur test:** the focal point should still be obvious with the page blurred. In the browser, run `document.body.style.filter = 'blur(6px)'`, take a screenshot, then `document.body.style.filter = ''`. If the eye goes somewhere you didn't intend, fix the hierarchy.

## Type

- About three sizes on one screen (body 14 to 16px, subhead 18 to 22px, heading up to 32px on app screens; marketing pages can go further), and two or three weights.
- Take sizes from a hand-picked, non-linear scale (for example 12, 14, 16, 18, 20, 24, 30, 36, 48, 60, 72), not arbitrary values.
- Line length 45 to 90 characters; 60 to 75 is a safe default. Cap it with `max-width` in `ch`.
- Line height 120 to 145% for body text; longer lines need more.
- All caps, when used at all, gets 5 to 12% extra letter-spacing.

## Space

- A fixed spacing scale (for example 2, 4, 8, 12, 16, 24, 32, 40, 48, 64, 80, 96).
- More space between groups than within them. If everything has the same gap, nothing is grouped.
- Start with too much space and remove it. Separate with space or a background change before reaching for a border.
- Dense tools (tables, dashboards, editors) stay calm by cutting noise, not by adding whitespace: fewer separators, dimmer chrome.
- Compose the first screen to the viewport. At desktop width nothing sits beside or under the content as an empty half or an empty band: the main panel grows to fill it, a second column carries the visual, or the content is centred on purpose. Space is something you place, not what's left over after the content stops. Check the 1440 screenshot for the largest empty rectangle and give it a job.

## Colour

- Define every shade up front: 8 to 10 greys and 5 to 10 shades of each brand colour, tuned by eye. Don't generate tints by lightening or darkening at runtime.
- Dark mode is not an inversion. Take dark surfaces as tonal steps from the palette (Material 3's tone-based surface roles), each step a little lighter for higher elevation, rather than hard-coding one grey such as #121212, which was Material 2's baseline. Make accents about 20 points less saturated than in light mode.

## Depth

A page with correct colours and no light on it reads as a wireframe. Gradients, glass and glow are on the tells list because of how they get misused, not because surfaces should be unlit: taking them out and putting nothing back is the commonest way a restyle ends up flat. Depth comes from three things used together, each defined once in DESIGN.md.

**Surfaces by role.** Name four and use them by what they contain, not by how important the thing feels: *sunken* (wells, inputs, the track behind a control), *page*, *raised* (cards, panels, the main figure) and *overlay* (menus, popovers, dialogs). This is the model current systems share: Material 3 replaced its elevation tint with five surface-container roles, and Atlassian uses sunken, default, raised and overlay (both as of 2026). Each role is a small lightness step from its neighbour, taken from the palette.

**A shadow paired with each raised role.** Raised surfaces get the raised shadow and overlays the overlay shadow, never mixed, and a tonal step on its own is not enough in a light theme. Build every shadow from layers, never one `box-shadow` value:

| Layer | Job | Starting values (light theme) |
|---|---|---|
| Top-edge highlight | the lit upper edge | `inset 0 1px 0 rgb(255 255 255 / .35)` on coloured or dark surfaces |
| Contact | where the surface meets the page | `0 0 0 1px rgb(0 0 0 / .05), 0 2px 4px rgb(0 0 0 / .05)` |
| Key | direction and height | raised: `0 18px 36px -12px rgb(0 0 0 / .16)`; overlay: `0 24px 48px -12px rgb(0 0 0 / .22)` |
| Ambient (the main visual and overlays) | soft, wide, tinted | `0 60px 100px -30px` in a dark shade of the accent at 30 to 40% |

These are sized to be seen. A 2px shadow at 14% passes a code check and is invisible on the page, which is how a run of this skill's own tests came out flat with "correct" shadows in the CSS. Dense app screens use the contact and key layers at about half these sizes; the main visual of a marketing page uses all four. As height rises, offset, blur and negative spread grow while each layer's opacity stays moderate; a dark, tight shadow reads as a sticker. Fluent 2 derives its key layer the same way: for level *n*, `0 n/2 n` at 14% black. The values above follow the layered recipes Fluent 2 and Frappe UI publish (2026); tune them, and swap black for a darker shade of the page's own hue. Light comes from above for the whole UI: every offset is positive on y and zero on x. Use `filter: drop-shadow()` for shapes that aren't rectangles.

**Dark themes: lighter surface, and keep the shadow.** A shadow alone disappears on a dark page, so each higher role is a lighter tone (a raised card a few points lighter than the page, itself lit top to bottom), with a 1px light edge (`inset 0 1px 0 rgb(255 255 255 / .14), 0 0 0 1px rgb(255 255 255 / .07)`), a deep drop shadow (`0 40px 80px -20px rgb(0 0 0 / .8)`) and, on the main visual, a wide glow in the accent (`0 0 140px -10px` at 40 to 50%).

**A lit ground.** The page background is a surface too, and the light on it has to be visible in a screenshot shrunk to a third of its size. Two layers: a top-to-bottom shift of a few lightness points across the page, and one pool of light behind the main visual, a radial gradient about 900 by 600px in a tint of the accent that fades to nothing. On a light ground the pool is a pale tint of the accent; on a dark ground it is the accent itself at 40 to 50% opacity. Derive both from the accent token with `color-mix()` so the lint's palette check passes. Raised surfaces can be lit top to bottom the same way. Declare the interpolation space (`linear-gradient(in oklab, ...)`); sRGB mixes go grey in the middle. Still tells: gradient-filled text, several blobs in different hues, glow behind text with no object casting it, a two-hue sweep across a hero. One pool of light in the product's own accent, behind the thing the page is showing, is not a tell.

**How much.** Most of the screen stays on the page surface. One or two things are raised, and what is raised is what matters; if every block has the shadow, nothing is lifted, which is the uniformity tell again. Marketing pages can carry more light than dense tools, where the tonal steps and the highlight edge do the work and shadows are kept for overlays.

- Derive the steps instead of hand-picking each one: `color-mix(in oklab, var(--page) 92%, white)` for a raised tone, `oklch(from var(--page) calc(l - .03) c h)` for a sunken one, and `light-dark()` to hold both themes in one token (set `color-scheme: light dark` on `:root`). `light-dark()` takes colours only, so shadow geometry that differs by theme needs its own custom properties. Based on web.dev and Chrome's guidance, 2025.
- Nested corners: inner radius = outer radius minus the padding between them, never below about 4px, so the corners look concentric. Adapted from the unslop skill by Damir Sokolovsky (CC BY 4.0), 2026.

## Finish

Stripping the tells and stopping there produces its own recognisable page: small type on a flat field, tight corners, outlined buttons and a bare table as the visual. It reads as old, not as considered. These are the things that make a screen read as current (2026); DESIGN.md records the exact values.

- **Type with range.** On a marketing page the display size is 72 to 96px at desktop, tracking -0.03 to -0.04em, line height about 1, and the lead paragraph 20 to 22px. Body never drops below 16px and small text below 13px. App screens keep the sizes in "Type" above.
- **Soft shapes.** A radius scale along the lines of 8, 14, 22: buttons and inputs 12 to 14px, cards and the main visual 20 to 24px, pills for chips. Radii of 3 to 6px on buttons and cards are what date a page fastest.
- **A primary button with weight.** Filled with the accent, lit top to bottom, with a top-edge highlight and a shadow in the accent's own hue (`inset 0 1px 0 rgb(255 255 255 / .35), 0 10px 22px -8px` in the accent at about 60%). The secondary button is a surface with a hairline and a small shadow, not an outline on nothing.
- **A main visual in layers.** The product preview is at least two overlapping pieces: the main card, and a smaller card that breaks its edge and shows the outcome (the confirmation, the notification, the total). A tilt of one or two degrees on the main card is optional. A single rectangle holding a table is a table, not a visual.
- **Light and dark, both.** Build both themes unless the product has fixed one: `color-scheme: light dark` on `:root`, colours held in `light-dark()` tokens, shadows and the pool of light as per-theme custom properties. The page follows the system setting on first load; a manual switch only overrides it and is never the only way to reach a theme. Screenshot both.
- **Thumbnail test.** Shrink the 1440 screenshot to 800px wide and look at it. The headline, the primary button, the raised visual and the light behind it must all still be visible. If it reads as a flat colour with small text, it isn't finished, whatever the CSS says.
- Replace what you remove. An emoji used as an icon becomes a real icon from one set (the project's own, or a single open line set at one size and stroke), not a bare label, unless the row reads better without any. A stripped glow leaves a surface step or a hairline behind. A page that only had things taken out looks unfinished.
- Small exact details carry a tool: tabular figures for numbers that line up, a monospace or condensed cut for counts and dates if DESIGN.md has one, real keycaps for shortcuts, one consistent marker for the current item.

## Waiting

Match the feedback to the wait:

| Wait | Feedback |
|---|---|
| Under about 300ms | Nothing; a spinner would only flash |
| Up to about 5 seconds | A spinner or busy label on the part that's working, not the whole page |
| First load of a list, cards or a table | A skeleton in the shape of the content, for a few seconds at most |
| Longer than about 5 seconds, or unknown | A progress bar with the current stage named, and a way to cancel. Indeterminate until progress can be measured, then determinate |

Skeletons belong to containers and data views only. Never put one on a toast, a menu or dropdown item, or a modal itself (content inside a modal can have one). Buttons, inputs, checkboxes and toggles don't get skeletons; a busy button keeps its label and shows an indicator (see `polish.md`). Based on IBM Carbon's loading pattern, updated 2026.

## Motion

- How often an action happens decides whether it animates. Frequent actions and keyboard actions get none. Rare, meaningful moments (opening a dialog, completing a task) can.
- 150 to 250ms for UI motion. Spring-based motion (Material 3 Expressive's model) is a valid alternative to fixed durations and curves. On the web, approximate a spring with CSS `linear()` easing, and keep the settle time in the same range.

## Hero visuals

- A landing hero, the top of a pricing or marketing page, an empty state and onboarding each get a visual; a headline with buttons and nothing beside it is unfinished. Dashboards and settings don't get an image, but a dashboard's main stat or chart can carry one small product detail of the same kind.
- Write the visual's subject as one plain sentence before making anything. If you can't, the idea isn't clear yet.
- The default visual is a preview of the product built in code (HTML, CSS, SVG), showing the one moment that proves the headline, not the whole app: the invoice it produces, the booking it confirms, the report it draws. Give it the same surfaces, type and radii as the rest of the page so it reads as the product, not a picture of one.
- Fill the preview with the brief's facts where it has them and with sample data where it doesn't. Sample data is what one customer of the product would see: an example client, line items, dates, amounts. Make it plausible, keep it consistent (the lines add up to the total) and let the frame show it's an example, by being an obvious mock-up or carrying a small "Example" label. It never says anything about the business itself: no ratings, customer counts, savings, awards, testimonials, real company names or logos, and no feature the brief didn't name. Those stay under the "Only facts you were given" rule in SKILL.md. List the sample data in the hand-back.
- Match the image's edges to the page background, or fade them with a mask. A hard rectangle of another colour looks pasted in.
- No text, logos or fake UI inside a generated image. Give every image fixed dimensions.
- Image generation is optional. If a tool for it is available, show the user the prompt, model and size and ask before running it, since it can cost money. If not, leave an empty slot at the final size and put the prompt in the hand-back.
- Avoid the stock generated look: no glossy blobs, orbs, nebulas, robots or glowing brains unless the product is literally about them.

Adapted from the unslop skill by Damir Sokolovsky (CC BY 4.0), 2026.

## Difficult data

Design with the content that breaks layouts, not the sample:

- the longest and shortest real values (a 60-character name, a one-letter one);
- 0, 3 and 25+ items in every list;
- a missing image, a missing optional field;
- an error, and a slow response.

If there's no real data, make fixtures with these cases and render each one.

## Rationale

Every major choice gets a one-line reason and the tradeoff it accepts ("one column so the price is the first thing read; costs a longer scroll on desktop"). A choice you can't give a reason for is probably a default.
