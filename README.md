# Amazon Listing Generator Skill

这是一个用于生成 Amazon Listing 资产的 Codex skill。它可以基于产品白底图和卖点信息，生成 Listing 文案、副图规划与提示词、可选 AI 副图、A+ / 详情页模块图，以及 Excel 汇总表。

## 功能说明

- 生成 Amazon 标题、五点描述、长描述、后台搜索词。
- 根据产品事实和买家关注点规划 Amazon 副图。
- 支持单张副图重做，例如尺寸图、场景图、对比图、功能拆解图。
- 区分轮播副图与详情页 / A+ 模块图。
- 支持参考竞品链接或参考图片，但不会照抄。
- 按买家决策逻辑合并卖点，而不是一个参数做一张图。
- 强调每张图都有清晰卖点、版式决策、视觉证明和商业层次；标题是可选表达方式，不是每张图的固定模板。

## 文件结构

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

## 安装方式

先克隆仓库：

```bash
git clone git@github.com:smilexiaobao1992/amazon-listing-generator-skill.git
```

安装到 Codex 的 skills 目录：

```bash
mkdir -p ~/.codex/skills
cp -R amazon-listing-generator-skill ~/.codex/skills/amazon-listing-generator
```

如果本地已经有旧版本，直接替换：

```bash
rm -rf ~/.codex/skills/amazon-listing-generator
cp -R amazon-listing-generator-skill ~/.codex/skills/amazon-listing-generator
```

如果本机有 system skill-creator 校验脚本，可以校验：

```bash
python3 ~/.codex/skills/.system/skill-creator/scripts/quick_validate.py \
  ~/.codex/skills/amazon-listing-generator
```

## 更新方式

在仓库目录执行：

```bash
git pull
rm -rf ~/.codex/skills/amazon-listing-generator
cp -R . ~/.codex/skills/amazon-listing-generator
```

## 基础用法

在 Codex 里配合产品图和卖点调用：

```text
[$amazon-listing-generator](~/.codex/skills/amazon-listing-generator/SKILL.md)
请根据白底图和卖点生成Amazon副图
```

一个典型请求：

```text
[$amazon-listing-generator](~/.codex/skills/amazon-listing-generator/SKILL.md)
请根据白底图，生成对应副图：
尺寸82 x 78 x 155-210cm，10档可调节，承重150kg，
钢材1.2mm，防滑吸盘底座，可以做引体向上等多种功能训练。
家庭风格，暖色。
```

## 支持的请求类型

### 1. Listing 文案

```text
只生成Listing文案
根据白底图和卖点生成Amazon listing
帮我写标题、五点、描述、后台词
```

### 2. 副图整套

```text
根据白底图和卖点生成副图
生成对应副图，家庭风格、暖色
生成Amazon副图，参考竞品
完整8张，AS-02到AS-09都要
先出4张预览
```

默认情况下，泛化“副图”请求会输出推荐的 `5-7` 张副图。只有明确要求时才会完整生成 `AS-02` 到 `AS-09` 这 8 张。

### 3. 单张副图

```text
只生成AS-04尺寸图
重新生成AS-07对比图
生成一张场景图，家庭暖色
只要卖点图
生成单张防滑底座细节图
把AS-05改成户外场景
```

### 4. 详情页 / A+ 模块图

```text
生成详情图
生成详情页6张模块图
生成A+页面图
只生成AD-03卖点详情模块
生成AD-04产品结构详情模块
```

### 5. 参考图 / 竞品方向

```text
参考这张图的感觉重新做AS-04
给你一张竞品副图参考，不要照抄，按我们的产品改
参考这个排版，但卖点用我们的
照这个风格做一张功能图
看看Amazon前几名怎么做
参考这个Amazon链接优化副图
```

### 6. 风格控制

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
主色调参考产品或品牌
参考这套图的排版层次
```

默认情况下，如果是 Amazon US / 美国站，图片可见文案会优先用英文。  
如果你希望图上直接显示中文或双语，可以明确这样说：

```text
生成中文副图
图片文字用中文
尺寸图用中文标注
标题和标签都用中文
生成中英双语副图
```

## Slot 对照表

### Amazon 副图

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

### 详情页 / A+ 模块图

```text
AD-01 品牌/价值主视觉
AD-02 痛点/解决方案
AD-03 核心卖点详情模块
AD-04 产品结构/细节解释模块
AD-05 使用场景扩展
AD-06 尺寸/规格/适配模块
AD-07 安装/维护/FAQ
```

## 设计逻辑

这个 skill 主要遵循这些原则：

- 先理解产品，再写图。
- 先理解买家担心什么，再安排每张图表达什么。
- 每张图只讲一个核心观点，但不强制每张都配大标题。
- 先做版式决策：是否需要标题、标题/标注放哪里、字体大小和颜色层级怎么处理、是否需要背景承托。
- 标题、尺寸线、局部特写、人物动作、对比、图标或标注都可以承担卖点表达。
- 标题必须有画面证明；如果没有标题，产品、人物、细节或标注也必须把卖点讲清楚。
- 主色调要根据产品、品牌、场景、类目或参考图灵活选择，不固定某一套配色。
- 同一套副图要变化标签系统，避免连续多张都是黑字白底小标签。
- 详情页 / A+ 模块同样需要版式决策和模块节奏，不做一组普通空背景 banner。
- 合并相关卖点，例如 `claim + proof + mechanism`。
- 不重复出图，不为了凑数量硬做弱图。
- 参考图和竞品只做策略参考，不做照抄模板。
- 副图要适合缩略图阅读，信息明确、层次强。
- 详情页 / A+ 图可以更完整、更讲故事。

## Excel 输出

仓库内置脚本可以在需要时生成 Excel 汇总表：

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

## 开发与维护

修改后校验：

```bash
python3 ~/.codex/skills/.system/skill-creator/scripts/quick_validate.py .
```

提交并推送：

```bash
git status
git add .
git commit -m "Update amazon listing generator skill"
git push
```
