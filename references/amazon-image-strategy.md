# Amazon Image Strategy

Use this reference before generating or revising Amazon secondary images.

## Market scan

When browsing is available and the user asks for real listing direction, inspect
3-5 current competitor examples before writing prompts. Do not copy a competitor
layout directly. Extract patterns:

- Which buyer worries are repeated across listings
- Which benefits are shown visually rather than explained in text
- Which image types appear in the carousel and in what order
- How much text is used per image
- Whether lifestyle scenes, diagrams, dimensions, or close-ups carry the sale

Reference source priority:

1. User-provided Amazon competitor links, especially top organic results or BSR
   leaders.
2. Amazon search results for the category keyword, using the first mature-looking
   organic listings and skipping obvious sponsored noise when possible.
3. Amazon product pages from adjacent keywords if the exact product phrase has
   weak results.
4. Brand sites, retailer pages, PDFs, or image search only as fallback context
   when Amazon pages are inaccessible or too thin.

For each Amazon competitor listing, inspect the carousel sequence, not just the
main image. Capture the strategy in words:

- Slot order, e.g. main, benefits, dimensions, stability, lifestyle, details
- Repeated buyer claims, e.g. anti-wobble, weight capacity, adjustable height
- Visual devices, e.g. dimension arrows, close-up circles, body silhouettes
- Text density, e.g. sparse badges vs paragraph-heavy banners
- Scene choices, e.g. home gym, living room, garage, outdoor training

Do not copy exact composition, text, colours, icons, model poses, or brand
elements from competitors. Use the scan to decide what buyers expect and where
the user's product can communicate more clearly.

If browsing is unavailable, infer these patterns from the product category.

## Reference image adaptation

When the user provides a reference image or competitor secondary image, treat it
as a visual brief, not a layout to copy. Extract what makes it work:

- Core viewpoint: what is the big claim?
- Buyer concern: what doubt does it remove?
- Visual proof: action, mechanism, comparison, scale, range, detail, scene
- Layout skeleton: where headline, product, proof, callouts, and whitespace sit
- Dynamic device: arrows, trails, ghosted states, zoom windows, silhouettes,
  before/after, split scene
- Colour mood: warm/cool, high contrast, premium, technical, lifestyle, playful
- Text hierarchy: headline size, number emphasis, label count, density

Then redesign for the current product:

- Keep the communication strategy, not the exact composition.
- Replace reference claims with user-provided specs and truthful product
  mechanics.
- Adjust the headline so it expresses the current product's buyer-facing
  conclusion.
- Change layout enough that the result is visually original.
- Avoid copying competitor wording, icons, exact colour palette, model pose,
  background, crop, or branded elements.

If the reference image is visually strong but strategically weak for this
product, keep the useful design energy and replace the message with a better
buyer concern.

## Competitive visual extraction

Before generating images, convert competitor observations into a usable visual
brief. Do not only list features. Extract:

- The first 3 buyer objections competitors try to remove
- Which image slot carries each objection
- The visual proof used, e.g. person training, heavy-load pose, reinforced base
  close-up, room-scale dimension lines
- The visual strength level: plain, moderate, or high-impact
- The text density level: sparse badge, short headline, or text-heavy

Prefer the mature pattern, then improve clarity. For a product with ordinary
specs, the image must win through visual proof, scene believability, and strong
thumbnail hierarchy.

## Dynamic visual language

A static generated image should still feel like something is happening. Before
writing each prompt, pick one dynamic device:

- **Action**: user is doing the core task, e.g. pull-up, dip, carrying, cooking.
- **Range**: ghosted low/high positions, slider trail, height scale, angle sweep.
- **Before/after**: problem state vs solved state, messy vs organized, unstable
  vs stable, beginner vs advanced.
- **Proof under use**: base contact, frame under load, water running, heat glow,
  light spread, battery powering devices.
- **Guided focus**: magnifier insets connected to the full product, exploded
  detail, numbered process arrows.
- **Scale/context**: human silhouette, room footprint, hand reference, object
  comparison.

If an image has no dynamic device and only shows the product with labels, it
must be redesigned or skipped unless it is a compliant main image.

## Buyer-first analysis

Before generating image prompts, write a short internal buyer-concern map:

- Will it fit my home?
- Is it stable under real use?
- Can it hold my weight?
- Can my height use it comfortably?
- What exercises can I actually do?
- Is the material strong enough?
- Is assembly/adjustment simple?
- What details make it safer or more comfortable?

Turn each concern into one image idea. Do not turn every specification into a
separate text block.

For every image idea, define the buyer cognition shift:

- Big headline/core viewpoint: the claim printed large on the image.
- Before seeing image: what is the buyer unsure about?
- After seeing image: what should they believe?
- Visual proof: what in the image makes that belief credible?

Reject or merge any image whose cognition shift duplicates another image.

## Core viewpoint headlines

Most strong Amazon secondary images have one large headline because the headline
forces a clear idea. Use this structure for every image:

`big headline + visual proof + short support labels`

The headline should be the buyer-facing conclusion, not the production slot
name. It must be specific enough that the visual can prove it.

Good headline patterns:

- "Built To Stay Stable" -> show base, structure, load/use proof.
- "Adjusts For Every User" -> show range trail, user silhouettes, lock holes.
- "Fits Your Home Gym" -> show footprint, room scale, product in corner.
- "Train More Than Pull-Ups" -> show multiple exercise silhouettes.
- "Comfort Where You Grip" -> show hand/grip contact, texture, ergonomic shape.
- "Keeps Water Out" -> show water flow and sealed structure.
- "Stores More In Less Space" -> show before/after storage capacity.

Weak headline patterns:

- "Features"
- "Details"
- "Product Benefits"
- "High Quality"
- "Multi Function" without showing the functions
- A raw spec alone when it does not say why the buyer should care

If a prompt cannot produce one strong headline, the product understanding is not
complete enough; rethink the buyer concern and visual proof before generating.

## Visual richness and layering

After the core viewpoint is clear, design the image so it has visual depth. A
good secondary image should not feel like a flat product cutout with stickers.

Use a three-layer plan:

- **Foreground**: strongest attention hook, such as hand/detail, badge, motion
  arrow, large number, or close-up proof.
- **Middle ground**: product hero, action, mechanism, or comparison state that
  proves the headline.
- **Background**: lifestyle context, room scale, soft technical grid, contrast
  block, or subtle environment that supports the claim.

When an image feels monotone, enrich it by fusing meaningful information:

- Add a dynamic device: action, range trail, before/after, proof under use,
  guided focus, or scale/context.
- Add one connected detail inset instead of a separate weak detail image.
- Add contrast between states: small/large, low/high, unstable/stable,
  messy/organized, ordinary/enhanced.
- Add human or environment context when it helps buyers understand fit, use, or
  scale.

Do not enrich by adding unrelated props, extra icons, decorative badges, or more
text. If extra layers do not strengthen the same headline, remove them.

## Selling-point grouping

Classify each user-provided selling point before designing the secondary-image
set:

- **Primary claim**: the result the buyer cares about.
- **Proof point**: material, number, certification, or structure that makes the
  claim believable.
- **Mechanism**: the visible part or action that enables the claim.
- **Use context**: where or how the buyer uses it.
- **Detail/safety**: small parts that reduce risk or improve comfort.

Then combine points into buyer-facing image themes. Use this rule:

`primary claim + proof point + visible mechanism = one stronger image`

Examples:

- `150 kg load capacity + 1.2 mm steel + reinforced frame` -> strength/stability
  image.
- `10-level adjustable + 155-210 cm height range + adjustment holes/pin` ->
  height-fit image.
- `82 x 78 cm footprint + warm home scene + room-scale arrows` -> home-space
  dimension image.
- `anti-slip suction base + wide base bars + training action` -> anti-wobble
  stability detail image.
- `pull-up + dips + knee raise + push-up` -> multi-function training image.

Do not allocate a separate image to a proof point unless it is the product's
main differentiator. A number such as tube thickness usually supports a larger
message such as durability, load bearing, or stability.

## Product understanding

Identify the physical product mechanics before designing images:

- Load-bearing structure: frame shape, base, braces, tube thickness, feet
- Adjustment mechanism: holes, pins, height range, number of levels
- Contact points: foam grips, elbow pads, back pad, handles, foot points
- Use modes: pull-up, dip, knee raise, push-up, stretch, resistance-band work
- Space requirements: width, depth, height range, room type

Images should explain these mechanics visually.

## Low-text visual rules

- One main message per image.
- Prefer large product visuals and user action over paragraphs.
- Use short callouts: 2-5 words each.
- Keep total visible marketing text low; avoid covering more than 15-20% of the image.
- Put exact specs in clean badges, dimension lines, or mini tables instead of sentences.
- Use icons only when they clarify the idea; avoid decorative icon lists.
- Use clear hierarchy: headline, product/action, 2-4 callouts.
- If a scene image is requested, show the product in use first and specs second.

Low text does not mean weak design. The image should still feel commercial:

- Strong product scale, not small centered product
- Warm or category-relevant background contrast
- Directional light, shadow, depth, or a real scene
- One bold headline that communicates the buyer benefit instantly
- Callout labels placed around the product without covering it

Avoid these weak outputs:

- Plain white/grey product render with small badges
- Too many specs competing equally
- Long paragraphs or banner text that hides the product
- Generic lifestyle room where the product looks pasted in
- Low-contrast beige-on-beige layouts that disappear at thumbnail size
- Close-up grids where each panel repeats a point already covered elsewhere
- Dimension diagrams with no sense of adjustment, movement, or real space fit
- First secondary image that only says "benefits" without a strong visual hook

## Secondary images vs detail-page images

Keep these two asset families separate:

- **Secondary images / 副图 / AS slots**: carousel images beside the Amazon main
  image. They should be punchy, thumbnail-legible, and low-text. Use AS-02 to
  AS-09.
- **Detail-page images / 详情图 / A+ / EBC / AD modules**: lower product-detail
  page content. They can be wider, more editorial, and more explanatory. Use
  AD-01 to AD-07.

Do not treat "详情图" as the same thing as AS-06. AS-06 means close-up secondary
image, such as a 2x2 macro grid or local magnification image. A detail-page
detail module is AD-04.

Detail-page modules should avoid duplicating the carousel one-to-one. Use them
to tell a longer story:

- Brand/value hero
- Problem/solution
- Core selling point with proof
- Structure/detail explanation
- Use-case expansion
- Specifications/fit
- Optional setup, care, or FAQ

## Power tower / fitness station priorities

For pull-up towers, dip stations, and home-gym racks, prioritize:

1. Stability and anti-wobble base
2. Height adjustability and user fit
3. Real exercise modes
4. Load capacity
5. Steel thickness and grip comfort
6. Home space fit
7. Easy adjustment or setup

Good secondary image pool:

1. Benefits hero: stable, adjustable, multi-training
2. Dimension and fit: width, depth, height range
3. Stability detail: base, feet, reinforced frame
4. Exercise scene: pull-up or dip in a home gym
5. Multi-function map: 4-6 exercise modes with silhouettes
6. Detail close-ups: grips, holes, pins, tube, feet
7. Load/material proof: 150 kg, 1.2 mm steel, reinforced structure
8. Adjustment steps: choose height, lock pin, train

Choose 5-7 images based on value. Do not force all 8 if a slot does not answer
a real buyer question. Common omissions:

- Skip installation/step images when setup is obvious or the product is already
  assembled in the listing photo.
- Skip packaging/box contents when no package contents, accessories, or bundle
  information was provided.
- Skip comparison when there is no fair, specific, non-infringing comparison
  point.
- Merge load/material proof with stability when both tell the same buyer story.
- Skip standalone detail close-ups when the same details can be stronger as
  magnifier insets inside stability, adjustment, or feature images.

Avoid making the first pass a collage of all eight images unless the user asks
for a storyboard or direction board. Generate separate images for production
assets when quality matters.

## Power tower high-impact visual plan

For power towers, build from concept roles instead of rigid slots. A strong set
is usually 5-7 images selected from these roles:

1. **Conversion hook**: a lively first image that sells the biggest result, e.g.
   "full-body bodyweight training at home" with product plus action silhouettes,
   or "stable pull-up station" with user action and base proof. Do not make this
   a passive product + badge summary.
2. **Stability/load proof**: merge load capacity, steel thickness, base width,
   suction feet, and frame reinforcement into one proof-under-use image.
3. **Dynamic height adjustment**: show 155-210 cm range with ghosted bar
   positions, upward arrow, 10-level markers, lock-pin/holes inset, and user
   height silhouettes.
4. **Home space fit**: show 82 x 78 cm footprint with room-scale floor plan or
   warm home-gym corner. Explain fit, not just numbers.
5. **Real exercise use**: one strong lifestyle action image for pull-up or dip,
   product fully visible, not just a posed room scene.
6. **Multi-function map**: clean action silhouettes around product for pull-up,
   dips, knee raises, push-ups; keep the product central.
7. **Optional detail/mechanism image**: use only if it adds a new story, such as
   grip comfort, anti-slip contact, or adjustment lock. Prefer connected insets
   over a generic 2x2 macro grid.

Optional only when useful:

- **AS-09 Adjustment/use steps**: use only if the adjustment mechanism needs
  explanation or the user asked for steps.
- Packaging/what's-in-the-box: use only if packaging, accessories, or included
  parts are known.

For "家庭风格, 暖色", use warm wood floors, off-white or taupe walls, natural
window light, soft shadows, and a neat living-room/home-gym corner. Add one
strong accent colour such as amber, black, or deep green so the image does not
look washed out.
