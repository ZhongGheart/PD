# 诗途闯关（Poetry Quest / PQ）高保真原型交付

## 概述

为「诗途闯关 PQ」iOS App 完成了**全链路高保真交互原型**，涵盖产品需求文档中定义的全部 **11 个页面**，采用 iPhone 15 Pro 真实设备尺寸（393×852px），完全遵循 iOS 原生设计规范（Human Interface Guidelines）。

---

## 设计系统核心决策

| 维度 | 决策 | 理由 |
|------|------|------|
| **主色调** | 墨韵蓝 `#1B2A4A` | 国风沉稳感，适配高中生审美，区别于竞品常见配色 |
| **强调色** | 朱砂红 `#C44536` | 错误/警告/行动召唤，视觉冲击力强 |
| **成功色** | 翠竹绿 `#2D6A4F` | 正确/已解锁/成功状态，清新积极 |
| **VIP色** | 鎏金 `#B8860B` | 高级感/推荐标签/连续打卡，变现引导 |
| **背景色** | 宣纸暖白 `#FAF8F5` / `#F2EDE6` | 护眼、国风质感、区别于纯白 |
| **字体** | PingFang SC (系统默认) + SF Pro | iOS 原生字体栈，零加载成本 |
| **圆角体系** | 8/10/12/16/20px 五级 | iOS 标准圆角，从小组件到大卡片递进 |

## 已完成的页面清单

| # | 页面名称 | 页面ID | 核心特性 | 对接状态 |
|---|---------|--------|----------|---------|
| 1 | 启动页 | `page-launch` | 全屏居中 Logo + Slogan + Loading 动画，2秒自动跳转 | ✅ 完成 |
| 2 | 登录页 | `page-login` | Apple 一键登录 + 微信授权登录 + 协议链接 | ✅ 完成 |
| 3 | 首页/主Tab页 | `page-home` | Hero 卡片 + 功能网格 + 数据统计 + 底部 TabBar（首页/闯关/我的） | ✅ 完成 |
| 4 | 篇目关卡列表 | `page-chapters` | SegmentedControl 筛选 + 关卡卡片(排名/星级/进度/锁定) + FAB悬浮解锁按钮 | ✅ 完成 |
| 5 | 答题页 | `page-quiz` | 进度条+计时器+难度标签 + 4类题型UI(填字/字词/意境/考点) + 字符备选池 + 提交+解析展开 | ✅ 完成 |
| 6 | 闯关结果页 | `page-result` | 成功/失败双态 + 星级评分 + 数据统计(正确率/用时/对错数) + 薄弱点标签 + 成就徽章 + 三大操作按钮 | ✅ 完成 |
| 7 | 错题本 | `page-wrongbook` | 多维筛选 + 统计条 + 错题卡片(来源/题型/正误对比) + 解析展开 + 再答一次 | ✅ 完成 |
| 8 | 学习资料库 | `page-library` | 筛选栏 + 篇目列表卡片(封面/篇名/作者/朝代) | ✅ 完成 |
| 9 | 单篇诗文详情 | `page-poem` | 渐变头部 + 8大板块(原文/注释/实词虚词/翻译/手法/情感/考点/拓展) + 底部操作栏 | ✅ 完成 |
| 10 | 个人中心 | `page-profile` | 头像信息 + 4项数据统计 + 3组菜单列表(学习/VIP/设置) + 退出登录 | ✅ 完成 |
| 11 | 内购购买页 | `page-iap` | 产品介绍 + 权益标签 + 双套餐对比(单册9.9元/全册19.9元推荐) + Apple IAP 购买 + 恢复购买 | ✅ 完成 |
| 12 | 打卡页面 | `page-checkin` | 连续打卡天数 + 本周目标 + 日历视图(已打卡/今日/未打卡) + 打卡按钮 | ✅ 完成 |

## 交互功能

- **页面导航**：所有页面间跳转逻辑完整，无死胡同
- **Tab 切换**：首页 TabBar 在首页/闯关/我的三个页面间切换，高亮跟随
- **SegmentedControl**：筛选栏可点击切换
- **答题交互**：字符选择→填空→提交→显示解析
- **套餐选择**：IAP 页套餐点击选中高亮，购买按钮价格联动更新
- **Toast 提示**：全局 Toast 反馈用户操作结果
- **启动自动跳转**：启动页 2 秒后自动进入登录页
- **快速导航**：双击手机框架区域显示侧边页面快速导航面板

## 开发对接说明

### Flutter/Cupertino 对接要点

1. **组件映射关系**：
   - `.card` → `CupertinoListSection` / 自带 border 的 `Container`
   - `.btn-primary` → `CupertinoButton.filled`
   - `.segmented` → `CupertinoSlidingSegmentedControl`
   - `.tab-bar` → 自定义 `CupertinoTabBar`（3个Tab）
   - `.nav-bar` → `CupertinoNavigationBar`

2. **颜色 Token 直接使用**：
   ```dart
   static const Color ink = Color(0xFF1B2A4A);
   static const Color cinnabar = Color(0xFFC44536);
   static const Color bamboo = Color(0xFF2D6A4F);
   static const Color gold = Color(0xFFB8860B);
   static const Color paper = Color(0xFFFAF8F5);
   ```

3. **间距系统**：严格遵循 4/8/12/16/20/24/32/48px 八级间距

4. **图标策略**：当前使用 emoji/SVG 占位符，开发时替换为 SF Symbols 图标集

## 文件结构

```
artifacts/
└── pq-prototype.html    # 完整高保真原型（单文件，含全部11页面+交互逻辑）
```

---

**设计者**: UI Designer  
**日期**: 2026-06-03  
**状态**: ✅ 已完成，可直接用于开发评审和 Flutter 对接
