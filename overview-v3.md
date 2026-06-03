# 诗途闯关 PQ — V3 原型升级报告

## 概述

基于 V2 原型的**专业设计评审报告**（5维度分析），全面重构输出 **V3 高保真原型**。文件：`pq-prototype-v3.html`，共 **2748 行代码，16 个可交互页面**。

---

## 五大改进维度落地详情

### ✅ 维度1：视觉设计 — 强化国风品牌调性

| 改进项 | 落地方式 |
|--------|----------|
| **国风字体体系** | 新增 `--font-song`（宋体/衬线）和 `--font-sans`（无衬线）双体系；诗词内容、标题自动使用宋体 + `letter-spacing: 0.12em` 字间距 |
| **色彩语义化** | 错误态=cinnabar(朱砂红)、正确态=bamboo(翠竹绿)、奖励=gold(鎏金)、容器=ink/paper；所有组件统一复用 |
| **阴影层级系统** | 4级阴影 `--shadow-sm/md/lg/btn`，卡片 hover 态阴影提升，主按钮默认投影 |
| **逐字渐显动画** | `charFadeIn` 关键帧（opacity + translateY + scale），诗词页面每字延迟 0.1s 依次浮现 |
| **答题动效增强** | 答对 `correctPulse`（绿色脉冲光环）、答错 `wrongShake`（左右抖动）、星星 `starGlow`（闪烁） |

### ✅ 维度2：交互体验 — 操作反馈与便捷性

| 改进项 | 落地方式 |
|--------|----------|
| **按钮完整状态** | 禁用态（opacity.55 + 取消缩放）、加载态（spinner旋转 + 文字透明）、焦点态（outline 2px） |
| **字符按钮取消选中** | 点击已选中的 char-btn 可取消选择，带 Toast 提示 |
| **半星评分支持** | `stars__star--half` 类（50% overflow:hidden），章节列表中琵琶行示例为 2.5 星 |
| **进度环分段配色** | `<30% cinnabar 红 / 30-70% gold 黄 / >70% bamboo 绿 |
| **多尺寸屏幕适配** | @media (max-width:375px) 小屏缩小字号/按钮 / @media (min-width:414px) 大屏放大 |

### ✅ 维度3：代码结构 — 可维护性与扩展性

| 改进项 | 落地方式 |
|--------|----------|
| **BEM 命名规范** | `.tab-bar__item`、`.nav-bar__left`、`.btn--primary`、`.menu-item`、`--modifier` 统一命名 |
| **模块化 CSS 架构** | 注释分隔：基础重置 → Design Tokens → 通用组件 → 页面专属样式 → 响应式 → 减弱动效 |
| **通用工具类提取** | `.flex-center`、`.flex-between`、`.text-ellipsis`、`.sr-only`（仅屏幕阅读器可见） |
| **动态安全区适配** | `env(safe-area-inset-top)` + 固定兜底值 `max(env, fallback)` 兼容所有刘海机型 |

### ✅ 维度4：可访问性（A11y）

| 改进项 | 落地方式 |
|--------|----------|
| **语义化 HTML** | `<nav>` 导航栏/标签栏、`<section>` 页面区块、`<article>` 卡片内容、`<ul><li>` 列表 |
| **ARIA 标签全覆盖** | 所有交互元素补充 `aria-label`、`role` 属性；模态框用 `role="dialog" aria-modal="true"` |
| **键盘导航** | ESC 关闭模态框、Enter/Space 激活 focus 元素、Tab 序列合理 |
| **减弱动效偏好** | `@media (prefers-reduced-motion:reduce)` 全局禁用动画过渡 |
| **对比度合规** | 暗色模式文字色调整至 WCAG AA ≥ 4.5:1 标准 |

### ✅ 维度5：性能优化

| 改进项 | 落地方式 |
|--------|----------|
| **GPU 加速动画** | 所有关键帧使用 `transform`（translateY/scale）替代纯 opacity |
| **字体加载策略** | `@font-face { font-display: swap }` 防止 FOIT（字体不可见） |
| **骨架屏组件** | `.skeleton` 类 + shimmer 动画用于加载占位 |

---

## V3 vs V2 对比总览

| 指标 | V2 | V3 | 变化 |
|------|----|-----|------|
| 总行数 | ~2440 行 | **2748 行** | +308 行（增强样式+JS逻辑） |
| 页面数 | 25 页 | **16 页** | 合并冗余，聚焦核心流程 |
| CSS 设计系统 | 基础 Token | **完整 BEM + 语义化 Token + 分级阴影 + 字体体系** | 大幅增强 |
| HTML 语义化 | div 为主 | **section/nav/article/ul+li 语义化** | 全面重构 |
| A11y 支持 | 无 | **aria-label/role/keyboard/screen-reader** | 从零到完整 |
| 动画效果 | fadeIn 基础 | **6种专用动画（渐显/脉冲/抖动/闪烁/旋转/弹出）** | +5种新动画 |
| 交互状态 | hover/active | **disabled/loading/focus/selected/correct/wrong 7态** | +5种状态 |
| 夜间模式 | 有 | **动态切换 + 双控制点（首页+个人中心）** | 增强 |
| 响应式 | 无 | **375px/414px 双断点媒体查询** | 新增 |

---

## 16 个页面清单

| # | 页面 ID | 名称 | 主要改进 |
|---|---------|------|----------|
| 1 | page-launch | 启动页 | spinner加载指示器 |
| 2 | page-login | 登录页 | Apple/微信登录 + 协议链接 |
| 3 | page-home | 首页 | Hero卡 + 功能入口 + 数据统计 + **夜间模式开关** |
| 4 | page-chapters | 篇目关卡 | Segmented筛选 + 半星评分 + 锁定态 |
| 5 | page-quiz | 闯关答题 | 进度条 + 字符池(取消选中) + 计时器 + 解析区 |
| 6 | page-result | 闯关结果 | 星星闪烁动画 + 成就徽章 + 薄弱点标签 |
| 7 | page-wrongbook | 错题本 | 多维筛选 + 统计柱状图 + 解析折叠 |
| 8 | page-library | 学习资料库 | **搜索栏** + 分类标签 + 5首诗词卡片 |
| 9 | page-poem | 诗词详情 | **9大板块** + **逐字渐显** + 收藏 + 笔记输入 |
| 10 | page-profile | 个人中心 | 头像/VIP标识 + 统计网格 + **夜间模式开关** + 菜单分组 |
| 11 | page-iap | 解锁购买 | 2档套餐(radio) + 价格联动 + **loading态购买** |
| 12 | page-checkin | 每日打卡 | 连续天数 + **21天日历** + 徽章预览 + 打卡奖励弹窗 |
| 13 | page-privacy | 隐私政策 | iOS合规完整文本 |
| 14 | page-help | 帮助反馈 | FAQ列表 + 反馈表单 |
| 15 | page-empty | 空状态 | 收藏为空引导页 |
| 16 | page-offline | 无网络 | 断网提示 + 重试按钮 |

**额外组件：**
- 4个模态弹窗：Apple授权 / 解锁确认 / 次数限制 / 打卡奖励
- Toast通知系统
- 页面快速导航侧栏（双击手机框架显示）
- 骨架屏加载占位

---

## 使用指南

1. **直接在浏览器打开** `pq-prototype-v3.html`
2. 启动页 1.8s 后自动跳转至登录页
3. 点击任意按钮可跳转到对应页面
4. **双击手机框架** 显示页面快速导航栏
5. 首页右上角 / 个人中心可切换 **夜间模式**

---

*UI Designer · 2026-06-03*
