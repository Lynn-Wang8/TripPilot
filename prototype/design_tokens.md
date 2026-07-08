# TripPilot 原型开发规范（Design Tokens）

> 首页（P1）定稿后固化的设计语言，从 `trippilot.html` 实测提取。
> **后续 P2–P9 全部照此开发，不即兴改值**，保证 9 个页面风格统一。
> 基调：手机竖屏 · 浅暖底 + 深森林绿 · 亲和松弛 · 有旅行温度 · 干净省负担。

---

## 1. 字体

```
正文/UI：  "Noto Sans SC"（思源黑体），经 CDN 引入
衬线标题： "Noto Serif SC"（思源宋体），用 class="serif"
兜底栈：  -apple-system, BlinkMacSystemFont, "PingFang SC", "Microsoft YaHei"
```

引入（放 `<head>`）：
```html
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+SC:wght@300;400;500;700;900&family=Noto+Serif+SC:wght@500;700;900&display=swap" rel="stylesheet">
```

全局：`letter-spacing:.2px; -webkit-font-smoothing:antialiased; text-rendering:optimizeLegibility;`

### 字阶（固定，不出现第 5 种）

| 用途 | 字号 / 字重 |
|---|---|
| Hero 大标题（衬线） | 32px / 细 300 + 粗 800 对比 |
| 页面/分区标题 | 18px / 700 |
| 卡片标题 | 15.5px / 600 |
| 正文·对话气泡 | 15px / 400 |
| 辅助（时间、依据、AI提示） | 12.5–13px / 400 |
| 标签、tab 文字 | 10–12px / 500–600 |

> 字重拉开对比是精致感来源：Hero 用 300 细体 + 800 黑体并置。

---

## 2. 色值（CSS 变量）

```css
:root{
  --primary:#2E6B5E;        /* 主色 深森林绿：主按钮、选中、强调、AI */
  --primary-dark:#1F4D42;   /* 按下态 */
  --primary-light:#E8F0ED;  /* 选中背景、标签底、AI气泡底 */
  --lime:#C3D982;           /* 柔和黄绿点睛（收敛使用，勿铺开） */
  --lime-dark:#A8C063;
  --bg-page:#F6F3EC;        /* 页面底 暖白（非纯白，更松弛） */
  --bg-card:#FFFFFF;
  --text:#20261F;           /* 主文字 近黑带绿灰 */
  --text-sub:#6B7770;       /* 次要文字 */
  --text-dis:#A8B0AB;       /* 占位/禁用 */
  --border:#EAE6DF;         /* 分割线/描边 暖灰 */
  --sun:#E8A54B;            /* 暖阳点缀，极少量 */
  /* 场景 C 告警色（柔和，不刺眼，仅告警用） */
  --hi:#D96A5B;   /* 高 */
  --mid:#E8A54B;  /* 中 */
  --low:#5B93D9;  /* 低 */
}
```

补充暖底/薄荷色（局部）：AI 提示条底 `#EEF6F1`、AI 提示文字 `#4A6B60`。

> 用色纪律：主体大面积是暖白 + 白卡 + 森林绿点缀。lime 只做极少点睛，勿铺开（会显幼稚）。告警三色只在场景 C 用。

---

## 3. 圆角

| 元素 | 圆角 |
|---|---|
| 卡片 / 大图卡 | 18px |
| 图标容器（缩略图块） | 12px |
| 按钮 | 12–16px |
| 对话气泡 | 20px（纯圆角，**无尖角**） |
| 标签 / 胶囊 | 8–9px（小标签）/ 999px（胶囊） |
| 输入框 | 24px（胶囊形） |
| 手机外壳 / 内屏 | 52 / 42px |

---

## 4. 间距

| Token | 值 | 用途 |
|---|---|---|
| 页面左右边距 | **20px**（全局统一，禁止贴边） |
| 卡片之间 | 10–12px |
| 卡片内边距 | 14–15px |
| 分区标题 padding | 16px 上 / 8px 下 |
| 卡片内元素间隔 | 8–16px |
| 大区块之间 | 16–24px |

> 留白铁律：宁可空，不要挤。一屏聚焦一个核心动作。

---

## 5. 阴影

```
卡片柔和阴影：  0 4px 18px rgba(31,77,66,.07)   /* 带绿调，不发灰 */
大图卡：       0 6px 20px rgba(31,77,66,.1)
浮层/胶囊：     0 8px 24px rgba(0,0,0,.12)
```

---

## 6. 图标规范

- **全部单色线性 SVG**，`fill:none; stroke:currentColor; stroke-width:1.8; stroke-linecap:round; stroke-linejoin:round`
- **禁止用 emoji 当图标**（塑料感、跳、不统一）——正文里的情绪 emoji 也慎用
- AI 头像符号：**太阳**（圆 + 8 道光芒），代表"陪你探索的旅行伙伴"
- 尺寸：正文图标 20px，小图标 14–16px，tab 图标 23px

---

## 7. 核心组件结构

### iPhone 外壳 + 灵动岛 + 状态栏
```
.phone(390×844,圆角52,padding12) > .screen(圆角42)
  .island（灵动岛，绝对定位顶部居中，110×32黑胶囊）
  .statusbar（绝对定位顶部，54px高，z-index150，pointer-events:none）
    .light=白字（深色hero上） / .dark=黑字（浅底页上）
    左：.time（9:41）  右：.sb-r（信号/wifi/电池 线性SVG，18×13）
  .page.on > .scroll（内容滚动区）
  内页需在内容顶部放 .safe-top（54px）给状态栏让位
```

### 按钮
```
.btn（高50，圆角16，字15/600）
  .btn-p  深绿底白字（主）
  .btn-s  白底绿字绿描边（次）
```

### 对话气泡（P2 起用）
```
.ava  32圆·主色底·白色太阳SVG（仅AI有，用户无头像）
.bub.ai  白底+柔阴影·圆角20·左对齐
.bub.u   深绿底白字·圆角20·右对齐
.typing  三点跳动打字动画
```

### 引导式选项卡（demo 推进用）
```
.opt  白底·1.5px描边·圆角16·点击变主色浅底+绿描边
每步给 1 个选项作"剧本台词"，点击推进，保证问答一一对应
底部保留输入框+语音按钮作示意（不接后端）
```

### 信息卡 / 行程卡
```
.trip-wrap  统一外壳（白底·圆角18·柔阴影·padding14-15·左右margin20）
  .trip-main  主行：缩略图44×44 + 标题/副文 + 状态标签
  .trip-ai    AI陪伴条：薄荷底#EEF6F1·圆角11·太阳图标 + 文案
同层级卡片必须同规格，只靠内容多少和标签区分
```

### 数据来源标签（对比/方案页用，体现"不编数字"）
```
小胶囊·11px·主色浅底绿字（如"机票API""攻略RAG""意外发现"）
```

### 底部 Tab
```
.tabbar  白底·顶部1px描边·padding 10/30（底部留手势安全区）
.tab.on  主色 + 图标填充；其余 --text-dis
```

---

## 8. 动效

```css
@keyframes fadeUp{from{opacity:0;transform:translateY(16px);}to{opacity:1;transform:translateY(0);}}
.rise{opacity:0;animation:fadeUp .6s cubic-bezier(.16,1,.3,1) forwards;}
.d1~.d6{animation-delay:.05s递增到.54s;}   /* 入场依次淡入上浮 */
@media(prefers-reduced-motion:reduce){.rise{animation:none;opacity:1;}}
```

- 页面加载：元素从上到下依次淡入（stagger）
- 卡片点击：`:active{transform:scale(.99)}`
- 克制原则：只用入场编排 + 轻点击反馈，**不用呼吸动效等花哨效果**

---

## 9. 图片

- 真实照片走 Unsplash CDN，`onerror` 隐藏 → 露出底层 SVG/渐变兜底，**绝不开天窗**
- 图片上有文字时压深色渐变遮罩：`linear-gradient(to top,rgba(0,0,0,.55),transparent)`
- 上线 Netlify 联网正常；离线看有渐变兜底

---

## 10. Demo 手法（无后端）

- 引导式：用户点固定选项推进，走设计好的 happy path
- 数据固定，选项即"剧本台词"，保证问答对应，不给凭空期待
- 保留输入框/语音按钮作示意，让体验者知道真实产品可自由对话
- P1→P9 一条龙可点通，展示"决策→执行→行中兜底"完整体验

---

## 页面清单与进度

| 页 | 名称 | 档位 | 状态 |
|---|---|---|---|
| P1 | 首页 | 完整 | ✅ 定稿 |
| P2 | 陪聊需求（S1+S2） | 🌟高光 | 已成型，待复核 |
| P3 | 偏好排序 | 完整 | 待做 |
| P4 | 多维对比报告 | 🌟高光 | 待做（下一个） |
| P5 | 行程确认 | 完整 | 待做 |
| P6 | 行程仪表盘 | 完整 | 待做 |
| P7 | 应急预警 | 完整 | 待做 |
| P8 | Plan B 方案 | 🌟高光 | 待做 |
| P9 | 执行引导 | 完整 | 待做 |
