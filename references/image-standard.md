# 配图标准 v5.0

## 基础风格

- 背景：warm oatmeal（燕麦色气质），可见冷压水彩纸纹理，保持干净
- 主体：Simplified silhouette（简化轮廓），Low contrast（低对比度），灰蒙蒙的半透明质感。去细节、去厚重阴影、去立体高光
- 技法：Watery wash（水洗感），色块自然晕开。Generous negative space（大量留白）
- 色彩：Translucent（半透明）、Airy（轻盈）、High-key（高调/明亮），颜料中加入大量白色。低饱和莫兰迪自然色系。不使用具体色号，用颜色气质描述（dusty rose / faded blue / warm sand）
- 留白：At least 50% negative space
- 文字：苹果风格无衬线骨架，watercolor wet-on-wet，soft blended organic edges

## 基础构图：画中画框架（可选）

方形纸张居中，宽画幅背景。横排文字较长时使用，文字写于纸张内，裁1:1时取到完整纸张与文字。不作为主流选择，仅作为可选构图方式。

## 可选变例系统（3种）

用户决定使用哪种变例。Agent执行时不主动选择变例。

### 1. anthropic（色块环绕）
色块围绕主体分布，不遮挡。色块边缘互相渗透，交界处柔和过渡。自然有机形态，非几何。

### 2. 纯文字（基于范式图标准）
- 字体基底：soft faded charcoal black → 褪色weathered → 被周围色块环境染色
- 左半部分吸收 dusty rose/terracotta 暖调
- 右半部分吸收 faded blue/blue-gray 冷调
- 中段 warm-to-cool 自然过渡
- 不是单一纯色，是环境染色的结果
- 主体为文字内容

### 3. 提取主体（拆分为三个子变例）

用户提供参考图，提取主体。Agent按以下三种技法选择执行：

**hard edge**：主体边缘清晰锐利，与水彩背景形成明确边界。主体轮廓精确，不晕染到背景。

**soft bleed**：主体边缘柔和过渡，与水彩背景自然融合。主体轮廓与色块互相渗透。

**minimal**：主体极度简化，仅保留最基本轮廓。去细节、去纹理，接近几何剪影但保留主体辨识度。

**选择规则**：
- 精确还原（第二层 `1`）→ 推荐 hard edge
- 简洁线条（第二层 `2`）→ 推荐 soft bleed 或 minimal
- 用户指定 → 按用户选择执行

## 变例混合建议（可选）

1. **anthropic + 纯文字**：色块氛围 + 环境染色文字
2. **anthropic + 提取主体**：提取主体 + 色块点缀
3. **纯文字 + 提取主体**：提取主体配合环境染色文字
4. **三变例全混合**：提取主体 + 色块点缀 + 环境染色文字

## 纯文字变例完整提示词模板（锁定范式）

**Agent严格复用此模板，只替换[SUBJECT]内容，不改动句式结构。**

```
Hand-drawn watercolor, Morandi color palette, minimalist style. A perfectly square sheet of warm oatmeal paper with 1:1 proportions, softly torn edges, visible cold press watercolor paper texture, not white, centered within a wide 2.35:1 cinematic composition.

[SUBJECT]

Watercolor color bleeds of dusty rose and faded terracotta bleeding from the bottom-left corner inward, and faded blue and dusty blue-gray bleeding from the top-right corner inward, organic fluid shapes with water stain textures and grainy paper fiber details, bleeding into each other at the edges with soft warm-to-cool transitions where they meet. All watercolor elements strictly confined within the square paper edges, never overlapping any core silhouette but close enough to softly color-dye its edges from below and beside.

Surrounding the paper is clean warm oatmeal background with generous negative space, at least 50% of the frame, no color bleeds outside the square. Low saturation matte finish, misty muted gray quality. Editorial magazine cover aesthetic. Ratio 2.35:1 wide cinematic composition.
```

**[SUBJECT] 替换规则：**
- 文字内容主体示例：At the center of the square paper, three lines of text stacked vertically: top line "LINE1", middle a small abstract watercolor cross mark like a soft ink brush stroke between the words, bottom line "LINE2". All rendered in a soft Apple-style sans-serif skeleton font, watercolor wet-on-wet technique with soft blended organic edges. The letterforms are not a single solid color — they are environmental-dyed: the left half of each line absorbs warm dusty rose and faded terracotta tones, the right half absorbs cool faded blue and dusty blue-gray tones, with a natural warm-to-cool transition in the middle. The text starts as soft faded charcoal black, weathered and aged, then gradually takes on the surrounding color bleeds.
- Agent根据用户指定的具体文字内容替换LINE1/LINE2，保持三段式结构不变。

## 调色盘（颜色气质描述）

- 基调底色：warm sand / warm oatmeal
- 暖系主体：dusty rose / faded terracotta
- 冷系主体：faded blue / dusty blue-gray
- 深色点缀：muted sage / soft charcoal

---

## 新变例：配色融入（Color Fusion · v5.3）

**定位**：可叠加于画中画 / anthropic / 纯文字 / 提取主体 任意主变例之上的子变例，不覆盖任何主范式。

**核心特征**：色块不刻意制造随机性，而是从固定方向自然渗入，与主体融为一体——像主体原本就生长在这个配色里。

- 色块从左下角（暖色 dusty rose/terracotta）和右上角（冷色 faded blue/blue-gray）渗入
- 不强行随机、不刻意对称、不去设计化
- 主体边缘被环境 softly color-dye，整体色调统一内敛
- **与主范式的关系**：主范式锁定模板的色块描述本身就是"配色融入"。本次只是明确命名并确立为独立子变例选项。
- **触发条件**：用户表达"自然融入""原本就是这个配色""不刻意"，或主动选择"配色融入"时采用
- **可叠加变例**：画中画、提取主体、纯文字、anthropic 均可叠加

**配色融入效果描述（用于S7提示词）**：

色块不刻意制造随机性，而是从固定方向自然渗入（左下暖色 dusty rose/terracotta，右上冷色 faded blue/blue-gray），与主体融为一体——像主体原本就生长在这个配色里。色块边缘与主体自然过渡，不强行设计化，整体色调内敛统一。

---

## 新变例：色彩溢出（Color Overflow · v5.4）

**定位**：色块突破方形纸张边界，向外部画布渗透的子变例。可叠加于任意主变例。

**核心特征**：色块不再严格限制在方形纸张内，而是自然溢出到周围燕麦色背景中，形成更自由、更动态的构图。

- 色块从纸张边缘自然渗透出来，呈水彩晕染状
- 溢出程度轻微（≤10%），不过度扩散
- 纸张轮廓仍然可辨识，但被色块软化
- 整体保持克制，不因溢出而混乱

**与主范式的关系**：可叠加于画中画、提取主体、anthropic、纯文字。不与留白守护叠加（留白守护要求色块退至远处）。

**触发条件**：用户表达"溢出""突破""流动""不受约束"时采用。

**色彩溢出效果描述（用于S7提示词）**：

色块轻微突破方形纸张边缘，自然渗透到周围燕麦色背景中， watercolor wash 沿纸张边界自然晕开，整体构图更自由流动，但纸张轮廓依然清晰可辨。溢出程度克制，不超过纸张面积的10%。

---

## "混乱"定义硬规则

以下情况定义为构图混乱，禁止输出：

| 混乱类型 | 判定标准 | 处理 |
|---------|---------|------|
| 色块过载 | 超过3种独立色块争夺视觉焦点 | 删减至2种主色块 |
| 主体失焦 | 主体与背景对比度低于0.3 | 提高主体明度或降低背景饱和度 |
| 方向冲突 | 两个及以上视觉引导线指向相反方向 | 统一为单一视觉流向 |
| 溢出过度 | 色块溢出超过纸张面积15% | 限制溢出在10%以内 |

---

## 不美化个例约束

Agent在输出提示词时，严禁对具体人物、品牌、产品进行美化性描述。包括但不限于：

- ❌ "elegant"、"graceful"、"stunning" 等褒义形容词修饰具体主体
- ❌ 理想化身材比例、面部特征
- ❌ 添加主体本身不具备的光环、特效
- ✅ 允许：客观物理描述（"短发"、"戴圆框眼镜"、"双手交叉"）
- ✅ 允许：氛围描述（"低对比度"、"半透明质感"）不针对主体本身

---

## 品牌标识/Logo 处理规则

当主体为知名品牌标识/Logo时：

### 描述策略
- **只给品牌名称+品牌原色**，不做几何细节描述（如"三个六边形"、"Q字母"）
- 让模型自行调用训练数据中的品牌标识知识
- 过度文字描述反而降低准确度——模型对"Google logo"的认知远强于任何文字描述

### 示例
| 做法 | 示例 |
|------|------|
| ✅ 正确 | "the Taobao logo in its original orange brand color" |
| ❌ 错误 | "a rounded shield-like shape with the Chinese character inside" |
| ✅ 正确 | "the Tongyi Qianwen logo in its original blue-violet brand color" |
| ❌ 错误 | "three hexagonal polygons arranged in a triangular formation" |

### 技法推荐
- 还原度第二层选 `1`（精确还原）
- 技法推荐 **hard edge**，保持品牌标识轮廓清晰锐利
- 不添加任何美化性形容词（elegant/graceful/stunning）

---

## 比例切换只改比例

切换 2.35:1 ↔ 3:4 时，**只修改以下两处**：

1. `centered within a wide 2.35:1 cinematic composition` → `centered within a tall 3:4 portrait composition`
2. 末尾 `Ratio 2.35:1 wide cinematic composition` → `Ratio 3:4 tall portrait composition`

**严禁修改**：
- 主体位置、大小、描述
- 色块位置、形态、技法
- 纸张质感、边缘描述
- 任何其他构图元素

---

## 3:4 纸张自适应拉长

3:4 竖版模式下，画中画方形纸张 **不强制保持 1:1**，可自适应为 **纵向略长的矩形纸张**（比例约 3:4 或 4:5），以更好利用竖版空间。

规则：
- 横向宽度不变（与 2.35:1 版相同）
- 纵向高度适度拉长，占满更多竖版中央区域
- 纸张边缘、质感、技法不变
- 色块仍从对应角渗入，比例自适应

---

## 安全框设计

3:4 竖版模式下，主体必须位于 **中央安全框** 内：

- 安全框范围：画面中央 70% 区域（上下左右各留 15% 边距）
- 主体核心（面部/主要识别区）必须完整在安全框内
- 安全框外仅用于色块溢出、背景留白
- 目的：确保在抖音/小红书等平台自动裁切时不丢失主体

---

## 双元素互动

画面中出现两个主体时的处理规则：

| 关系 | 构图方式 | 示例 |
|------|---------|------|
| 对立 | 左右对称分布，中间留白 | 传统 vs 现代 |
| 呼应 | 对角线分布，色块连接 | 人与人、人与物 |
| 层级 | 一大一小，主前次后 | 产品与概念 |

规则：
- 两个主体共享同一色彩处理（第四层统一）
- 空间载体（第三层）统一
- 主体之间必须有视觉连接（色块、视线、姿态），禁止孤立放置

---

## hard edge 独立技法

hard edge 作为提取主体的子变例，独立技法规范：

**边缘处理**：
- 主体轮廓线清晰、锐利、无晕染
- 轮廓线宽度均匀，约 1-2px 视觉宽度
- 轮廓颜色：soft charcoal 或主体固有色的深色版本

**与背景的关系**：
- 主体与背景之间有明确边界，不互相渗透
- 背景色块（anthropic/配色融入）可接近但不越过主体边缘
- 留白守护下，色块与主体边缘保持 30% 缓冲区

**适用场景**：
- 精确还原（第二层 `1`）
- 品牌标识、产品图需要清晰轮廓
- 用户明确要求"清晰边缘""不要模糊"

---

## 新变例：留白守护（Negative Space Guard · v5.4）

**定位**：专为复杂轮廓主体（人物、动物、精细插画）设计的保护性色彩处理方式。可叠加于画中画或直接放置之上。

**核心规则**：

| 规则 | 说明 |
|------|------|
| **保护区机制** | 主体周围 ≥30% 负空间缓冲区，色块不进入 |
| **远距离晕染** | 色块仅在画布/纸张边缘或四角存在，作为氛围背景，不逼近主体 |
| **零接触原则** | 色块与主体轮廓之间至少保留一个呼吸距离，禁止 color-dye 主体边缘 |
| **主体完整优先** | 人物/动物轮廓完整清晰，不被色块切割、染色、或视觉挤压 |

**与空间载体的叠加关系**：

| 叠加组合 | 效果 |
|---------|------|
| 画中画 + 留白守护 | 方形纸张内主体居中，四角淡色晕染，主体周围干净留白 |
| 直接放置 + 留白守护 | 主体直接放画布中央，色块退至画布边缘，远距离氛围 |

**提示词关键句（插入主体描述后）**：

```
The subject stands with generous breathing space around it, 
at least 30% negative space buffer between the subject's outline and any color elements. 
Color bleeds remain at the distant edges of the composition, 
never approaching the subject's silhouette. 
The subject's form stays fully intact, clear, and undisturbed by surrounding color blocks.
```

---

## 留白守护完整提示词模板（直接放置版）

```
Hand-drawn watercolor, Morandi color palette, minimalist style. Wide 2.35:1 cinematic composition with clean warm oatmeal background.

At the center of the composition, [SUBJECT] in low contrast, semi-transparent grayish quality, misty muted tones, no heavy shadows, no 3D highlights. The subject stands with generous breathing space around it, airy and high-key. At least 30% negative space buffer between the subject's outline and any color elements — the subject's form stays fully intact, clear, and undisturbed.

Soft watercolor color bleeds of dusty rose and faded terracotta at the bottom-left distant corner, and faded blue and dusty blue-gray at the top-right distant corner, organic fluid shapes with water stain textures, remaining at the composition's edges only. Color elements never approach the subject's silhouette. No color-dye on the subject's edges. The color blocks serve as subtle ambient atmosphere, not as surrounding elements.

Surrounding the subject is clean warm oatmeal background with generous negative space, at least 50% of the frame. Low saturation matte finish, misty muted gray quality. Editorial magazine cover aesthetic. Ratio 2.35:1 wide cinematic composition.
```

## 留白守护完整提示词模板（画中画版）

```
Hand-drawn watercolor, Morandi color palette, minimalist style. A perfectly square sheet of warm oatmeal paper with 1:1 proportions, softly torn edges, visible cold press watercolor paper texture, not white, centered within a wide 2.35:1 cinematic composition.

At the center of the square paper, [SUBJECT] in low contrast, semi-transparent grayish quality, misty muted tones, no heavy shadows, no 3D highlights. The subject stands with generous breathing space around it, at least 30% negative space buffer between the subject's outline and any color elements — the subject's form stays fully intact, clear, and undisturbed.

Soft watercolor color bleeds of dusty rose and faded terracotta at the bottom-left distant corner of the square paper, and faded blue and dusty blue-gray at the top-right distant corner, organic fluid shapes with water stain textures, remaining at the paper's edges only. Color elements never approach the subject's silhouette. No color-dye on the subject's edges.

Surrounding the paper is clean warm oatmeal background with generous negative space, at least 50% of the frame, no color bleeds outside the square. Low saturation matte finish, misty muted gray quality. Editorial magazine cover aesthetic. Ratio 2.35:1 wide cinematic composition.
```

---

## S6 四层体系速查表

```
N-N-N-N
│ │ │ └─ 第四层：色彩处理
│ │ │     1=anthropic（逼近染色）
│ │ │     2=留白守护（远离保护）
│ │ │     3=配色融入（固定方向渗入）
│ │ └──── 第三层：空间载体
│ │       1=画中画（方形纸张1:1）
│ │       2=直接放置（画布中央无遮挡）
│ └────── 第二层：还原度
│         1=精确还原（公众人物、具体形态）
│         2=简洁线条（概念表达、匿名化）
└──────── 第一层：主体类型
          1=人物 2=物体/场景 3=动物 4=纯文字 5=通用
```

### 纯文字硬规则
- 第一层 `4` → 第三层固定 `1`（画中画）
- 第四层默认 `3`（配色融入）
- `4-x-1-3` = 标准纯文字

### 冲突矩阵速查

**🔴 红灯阻断**
- `1-1-x-1` 精确人物 + anthropic
- `3-1-x-1` 精确动物 + anthropic
- `4-x-1-2` 纯文字 + 留白守护
- `4-x-2-x` 纯文字 + 直接放置

**🟡 黄灯确认**
- `1-1-x-3` 精确人物 + 配色融入
- `4-x-1-1` 纯文字 + anthropic

**🟢 绿灯** 其余组合直接输出

---

## 尺寸切换规则

| 比例 | 类型 | 使用场景 |
|------|------|---------|
| **2.35:1** | 默认·宽画幅 | 公众号/图文平台头图、横版封面 |
| **3:4** | 可选·竖长图 | 抖音/小红书封面、竖版平台 |

切换方式：将提示词中的 `centered within a wide 2.35:1 cinematic composition` 和末尾 `Ratio 2.35:1 wide cinematic composition` 替换为 `tall 3:4 portrait` 对应描述。主体、色块、技法均不变。
