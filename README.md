# Amazon Listing Generator Skill

Codex skill for creating Amazon listing assets from a product photo and selling
points. It can generate listing copy, secondary-image plans/prompts, optional AI
secondary images, A+ detail-page modules, and an Excel summary.

## What It Does

- Generates Amazon title, 5 bullets, description, and backend search terms.
- Plans Amazon secondary images from product facts and buyer concerns.
- Supports single image slots such as dimensions, lifestyle, comparison, or
  feature breakdown.
- Separates carousel secondary images from detail-page / A+ modules.
- Uses competitor or reference images as inspiration without copying them.
- Groups selling points by buyer logic instead of making one image per spec.
- Emphasizes clear headline, visual proof, dynamic composition, and strong
  commercial hierarchy for every image.

## File Structure

```text
amazon-listing-generator-skill/
├── SKILL.md
├── agents/
│   └── openai.yaml
├── references/
│   └── amazon-image-strategy.md
├── scripts/
│   └── generate_excel.py
└── README.md
```

## Install

Clone the repository:

```bash
git clone git@github.com:smilexiaobao1992/amazon-listing-generator-skill.git
```

Install into your Codex skills directory:

```bash
mkdir -p ~/.codex/skills
cp -R amazon-listing-generator-skill ~/.codex/skills/amazon-listing-generator
```

If you already have an older local version, replace it:

```bash
rm -rf ~/.codex/skills/amazon-listing-generator
cp -R amazon-listing-generator-skill ~/.codex/skills/amazon-listing-generator
```

Validate the skill if you have the system skill creator scripts available:

```bash
python3 ~/.codex/skills/.system/skill-creator/scripts/quick_validate.py \
  ~/.codex/skills/amazon-listing-generator
```

## Update

From the cloned repository:

```bash
git pull
rm -rf ~/.codex/skills/amazon-listing-generator
cp -R . ~/.codex/skills/amazon-listing-generator
```

## Basic Usage

Invoke the skill in Codex with a product photo and selling points:

```text
[$amazon-listing-generator](~/.codex/skills/amazon-listing-generator/SKILL.md)
请根据白底图和卖点生成Amazon副图
```

A typical request:

```text
[$amazon-listing-generator](~/.codex/skills/amazon-listing-generator/SKILL.md)
请根据白底图，生成对应副图：
尺寸82 x 78 x 155-210cm，10档可调节，承重150kg，
钢材1.2mm，防滑吸盘底座，可以做引体向上等多种功能训练。
家庭风格，暖色。
```

## Supported Request Types

### Listing Copy

```text
只生成Listing文案
根据白底图和卖点生成Amazon listing
帮我写标题、五点、描述、后台词
```

### Secondary Image Set

```text
根据白底图和卖点生成副图
生成对应副图，家庭风格、暖色
生成Amazon副图，参考竞品
完整8张，AS-02到AS-09都要
先出4张预览
```

By default, broad secondary-image requests produce a recommended 5-7 image set.
The skill only creates all 8 AS slots when explicitly requested.

### Single Secondary Image

```text
只生成AS-04尺寸图
重新生成AS-07对比图
生成一张场景图，家庭暖色
只要卖点图
生成单张防滑底座细节图
把AS-05改成户外场景
```

### Detail Page / A+ Modules

```text
生成详情图
生成详情页6张模块图
生成A+页面图
只生成AD-03卖点详情模块
生成AD-04产品结构详情模块
```

### Reference Image Or Competitor Direction

```text
参考这张图的感觉重新做AS-04
给你一张竞品副图参考，不要照抄，按我们的产品改
参考这个排版，但卖点用我们的
照这个风格做一张功能图
看看Amazon前几名怎么做
参考这个Amazon链接优化副图
```

### Style Controls

```text
家庭风格、暖色
黑金风
户外场景
厨房场景
只要英文
中文文案
图片文字用中文
中英双语
不要文字
高端商业感
更有动态感
视觉更强烈
```

Visible text defaults to English for Amazon US. Ask for Chinese or bilingual
copy explicitly when needed:

```text
生成中文副图
图片文字用中文
尺寸图用中文标注
标题和标签都用中文
生成中英双语副图
```

## Slot Reference

### Amazon Secondary Images

```text
AS-02 核心卖点图
AS-03 功能拆解图
AS-04 尺寸参数图
AS-05 场景使用图
AS-06 细节/局部放大图
AS-07 对比图
AS-08 使用步骤图
AS-09 包装全家福
```

### Detail Page / A+ Modules

```text
AD-01 品牌/价值主视觉
AD-02 痛点/解决方案
AD-03 核心卖点详情模块
AD-04 产品结构/细节解释模块
AD-05 使用场景扩展
AD-06 尺寸/规格/适配模块
AD-07 安装/维护/FAQ
```

## Design Logic

The skill follows these principles:

- Understand the product before writing image prompts.
- Understand buyer intent and the concern each image should resolve.
- Use one clear headline/core viewpoint per image.
- Prove the headline visually with action, scale, mechanism, comparison, or
  scene context.
- Combine related facts such as claim + proof + mechanism.
- Avoid repeated images and avoid filling weak slots just to hit a count.
- Use reference images and competitors as strategic input, not as templates to
  copy.
- Keep secondary images punchy and thumbnail-legible.
- Keep detail-page modules richer and more editorial.

## Excel Output

The included script can generate an Excel summary when the skill produces
listing copy or prompt plans:

```bash
python3 scripts/generate_excel.py \
  --product "Product Name" \
  --title "Amazon Title" \
  --b1 "Bullet 1" \
  --b2 "Bullet 2" \
  --b3 "Bullet 3" \
  --b4 "Bullet 4" \
  --b5 "Bullet 5" \
  --description "Description" \
  --backend "backend search terms" \
  --output "Amazon_Listing_Product.xlsx"
```

## Development

Validate after editing:

```bash
python3 ~/.codex/skills/.system/skill-creator/scripts/quick_validate.py .
```

Commit and push:

```bash
git status
git add .
git commit -m "Update amazon listing generator skill"
git push
```
