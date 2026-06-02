---
name: amazon-listing-generator
description: >
  Generate Amazon listing assets from a product photo and selling points:
  title, five bullets, description, backend search terms, image prompt plan,
  competitor-aware secondary-image strategy, optional secondary image generation,
  and an Excel summary. Use when the user uploads or describes a product and
  mentions Amazon, listing, 亚马逊, 上架, 主图, 副图, 卖点, product images,
  "帮我做listing", "生成产品图", "Amazon listing", "我要上架",
  "帮我写卖点", "只生成Listing文案", "看看别人怎么弄",
  "参考竞品", "参考Amazon前几名", "参考图", "参考图片", "参考样式",
  "照这个风格", "按这个感觉", "尺寸图", "功能图", "卖点图",
  "场景图", "细节图", "局部放大图", "对比图", "步骤图", "包装图",
  "详情图", "详情页", "A+页面", "A+ Content", "EBC", or asks to
  regenerate a specific Amazon listing image slot such as AS-05 or detail-page
  module such as AD-03.
---

# Amazon Listing Generator

From one white-background photo plus 2-3 selling points, generate an Amazon
listing kit: listing copy, image prompts, optional secondary images, and an
Excel summary.

---

## STEP 0 — Choose execution mode and read inputs

Infer the mode from the user request:

- **Full kit**: Generate listing copy, image prompts, a recommended secondary
  image set, and Excel.
- **Image set only**: If the user asks for "副图", "对应副图", "生成产品图",
  "secondary images", "Amazon images", or similar without asking for listing
  copy, generate a recommended secondary-image set, usually **5-7 separate
  production images**. Choose the count by image value, not by filling slots.
  Skip listing copy and Excel unless explicitly requested.
- **Copy only**: If the user says "只生成Listing文案", "不需要图片", "copy only",
  or similar, generate only the listing copy in chat. Skip image prompts, image
  generation, and Excel unless the user explicitly asks to export, save, or
  generate an Excel/table file.
- **One image slot**: If the user asks to regenerate AS-02 through AS-09, generate
  only that slot using the existing product/listing context.
- **Detail page / A+ image set**: If the user says "详情图", "详情页",
  "A+页面", "A+ Content", "EBC", "产品详情长图", or similar, treat it as a
  separate detail-page module set, not Amazon secondary images. Generate AD
  modules rather than AS slots.
- **Full 8-image set**: Generate all AS-02 through AS-09 only when the user says
  "完整8张", "全套8张", "AS-02到AS-09都要", or "full set".
- **Preview batch**: Generate only 1-4 secondary images when the user explicitly
  asks for a preview, a first batch, "先出几张", "先来4张", or names only those
  specific slots.

Map natural-language image type requests to slots:

- 卖点图 / 核心卖点 / benefits: **AS-02**
- 功能图 / 功能拆解 / callouts / feature breakdown: **AS-03**
- 尺寸图 / 尺寸参数 / dimension: **AS-04**
- 场景图 / lifestyle / 使用场景: **AS-05**
- 细节图 / 局部放大图 / 特写 / macro / close-up: **AS-06**
- 对比图 / comparison / 竞品对比: **AS-07**
- 步骤图 / how to use / 安装步骤 / 使用步骤: **AS-08**
- 包装图 / 全家福 / what's in the box: **AS-09**

Do not map "详情图" to AS-06. "详情图" means detail-page/A+ content modules.
AS-06 is only for secondary-image close-ups or local detail magnification.

When a user asks for one type, generate only that mapped slot unless they
explicitly request the full set.

When a user says "副图" without a specific type, create a recommended production
set. Default to 5-7 images:

- Use 5-6 images when the product story is complete and low-value slots such as
  installation, comparison, or packaging would feel forced.
- Use 7 images when one optional proof or process image adds real buyer value.
- Use 8 images only when each slot has a distinct buyer question, or when the
  user explicitly asks for the full set.

Do not silently generate only 3-4 images unless the user asked for a preview or
named only those specific slots.

Common single-image commands users may give:

- "只生成AS-04尺寸图"
- "生成一张场景图，家庭暖色"
- "重新生成AS-07对比图"
- "只要卖点图"
- "把AS-05改成户外场景"
- "生成单张防滑底座细节图"
- "生成详情页6张模块图"
- "只生成AD-03卖点详情模块"
- "参考这张图的感觉重新做AS-04"
- "给你一张竞品副图参考，不要照抄，按我们的产品改"

Examine the uploaded product photo. Extract: product category, materials,
colours, size, brand markings, key design features, and the **current product
state** shown in the image: folded, unfolded, extended, collapsed, open, closed,
assembled, packed, filled, empty, on/off, locked, unlocked, or in-use.

Protect the observed product state. Do not invent a more extreme state just to
make a visual claim stronger. If the product photo already shows a folded or
stored state, do not compress, narrow, shorten, flatten, or create a "more
folded" version. If it shows an unfolded state and no folding mechanism is
visible or provided by the user, do not invent a folded version. Preserve the
product's real proportions, width, height, tube spacing, hinge/lock positions,
and structural relationships.

Read selling points verbatim. Identify: core benefit, material claim,
target user, any specs or numbers.

Before planning images, understand the product and buyer concerns. For any
secondary-image or detail-page image request, read
`references/amazon-image-strategy.md`. If the user asks for Amazon-style images,
competitor direction, or says "看看别人怎么弄", "参考竞品", or "参考Amazon前几名",
also inspect 3-5 current Amazon top-result or user-supplied Amazon competitor
examples when browsing is available, and extract patterns without copying them.
Use non-Amazon examples only as fallback or supplementary category context. If
browsing is unavailable, infer category norms from the product.

If the user provides a reference image, competitor image, or says "参考图",
"参考图片", "照这个风格", "按这个感觉", use it as inspiration, not a template.
Analyze and transfer only the underlying design decisions:

- Core viewpoint/headline logic
- Visual proof type: action, range, before/after, detail inset, comparison, scale
- Composition structure: product placement, foreground/middle/background,
  headline area, support labels, whitespace
- Colour mood and contrast level
- Text density and hierarchy
- Dynamic devices such as arrows, trails, silhouettes, zoom windows, or split
  states

Do not copy exact layout, exact wording, icons, brand elements, model pose,
background, colours, or proprietary visual devices. Adapt the reference to the
current product, buyer concern, user-provided specs, and brand/style direction.
When the reference conflicts with product truth or buyer value, prioritize the
current product and explain the adaptation briefly before generation.

Reference images can suggest how to express a benefit, but they must not
override the current product state. Transfer the communication method, such as
home corner placement, floor footprint highlight, folding arrow, or mechanism
zoom, without forcing the current product into a different or exaggerated shape.
For foldable/storage claims, express compactness with environment and proof
devices: wall/corner placement, furniture-scale comparison, floor footprint
highlight, clearance shadow, measuring grid, storage path arrow, or lock/hinge
close-up. Do not show compactness by shrinking or distorting the product body.

Build a buyer-concern map before writing image prompts. Identify what a buyer
would worry about first: fit, stability, capacity, adjustability, use cases,
material strength, comfort, assembly, safety, and home space. Each secondary
image should answer one concern visually.

Classify and merge selling points before assigning image slots. Do not make one
image per raw parameter. Group specs by buyer decision logic:

- **Claim**: buyer-facing promise/result, e.g. load capacity, waterproof,
  long battery life, fast heating.
- **Proof**: structural/material reason that supports the claim, e.g. tube
  thickness, motor wattage, battery capacity, fabric density, certification.
- **Mechanism**: how the product works, e.g. adjustable holes, locking pin,
  foldable hinge, suction base, remote control.
- **Fit/size**: dimensions, height range, capacity, room or body fit.
- **Use case**: what the customer does with it, e.g. pull-ups, storage,
  cooking, cleaning, travel.
- **Comfort/safety detail**: grip, anti-slip pad, rounded edge, cushion,
  insulation, guard rail.
- **Included/installation**: package contents, assembly steps, accessories,
  maintenance.

Merge related points into one image when one explains or proves another:

- Proof + claim: "1.2 mm steel" supports "150 kg load capacity", so they belong
  in one strength/stability image.
- Mechanism + fit: "10-level adjustable" supports "155-210 cm height range",
  so they belong in one adjustment/user-fit image.
- Fit + scene: "82 x 78 cm footprint" supports home-space suitability, so show
  it in one dimension/home-fit image.
- Detail + safety: "anti-slip suction base" supports stability under training,
  so show it with base contact and anti-wobble proof.
- Use cases together: related exercises belong in one multi-function image
  unless one exercise is the primary lifestyle hero. Multi-use images must plan
  each body pose around a real contact point on the product before generation:
  hands on the pull-up bar, hands on dip handles, feet/hands near base support,
  or body hanging from the correct bar. If the actions cannot all fit cleanly
  around one product, use separated vignettes, split panels, or ghosted
  same-person motion states instead of crowding multiple full people onto one
  distorted product.

If a spec is only a proof point, avoid making it the headline alone. Use it as a
badge, callout, close-up, or secondary label under the buyer-facing claim.

Design images by buyer journey and visual event, not by rigid slot templates.
AS slot ids are output labels; they must not force weak or repetitive images.
For every proposed image, answer these checks before generating:

- **What is the one-sentence core viewpoint?** Write it first as the image's
  big headline claim. If it cannot be expressed as one clear buyer-facing
  sentence or phrase, the image is not ready.
- **What changes in the buyer's mind after this image?** If the answer is
  unclear, merge it into another image or skip it.
- **What is moving, changing, comparing, or being proven visually?** Static
  product + badges is not enough for a production asset unless the product
  itself is the proof.
- **Which point is unique to this image?** Do not repeat the same base, grip,
  steel, or training claim across multiple images unless each use is serving a
  different buyer question.
- **What is the visual hook before text?** A viewer should understand the core
  idea from action, arrows, ghosted positions, contrast, close-up context, or
  scale before reading labels.

Every production secondary image needs a clear communication hierarchy. A
headline is a design choice, not a required decoration:

1. **Core selling point / viewpoint**: one buyer-facing claim. It should answer
   "why should I care?" not merely name the slot. This can be expressed through
   a headline, a large spec, a dimension line, a before/after split, a product
   detail, or a clear action scene.
2. **Visual proof**: product in action, range, comparison, mechanism, scale, or
   environment that proves the claim.
3. **Support labels**: 0-3 short badges/callouts with exact specs or proof.
   Use no support labels when the photo or detail already proves the point.

Before writing any image prompt, make a **layout decision**, the way a human
designer would:

- Text role: no headline / small caption / large headline / large spec number /
  callout-only. Do not force a headline on lifestyle, macro, or clean dimension
  images when the visual proof is stronger without it.
- Text placement: choose a reserved text zone such as top-left, side band, lower
  strip, open wall area, or no text zone. Do not place text over the user's body,
  product contact points, adjustment holes, handles, feet, or key details.
- Typography treatment: define font weight, relative size, line breaks, and
  hierarchy. Use colour only to separate meaning, such as a key number, material,
  or action word; avoid styling every word equally.
- Text background: decide whether text needs a subtle translucent panel,
  solid block, white space, or no background. Use a background only to improve
  readability, not as decoration.
- Visual text system: choose a varied treatment across the set. Do not make
  three or more images in a row use the same black-text-on-white small labels.
  Mix appropriate treatments such as large two-tone headline, small caption,
  side ribbon, colour band, circular detail label, dimension label, icon + text,
  or text-free lifestyle scene.
- Product/person relationship: decide whether the product is unobstructed hero,
  action partner, background context, or macro source. People must support the
  selling point and must not hide structural proof.
- Detail expression: decide whether the detail should be a real close-up,
  connected inset, crop, cutaway, or callout point. Details must point to real
  visible parts and answer a buyer concern.
- Dynamic expression: choose the physical effect to show: correct user contact,
  base pressure, adjustment range, footprint, comparison state, hand scale,
  opened/closed state, pour/flow, load, texture, or usage sequence. Avoid adding
  motion devices that do not prove the selling point.

Amazon secondary-image headline style:

- Prefer concrete feature-benefit noun phrases over generic advertising
  sentences. The headline should sound like Amazon carousel copy, not a brand
  poster slogan.
- Large headlines are allowed when the slot needs a strong claim. Make them feel
  human-designed: reserve a clean title zone, use deliberate line breaks, italic
  or condensed bold type when suitable, and use a flexible colour hierarchy:
  one product/brand/category accent colour plus one neutral text colour. Add a
  smaller subheadline only when it clarifies fit, compatibility, or proof. A large
  headline should never sit randomly over the product or cover the selling detail.
- Use product-specific words, mechanism words, or exact specs when known:
  "4-in-1 Training Station", "Adjustable Height Range", "Wide Anti-Slip Base",
  "Compact Home Gym Footprint", "Reinforced Steel Frame".
- Avoid generic lifestyle slogans such as "Train More At Home", "Fits Your Home
  Space", "Upgrade Your Workout", "Built For Your Life", or "Home Fitness Made
  Easy" unless paired with a specific proof point.
- For dimension images, use utility-style headlines such as "Home Gym Fit
  Guide", "Check Your Workout Space", or "Compact Footprint" instead of vague
  fit claims.
- For lifestyle images, use the action or product role as the headline, e.g.
  "Pull-Up Training Station" or "Dip Station For Home", not motivational copy.

Headline examples by category logic:

- Instead of "Details": "Built To Stay Stable"
- Instead of "Dimensions": "Fits Your Home Gym"
- Instead of "Adjustable Height": "Adjusts For Every User"
- Instead of "Waterproof": "Keeps Gear Dry In Rain"
- Instead of "Large Capacity": "Stores More In Less Space"
- Instead of "Fast Heating": "Ready To Cook In Minutes"

Do not generate an image whose big headline, visual proof, and support labels
are not aligned. If the headline says stability, the image must show stability,
not only a product render with a stability badge.

Use dynamic expression for static generated images:

- Show process with ghosted positions, motion arrows, step overlays, before/after
  splits, height-range trails, action silhouettes, or inset magnifiers connected
  to the full product.
- For adjustable products, show the range as movement: low-to-high ghosted bars,
  sliding arrows, user-height silhouettes, and the lock/adjust mechanism.
- For dimension images, combine exact dimensions with room fit or adjustable
  range, not only front-view measurement arrows.
- For foldable or compact-storage products, first determine whether the uploaded
  product is already folded or unfolded. If it is already folded, keep that
  exact state and show compact storage through wall/corner placement, floor
  footprint overlay, room-scale context, or mechanism inset. If both states are
  needed but only one is visible, use a subtle ghosted outline only when it can
  be inferred from the real mechanism; label inferred states as visual
  explanation, not exact product shape.
- For strength/stability, show the claim being visually tested or explained:
  user action, base contact, reinforced structure, load badge, and proof detail
  in one composition.

If the image idea feels monotone, enrich it before generating. Do not add random
decoration; add useful layers that strengthen the same core viewpoint:

- **Fuse related information**: combine claim + proof + mechanism instead of
  putting each in isolated badges.
- **Create depth**: use foreground detail, main product/action in the middle,
  and scene/context or soft graphic shapes in the background.
- **Add contrast**: before/after, low/high, stable/unstable, empty/filled,
  ordinary/enhanced, indoor/space fit.
- **Connect details to the whole**: use magnifier insets, callout lines, cutaway
  highlights, or zoom windows tied to the full product.
- **Use scale and human context**: hand, body silhouette, room footprint, common
  object comparison, or product-in-use posture.
- **Vary layout across the set**: avoid producing multiple centered product
  images with badges. Mix hero action, technical range, proof-under-use, room
  fit, comparison, and mechanism explanation.

Layering check before generation:

- Foreground: what grabs attention first?
- Middle ground: where is the product or action proof?
- Background: what context supports the claim without clutter?
- Text/annotation hierarchy: no text, small caption, headline, large spec,
  dimension labels, or callouts only, whichever best expresses the selling point.

Detail/close-up images are optional. Generate AS-06 only when the details add
new buying confidence that is not already proven in other images. If the same
detail is already used in a feature, stability, or material-proof image, skip
AS-06 or integrate the detail as an inset there. Avoid generic 2x2 macro grids
when the product details do not create a stronger selling story.

For image requests, translate the buyer-concern map into a shot plan before
generation:

- Slot id and image type
- Core selling point / viewpoint
- Buyer question being answered
- Main visual proof, not just text
- Dynamic device: action, arrow, ghosted state, before/after, scale, or inset
- Layout decision: text role, text placement, typography hierarchy, text
  background, product/person relationship, detail expression, and dynamic proof
- Layering plan: foreground, middle ground, background, communication hierarchy
- Scene/background and visual intensity
- Exact short text labels to place on the image
- Negative constraints: what must not appear

Use creative auto-completion for image planning. If the user provides only a
product photo, product type, and basic selling points, infer an appropriate
Amazon secondary-image composition, visual hierarchy, background, lighting,
icons, short callout copy, and scene context. Prefer clean commercial layouts
that make the product and benefit legible at thumbnail size.

Default to **credible commercial Amazon visuals**, not plain catalog layouts or
AI-poster layouts:

- Use high contrast between product and background, warm directional lighting,
  clear shadows, depth, large product scale, and confident but restrained
  typography.
- Use lifestyle action, dramatic close-ups, dimension arrows, or comparison
  devices to prove the benefit visually.
- Avoid flat grey backgrounds for every image; reserve them for technical slots
  only, and add accent colour, depth, or context when the user asks for a style
  such as "家庭风格" or "暖色".
- Keep text minimal, but make the few words large and high-impact.
- For real-home styles, the image should feel like a high-quality product photo
  with light commercial layout added afterwards, not a CGI fitness poster.

User-specified art direction overrides auto-completion. Preserve explicit
requests for style, scene, colour palette, language, target user, background,
layout, angle, size labels, and text placement. If the user says "家庭风格",
"厨房场景", "黑金风", "不要文字", "只要英文", "中文文案", "中英双语",
or similar, incorporate that constraint into the relevant prompt instead of
using the default style.

When the user asks for "家庭暖色", "家庭风格", "暖色家居", "home warm style",
or similar, use the **Warm Real Home Ecommerce** preset unless they explicitly
request a poster, neon, gaming, cyber, or black-gold style:

- Scene: realistic lived-in home fitness corner, warm wood floor, cream or warm
  neutral wall, natural window light, soft shadows, simple mat, plant, shelf, or
  sofa edge only when it helps scale and realism.
- Lighting: warm daylight or late-afternoon natural light with believable
  shadows. Avoid fake glow, neon rim light, gold light trails, lens flare, and
  fantasy energy effects.
- Dynamic visual proof: use real physical events such as a correct exercise
  action, hand contact, base pressure, floor footprint, perspective dimension
  lines, range ghosting for product parts only, foreground macro, or before/after
  room composition.
- Typography: choose text only after the layout is clear. Some warm home images
  should have no headline or only a small caption; others can use a large
  confident Amazon headline when the claim needs it. If a large headline is used,
  reserve a clean title zone, use clear line breaks, and use 1-2 colours for
  hierarchy. Choose the accent colour from the product, brand, category norm,
  scene, or user reference image; do not default to orange/black unless that fits
  the product or provided direction. Use concrete Amazon-style feature-benefit
  phrases instead of generic lifestyle slogans.
- Labels: vary the label system based on image role: thin callout lines, compact
  spec chips, coloured side bands, circular detail labels, icon + text stacks, or
  subtle text panels. Avoid repeating black text on white tags across the set,
  and avoid black-gold pill buttons, glowing outlines, decorative icon stacks, or
  UI panels that make the image look like a template.
- People: use realistic fitness models only where the buyer needs action proof.
  Avoid bodybuilder-poster poses, plastic skin, duplicated ghost athletes, and
  poses that cover the product or distort the frame.

Add this negative constraint to every Warm Real Home Ecommerce prompt:
"Avoid AI poster aesthetics: no neon glow, no energy rings, no fantasy light
trails, no black-and-gold button badges, no random oversized motivational
typography that lacks a reserved title area, no CGI showroom, no plastic-smooth
skin, no duplicated ghost athletes, no product-obscuring bodies. Keep the image
grounded in realistic warm home photography with purposeful Amazon ecommerce
annotations."

Visible image text language rules:

- Default to English for Amazon US or when the user says "Amazon.com", "US",
  "美国站", or "只要英文".
- Use Chinese visible text when the user says "中文文案", "图片文字用中文",
  "中文副图", "中文标注", "国内平台", or similar.
- Use bilingual Chinese-English text only when the user explicitly asks for
  "中英双语" or "双语". Keep bilingual text sparse because it increases visual
  density.
- If the marketplace is unclear, infer from the user's requested language and
  product context. State the language assumption before generation when it may
  affect the output.
- Keep specs exact in any language. Translate meaning, not unit values. Do not
  invent claims while translating.

Assume anything missing and state assumptions clearly. Do not ask questions
first unless the request is impossible without a product category.

Resolve bundled resources relative to this skill folder. Do not assume the
current shell directory is the skill folder.

Execution flow:

- Copy only: run STEP 1 and STEP 2, then stop.
- Image set only or one image slot: skip STEP 1 and STEP 2 unless the user also
  asks for listing copy; run STEP 3 directly.
- Detail page / A+ image set: skip AS secondary-image slot planning unless the
  user also asks for副图; run STEP 3B.
- Full kit: run STEP 1 through STEP 5.

---

## STEP 1 — Generate Listing copy

**Title rules:**
- `[Brand] [Core Keyword] [Key Attribute] – [Differentiator], [Context]`
- Hard limit: 150 characters. Primary keyword in first 80 chars.
- Capitalise Every Main Word (skip: and/for/the/with/in/of/a)
- No ALL-CAPS words, no special chars (!$?_{}^), no promo words (Best/Free/Sale)

**5 Bullets — formula per bullet:**
`【ALL-CAPS LABEL】Benefit statement. Feature/spec support. Context or proof.`
- 150–200 chars each. COSMO dimension per bullet:
  - B1: capableOf + causes (function + problem solved)
  - B2: hasProperty + distinguishedFrom (material/spec + vs competitors)
  - B3: suitableFor + usedInContext (who + where)
  - B4: motivatedBy + distinguishedFrom (why buy + differentiation)
  - B5: partOf + relatedTo (what's included + guarantee)

**Description — 5 paragraphs, 1500-2000 chars total:**
1. Pain-point hook (emotional, 2-3 sentences)
2. Product as solution (introduce product + core benefit)
3. Feature deep-dive (expand bullets, add secondary keywords naturally)
4. Use-case expansion (3-4 distinct usage scenarios)
5. Brand promise / guarantee close

**Backend Search Terms:**
- Space-separated ONLY. No commas, quotes, repeated words.
- Hard limit: 250 bytes (exceeding invalidates ALL terms — count carefully)
- Include: synonyms, misspellings, Spanish variants (US market), conversational phrases

---

## STEP 2 — Display listing

Output in this format so each section is easy to copy:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🛒  AMAZON LISTING  ·  [Product Name]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📌  TITLE  ([X] chars)
[title]

📋  BULLET POINTS
①  [bullet 1]
②  [bullet 2]
③  [bullet 3]
④  [bullet 4]
⑤  [bullet 5]

📝  DESCRIPTION
[full description]

🔑  BACKEND SEARCH TERMS  ([X] bytes)
[space-separated terms]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

If in full-kit mode, continue to the image prompt plan. If in copy-only mode,
stop after displaying the listing unless the user explicitly requested export.

---

## STEP 3 — Build image prompts and optionally generate images

Build prompt text for the selected AS slots before generating images. For full
8-image requests, build AS-02 through AS-09. For recommended image sets, build
only the 5-7 slots that answer real buyer questions. Include this product
consistency rule in every prompt:
> "Use the uploaded white-background product photo as exact visual reference.
> Product shape, colour, proportions, and design details must match precisely."

Do not default to a text-heavy collage. For production assets, generate separate
secondary images unless the user explicitly asks for a storyboard or direction
board. Each image should have one main message, large product/action visuals,
and short callouts instead of paragraphs. Keep visible text minimal: headline +
2-4 short labels or badges.

Count discipline:

- Full 8-image requests generate exactly **8 separate images**: AS-02, AS-03,
  AS-04, AS-05, AS-06, AS-07, AS-08, and AS-09.
- Broad secondary-image requests generate the strongest recommended set, usually
  5-7 images. Do not fill weak slots just to reach 8.
- Do not combine the full set into a single collage unless the user asks for a
  direction board.
- If generating fewer than 8 for a broad image-set request, briefly state the
  selected slots and why omitted slots were skipped before generation.
- If only 3-4 images are generated because of a preview request, state that it
  is a preview before generation.
- When using an image-generation tool that returns one image per call, call it
  once per required slot.

For each prompt, add any inferred or user-specified style guidance. If the user
does not specify style, choose a suitable ecommerce style automatically:
technical and clean for dimension/function diagrams, warm realistic interiors
for lifestyle scenes, crisp macro lighting for detail grids, and simple
high-contrast tables for comparisons.

Visual intensity requirement:

- Every image must be thumbnail-legible: product/action occupies the visual
  center, headline is short, and labels are readable.
- Use stronger composition than a plain white/grey product render: foreground
  scale, real action, perspective depth, before/after contrast, clean dimension
  lines, inset magnifiers, or human action where appropriate.
- "Dynamic" means a visible buyer-relevant event is happening: a user is gripping
  the correct bar, the base is shown under pressure, the height range is shown
  with a product-part trail, the footprint is anchored to the floor, or a detail
  is connected to the full product. It does not mean decorative glow, energy
  rings, fantasy motion trails, or oversized poster typography.
- For "家庭风格" use a warm real home-gym/living-room fitness environment with
  wood floor, warm wall colour, natural light, and clean lifestyle props, while
  keeping the product as the hero. For "家庭暖色", apply the Warm Real Home
  Ecommerce preset above.
- For fitness equipment, include people only in scene/use images or small action
  silhouettes; keep technical images product-led.
- For fitness multi-action images, protect product geometry first. The product
  must stay as one rigid object with straight uprights, correct bars, correct
  handle locations, and stable base proportions. Do not let bodies bend, stretch,
  duplicate, or move product parts. If showing several exercises, prefer a
  workout-flow band, numbered vignettes, or separate action zones. Each pose must
  visibly grip, press, hang from, or align with the correct product part.
- Do not let technical images become static. Use arrows, range trails,
  magnified mechanisms, pressure/contact cues, or comparison states to make the
  benefit feel active and understandable.

Call image generation when the user requested the full kit, image set only, or a
specific image slot. If image generation is unavailable, output the prompts and
still write them into the Excel file when Excel output was requested.

**AS-02 — 核心卖点图 (Key Benefits)**
```
$imagegen [product matching reference photo] as the dominant visual.
This is the conversion hook, not a summary of all specs. Choose the strongest
buyer-facing promise and make it visually obvious through action, scale,
before/after, or multi-use composition. Use one bold headline and only 2-3
supporting badges. Avoid generic lines like "stable home training" unless the
visual clearly proves stability. Strong Amazon hero composition: large product,
warm commercial background, high contrast, confident but restrained headline,
shadow and depth. For Warm Real Home Ecommerce, use a real home fitness corner,
one believable action moment, and light ecommerce annotations. Avoid motivational
poster composition, giant stacked typography, glow rings, motion-trail athletes,
black-gold button badges, and bodies that hide the product. Headline should be
a concrete product-role or feature-benefit phrase, such as "4-in-1 Training
Station", "Pull-Up & Dip Station", or "Full-Body Home Workouts"; avoid generic
slogans such as "Train More At Home".
Product must exactly match the uploaded reference photo.
```

**AS-03 — 功能拆解图 (Feature Breakdown)**
```
$imagegen [product matching reference photo] centred, 3/4 angle.
Thin callout lines with circle dots pointing to only the 4-6 most important
physical features from the buyer-concern map. Labels are 2-4 words each.
Warm technical ecommerce style with either a restrained beige/light grey
background or a realistic warm home background when requested. Make a deliberate
designer layout: either a large two-tone feature headline in a reserved title
zone, or a smaller title plus coloured side-band labels. Do not default to all
black text on white tags. Use varied Amazon-style callouts such as accent-colour
side ribbons, circular detail markers, thin lines, and connected inset
magnifiers for important details. Lines balanced. Avoid a flat low-energy
diagram, but also avoid glowing arcs, neon outlines, black-gold UI buttons,
heavy decorative icon stacks, and random poster typography.
Product must exactly match the uploaded reference photo.
```

**AS-04 — 尺寸参数图 (Dimensions)**
```
$imagegen [product matching reference photo] front/side or 3/4 view.
Engineering double-arrow dimension lines for [key dimensions]. If height is
adjustable, show it dynamically with ghosted low/high positions, vertical range
trail, upward arrow, and/or user-height silhouettes.
Use exact user-provided dimensions when available. If dimensions are missing,
infer only approximate visual proportions and label every inferred size as approx.;
never present estimated dimensions as exact product specifications.
Use a small spec badge or mini table with only the dimensions and 1-2 specs
needed for space planning.
Default to white or light grey technical background unless the user requests a
scene style such as home, kitchen, bathroom, garage, or outdoor. Monospace
numbers. Dual units cm and inch. Add subtle room-scale context when helpful so
buyers understand home fit.
For Warm Real Home Ecommerce, anchor measurements to a believable room floor:
use a non-glowing footprint rectangle, perspective dimension lines, a realistic
scale silhouette, and restrained labels. Avoid luminous floor grids, neon arrows,
oversized labels, and fake exact numbers. Headline should be utility-style and
specific, such as "Home Gym Fit Guide", "Check Your Workout Space", or "Compact
Footprint"; avoid vague slogans such as "Fits Your Home Space".
Product must exactly match the uploaded reference photo.
```

**AS-05 — 场景使用图 (Lifestyle)**
```
$imagegen [target user from selling points] using [product matching
reference photo] in [natural setting appropriate for this product type].
Warm natural lighting, stronger commercial contrast, authentic action pose,
slight depth of field. Real environment, not a studio. Show the exercise action
first and the product fully visible/stable; then optionally add 1-2 small
benefit labels. Product colour/shape matches reference.
For Warm Real Home Ecommerce, make the scene look like realistic home fitness
photography: warm daylight, wood floor, lived-in but tidy room, restrained text,
and no fantasy glow, energy rings, duplicated action trails, or motivational
poster wall text.
Use one primary athlete action only unless the user explicitly asks for multiple
actions. The athlete must physically connect to the correct product contact
points: hands gripping the actual pull-up bar or dip handles, feet clear of
base bars unless the exercise requires contact, no limbs passing through the
frame. Keep the product rigid and undistorted.
```

**AS-06 — 细节特写图 (Detail Macro 2x2)**
```
$imagegen close-up detail image only if the details communicate new buying
confidence not already covered elsewhere. Prefer one integrated composition:
full product context plus 2-4 connected magnifier insets, or a dynamic detail
story such as base contact under pressure, adjustment hole/lock mechanism, grip
texture in use, or reinforced tube connection. Use 2x2 macro grid only when all
four panels are meaningful and distinct. Each label is 2-4 words. Details match
reference photo. For Warm Real Home Ecommerce, use real macro photography cues:
natural warm light, floor contact, visible texture, enlarged real part crops,
soft circular or rounded zoom windows, and labels integrated into the crop.
Detail images should feel comfortable and product-real: large macro part, clear
connection to the full product, soft warm or white background, and 1-3 label
styles that match the detail. Avoid making every label a black-on-white tag.
Avoid generic parts, repeated proof points, decorative macro panels, glowing
pressure rings, neon outlines, and black-gold label boxes.
```

**AS-07 — 竞品对比图 (Comparison)**
```
$imagegen Clean comparison table infographic.
Title: "WHY CHOOSE [product type]?"
Left "OUR PRODUCT" highlighted background, right "OTHERS" grey.
3-5 rows comparing only buyer-relevant differences from the concern map.
Left column ✓ checkmarks. Right column ✗. No competitor brand names.
Bold sans-serif. High-contrast Amazon infographic style with a product hero or
detail crop, not a plain spreadsheet.
```

**AS-08 — 使用步骤图 (How to Use)**
```
$imagegen Step-by-step diagram. 4 panels horizontal connected by arrows.
Large step numbers 1-2-3-4 with short captions (4-6 words each):
[logical usage steps for this product type].
Each panel shows product at that stage. Clean warm infographic with strong
visual hierarchy, not small low-contrast thumbnails. Product matches reference
photo.
For multi-function training, this slot may become a "training modes" module
instead of literal usage steps. Use separated panels or clearly spaced action
vignettes so each exercise has its own product copy or its own action zone.
Do not stack several full athletes on one central product if that causes
misalignment. Label each action only after verifying the pose uses the correct
bar/handle/base point.
For Warm Real Home Ecommerce, keep panels photographic and restrained: warm home
background, consistent product geometry, simple separators, small labels, and
realistic poses. Avoid comic-book panels, glowing arrows, ghost athletes,
oversized step numbers, and black-gold UI styling.
```

**AS-09 — 包装全家福 (What's in the Box)**
```
$imagegen Flat-lay overhead on pure white background.
All items laid out: [product from reference], [accessories from selling points
or standard for product type], [packaging if applicable].
Each item has a label line. "WHAT'S IN THE BOX" bold title at top.
Overhead studio lighting. Symmetric organized layout.
Product matches reference photo.
```

---

## STEP 3B — Build detail-page / A+ image modules

Use this section only when the user asks for "详情图", "详情页", "A+页面",
"A+ Content", "EBC", or product-detail long images.

Detail-page images are separate from Amazon secondary images:

- **AS slots**: carousel/listing secondary images near the main image.
- **AD modules**: detail-page/A+ modules lower on the product page.

Default to 5-7 AD modules. Do not force a module if it repeats the carousel.
Use wider, more editorial compositions than AS images. Text can be slightly
more explanatory than carousel images, but still avoid paragraph-heavy layouts.
Each module should have one section message and one clear visual proof.

Detail-page modules must feel richer than carousel secondary images. Do not
make a set of ordinary banners with empty backgrounds and one isolated model.
Before generating each AD module, make the same human design decisions required
for AS images, but at a wider/editorial scale:

- Module role: hero, problem/solution, proof, structure/detail, use-case,
  specification, setup, or FAQ.
- Text system: no headline / large headline + subheadline / large spec /
  callout-only / comparison labels. A+ can use more text than AS, but the text
  must be grouped into a designed title zone, side panel, spec block, or caption
  band.
- Colour system: choose one accent colour from product, brand, scene, category,
  or reference image plus neutral text. Do not default to any fixed colour pair.
- Layout rhythm: vary modules across the set. Avoid several consecutive wide
  banners with centered product, black-on-white tags, or identical title
  placement.
- Detail and proof: connect labels, zoom windows, cutaways, icons, and spec
  blocks to real visible product parts or scene evidence.

Before generating each AD module, choose one memorable visual hook:

- **Lifestyle world**: family members, children, pets, furniture, room depth, or
  daily-life props that make the home context feel real. Children and pets can
  appear as atmosphere or scale context, but never show a child using fitness
  equipment unless the product is explicitly designed for children.
- **Physical proof anchored to the product**: if a module mentions footprint,
  show the base on the floor with a highlighted floor rectangle, subtle
  footprint zone, measuring-tape grid, clearance shadow, or furniture-scale
  comparison. If it mentions stability, show force through the frame, base
  pressure, suction-foot contact, or anti-wobble contrast. If it mentions
  adjustability, show the movement path on the upright holes and ghosted
  low/high states.
- **Editorial depth**: foreground detail, middle-ground product/action, and
  background context must all support the same claim. Avoid large unused blank
  areas unless they intentionally protect a strong headline.
- **Contrast device**: before/after, small-space/converted-space,
  unstable/stable, low/high adjustment, single exercise/multiple exercises, or
  ordinary tube/reinforced frame.
- **Connected detail**: use zoom windows, cutaways, overlays, or callout lines
  attached to the real product. Do not place generic floating icons that are not
  tied to visible product parts.

A+ creative gate: every AD module needs **one big idea + one visual event + one
environmental layer**. If the module can be described as "product with a label",
redesign it before generation. Use text to name the conclusion, but let the
scene, overlay, action, or product close-up prove it.

A+ layout quality gate:

- Headline, if used, must have a reserved title zone, deliberate size/line breaks,
  and colour hierarchy. It may be large and bold when it serves the module.
- Support text should be 1 short subheadline or 2-4 short proof labels, not a
  paragraph pasted over the image.
- Avoid repeating the same label style across AD modules. Mix title bands,
  diagonal panels, macro crops, icon rows, comparison panels, spec boards, and
  text-free lifestyle modules when appropriate.
- A+ detail modules should use comfortable enlarged part crops, rounded/circular
  zoom windows, cutaways, or connected insets, not a rigid grid of tiny labels.

Suggested AD module map:

- **AD-01 — Brand/value hero**: wide lifestyle banner; product in the target
  environment; concise value statement. Build a full home training atmosphere,
  not an empty room: family member watching, child/pet nearby as lifestyle
  context, warm furniture, mat, water bottle, sunlight, and room depth. Keep the
  product/action as the hero, but use family-life elements to make the image
  emotionally warmer and less sparse.
- **AD-02 — Problem/solution / space transformation**: show the buyer pain point
  and how the product solves it in one strong visual. If home footprint is the
  proof, anchor the claim to the base with a highlighted floor footprint,
  subtle rectangle, measuring grid, tape marks, or furniture clearance. Avoid a
  weak split screen where both sides look similar.
- **AD-03 — Core selling-point module**: combine related claims and proof points,
  e.g. load capacity + material thickness + reinforced frame. Show force moving
  through the frame, reinforced structure highlights, base pressure/contact, and
  connected tube-thickness detail, instead of only listing "150 kg" and
  "1.2 mm".
- **AD-04 — Product structure/detail module**: larger callout diagram or macro
  detail grid explaining important mechanics. Use connected zoom insets tied to
  the product, and make each inset answer a different concern: grip comfort,
  adjustment lock, dip handle contact, suction-foot stability, fastener/frame
  joint. Prefer one large hero detail plus 2-3 connected support details over a
  generic grid. Skip any inset that repeats another module.
- **AD-05 — Use-case module**: multiple scenes or actions showing how the
  product fits real daily use. Use a workout-flow composition, split scene,
  diagonal collage, radial action map, or multi-scene band; bodies must align
  correctly with bars and handles. The
  product may be smaller than in carousel images if action clarity is the point.
  For complex multi-action modules, split exercises into separate zones,
  repeated mini product views, or numbered action panels. Do not force
  pull-up, dip, knee raise, and push-up bodies onto one central product if it
  creates product distortion or wrong body/product contact.
- **AD-06 — Size/specification module**: dimensions, compatibility, fit, or
  model/spec table if those facts affect purchase confidence. Specs must be
  shown on the real product or floor plane: footprint on the base, height range
  on uprights, user/family silhouettes for scale, and exact numbers in a small
  clean spec block.
- **AD-07 — Setup/care/FAQ module**: optional only when installation, use steps,
  maintenance, or common objections are meaningful.

For detail-page modules, classify selling points the same way as secondary
images, but allow richer storytelling:

- Headline: buyer-facing result.
- Subheadline: 1 short supporting sentence when needed.
- Visual proof: real product scene, structural callout, spec table, or macro.
- Proof labels: exact specs from the user, not invented claims.

A+ module prompt checklist:

- Specify wide module format such as 970x600, 1464x600, or vertical long-module
  only when requested.
- State what fills foreground, middle ground, and background.
- State the exact product part that proves each spec.
- State the module's layout decision: text role, title zone, colour hierarchy,
  label style, product/person relationship, detail expression, and dynamic proof.
- Allow family, children, pets, or daily-life objects only when they support the
  buyer concern; keep them secondary to the product.
- Keep visible text designed and grouped: headline or large spec if useful,
  optional short subheadline, and 1-4 proof labels. Do not paste paragraphs over
  the image.
- Avoid making every module a centered product hero. Vary layout: cinematic
  home scene, floor-footprint proof, structural overlay, connected detail
  diagram, workout-flow scene, and specification board.

Do not use "详情图" to mean AS-06. If the user asks for "详情图里的细节模块",
generate AD-04, not AS-06, unless they explicitly say "副图细节图".

**After image generation or prompt output:** remind the user:
> ⚠️ AM-01 主图：使用真实拍摄白底照片，并在上传前复核 Amazon 当前图片规则。

When Excel output is requested, produce a temporary prompts JSON file keyed by
slot id. Include all selected slots and mark omitted optional slots as skipped
with a short reason:

```json
{
  "AM-01": "Real white-background product photo required; do not generate.",
  "AS-02": "...",
  "AS-03": "...",
  "AS-04": "...",
  "AS-05": "...",
  "AS-06": "...",
  "AS-07": "...",
  "AS-08": "...",
  "AS-09": "Skipped: no packaging or included accessory information provided.",
  "AV-01": "..."
}
```

---

## STEP 4 — Save Excel summary

Skip this step in copy-only mode and image set only mode unless the user
explicitly asks for Excel, export, save, table, or a downloadable summary file.

Use the script from this skill's `scripts/` directory. Pass `--prompts-json`
when image prompts were produced.

```bash
python3 /path/to/amazon-listing-generator/scripts/generate_excel.py \
  --product "[product name]" \
  --title "[title]" \
  --b1 "[bullet1]" --b2 "[bullet2]" --b3 "[bullet3]" \
  --b4 "[bullet4]" --b5 "[bullet5]" \
  --description "[description]" \
  --backend "[backend terms]" \
  --prompts-json "[prompts.json]" \
  --output "Amazon_Listing_[ProductSlug].xlsx"
```

---

## STEP 5 — Done message

Adapt the done message to the mode. Do not claim images or Excel were generated
when the user requested copy-only output.

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅  按本次模式输出完成
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📄  Listing文案（Title + 5 Bullets + Description + Backend）[done/skip]
🖼️  副图推荐集（5-7张或指定slot）[done/skip]
📊  Excel汇总文件 [done/skip]
⚠️  主图 AM-01：需真实拍摄白底图

修改说："把场景图改成户外" / "Title加品牌名xxx" / "重新生成对比图"
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## COSMO dimensions (cover ≥8 across all copy)

isUsedFor / capableOf / suitableFor / hasProperty / partOf / relatedTo /
causes / motivatedBy / usedInContext / complementedBy / distinguishedFrom /
associatedWith / instanceOf / preconditionOf / enabledBy

---

## Quality gates (check before finishing)

- [ ] Title ≤150 chars, primary keyword in first 80
- [ ] Listing copy checks apply only when listing copy is generated
- [ ] Each bullet 150-200 chars with ALL-CAPS label when bullets are generated
- [ ] Description ≥1500 chars, 5 paragraphs when description is generated
- [ ] Backend ≤250 bytes, space-separated, no repeats when backend terms are generated
- [ ] ≥8 COSMO dimensions covered when listing copy is generated
- [ ] Image generation matches requested mode; copy-only requests do not force images
- [ ] Broad "副图" requests produce a justified 5-7 image set, not a weak forced 8
- [ ] Exact "完整8张" requests produce AS-02 through AS-09 as 8 separate images
- [ ] Preview batches under 5 images are generated only when explicitly requested
- [ ] Every image has a buyer concern, visual proof, and strong thumbnail hierarchy
- [ ] Every image makes a deliberate layout decision: text/no text, text position, typography hierarchy, text background, product/person relationship, detail expression, and dynamic proof
- [ ] Each image expresses its selling point correctly even if it has no headline; the product, person, detail, or annotation must carry the claim
- [ ] Visible text, when used, has human-designed size, position, spacing, colour hierarchy, and optional background support; it does not feel auto-placed
- [ ] Visible headlines use concrete Amazon-style feature-benefit copy, not generic poster slogans such as "Train More At Home" or "Fits Your Home Space"
- [ ] A multi-image set varies its text/label system; do not repeat black text on white tags across several images when bolder Amazon-style bands, icons, circles, dimension labels, or large specs would fit better
- [ ] Fitness-equipment images may use bold accent-colour + neutral headline hierarchy, large spec numbers, side ribbons, circular icons, diagonal detail panels, or large part close-ups when they serve the selling point; accent colour is selected from product/brand/category/reference context, not fixed to orange/red
- [ ] Warm-home images follow Warm Real Home Ecommerce: realistic home photo feel, restrained annotations, natural warm light, no CGI showroom look
- [ ] Images do not show AI-poster artifacts: neon glow, energy rings, fantasy light trails, black-gold button badges, oversized motivational typography, duplicated ghost athletes, or product-obscuring bodies
- [ ] Product current state is protected; foldable/storage claims do not compress, shrink, or invent a more extreme product state
- [ ] Multi-action fitness images keep the product rigid and anatomically plausible; each body pose connects to the correct bar, handle, or base point
- [ ] Detail-page/A+ modules are not ordinary empty banners; each has one big idea, one visual event, and one environmental layer
- [ ] A+ specs are anchored to visible product parts or floor/room context, such as footprint on the base or load proof through the frame
- [ ] User style constraints such as "家庭风格" and "暖色" are visible in the prompt
- [ ] Visible image text language follows user request or marketplace assumption
- [ ] Excel generation matches requested mode; copy-only requests do not write files unless export is requested
- [ ] AM-01 warning shown
- [ ] Excel file written only when the user requested export or full-kit output
