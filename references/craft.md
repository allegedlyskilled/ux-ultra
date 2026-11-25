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

- A screen needs at least two surface levels, such as the page and the cards or panels on it, and usually a third for what is raised or active. Separate them with a small tonal step from the palette, a hairline at low contrast or a soft shadow, picked once in DESIGN.md. One flat colour edge to edge with text sitting on it reads as a wireframe, however good the colour is. Removing gradients, glass and glow is not the same as removing depth.
- One light source for the whole UI. Higher elevation means more offset and more blur.
- Layered shadows tinted with the background hue look real; a single grey `rgba(0,0,0,.1)` looks pasted on.
- Nested corners: inner radius = outer radius minus the padding between them, never below about 4px, so the corners look concentric. Adapted from the unslop skill by Damir Sokolovsky (CC BY 4.0), 2026.

## Finish

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
