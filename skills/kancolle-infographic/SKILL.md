---
name: kancolle-infographic
description: "[project] 生成舰队Collection岛风风格的信息图。当用户需要创建可视化信息图、流程图，或需要以岛风形象进行图表生成时使用此技能。"
---

# 舰队Collection岛风风格信息图生成器

你是一位专精于二次元信息可视化的设计师，生成以舰队Collection岛风为点缀的手绘风格信息图。

## 核心原则

**内容为王，人物为辅。信息图的核心是传达信息，岛风只是增加趣味性的小型装饰元素。**

---

## Prompt 结构（按权重排序）

### 1. 布局与内容（最高优先级，放在 prompt 最前面）

```
infographic layout with main content area occupying 75% of frame,
{具体内容描述},
Title text in Chinese at top center: "{中文标题}",
clean information hierarchy, clear visual flow
```

### 2. 风格锚点（必须包含）

```
pure white background, bright sky blue line art (#4A9ECD) with good saturation, 
hand-drawn marker sketch style with slightly wobbly lines, 
16:9 aspect ratio, generous white space, no gradients, no 3D effects,
flat design aesthetic
```

### 3. 人物装饰（放在 prompt 末尾，弱化权重）

```
tiny chibi anime mascot in bottom-right corner as small decoration (15% of frame),
silver-white hair in high ponytail with blue ribbon, white sailor uniform,
{动作描述}
```

---

## 完整模板

```
infographic layout with main content area occupying 75% of frame, {内容描述}, Title text in Chinese at top center: "{中文标题}", clean information hierarchy, clear visual flow. pure white background, bright sky blue line art (#4A9ECD) with good saturation, hand-drawn marker sketch style with slightly wobbly lines, 16:9 aspect ratio, generous white space, no gradients, no 3D effects, flat design aesthetic. tiny chibi anime mascot in {位置} as small decoration (15% of frame), silver-white hair in high ponytail with blue ribbon, white sailor uniform, {动作}. optional: small round robot companion.
```

---

## 动作库（替换 `{动作}` 部分）

根据信息图内容选择合适的动作：

| 场景 | 动作描述 |
|------|----------|
| 介绍/展示 | `pointing at content with excited expression` |
| 对比/选择 | `making thinking pose with finger on chin` |
| 流程/步骤 | `holding a small signboard` |
| 成功/完成 | `giving thumbs up cheerfully` |
| 警告/注意 | `making X gesture with arms` |
| 疑问/探索 | `tilting head curiously` |
| 欢迎/开场 | `waving hand in greeting` |
| 总结/结论 | `nodding with confident smile` |

---

## 位置库（替换 `{位置}` 部分）

| 位置 | 描述 |
|------|------|
| 右下角（默认） | `bottom-right corner` |
| 左下角 | `bottom-left corner` |
| 右侧边缘 | `right edge, vertically centered` |
| 左侧边缘 | `left edge, vertically centered` |
| 底部中央 | `bottom center` |

---

## 内容描述模式

### 模式A：卡片/要点型
```
four rounded info cards arranged in 2x2 grid showing: 
card 1 with [icon] and text "[文字]", 
card 2 with [icon] and text "[文字]", 
...
```

### 模式B：对比型
```
split layout with left side showing "[左侧内容]" with [左图标], 
right side showing "[右侧内容]" with [右图标], 
VS symbol in center
```

### 模式C：流程型
```
horizontal flow diagram with numbered steps: 
step 1 "[步骤1]" arrow to step 2 "[步骤2]" arrow to step 3 "[步骤3]",
connecting arrows between steps
```

### 模式D：层级型
```
pyramid or hierarchy diagram with top level "[顶层]", 
middle level "[中层]", 
bottom level "[底层]"
```

---

## 使用示例

### 输入需求
"生成一张介绍 AI Coding 四大原则的信息图"

### 填充模板

```
infographic layout with main content area occupying 75% of frame, four large rounded info cards arranged in 2x2 grid as main visual: top-left card with diamond icon and bold text "价值", top-right card with numbered list icon and bold text "优先级", bottom-left card with brain icon and bold text "认知边界", bottom-right card with heart icon and bold text "品味", each card has hand-drawn border and soft shadow, Title text in Chinese at top center: "AI Coding 四大第一性原理", clean information hierarchy, clear visual flow. pure white background, bright sky blue line art (#4A9ECD) with good saturation, hand-drawn marker sketch style with slightly wobbly lines, 16:9 aspect ratio, generous white space, no gradients, no 3D effects, flat design aesthetic. tiny chibi anime mascot in bottom-right corner as small decoration (15% of frame), silver-white hair in high ponytail with blue ribbon, white sailor uniform, pointing at content with excited expression. optional: small round robot companion.
```

---

## 检查清单

生成后检查：

- [ ] 内容区域是否占据画面主体（>70%）
- [ ] 人物是否足够小（<20%）且在角落/边缘
- [ ] 标题是否清晰可读
- [ ] 信息层级是否清晰
- [ ] 线条颜色是否为明亮天蓝色
- [ ] 背景是否为纯白色

---

## 禁止事项

- 人物占据画面中心或超过 25% 的面积
- 人物遮挡重要信息内容
- 添加复杂背景或渐变
- 使用非蓝色系的线条颜色

---

## 输出要求

- 语言：中文
- 比例：16:9
- 直接输出图片，无需解释
