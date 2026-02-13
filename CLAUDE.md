# Moodlet iOS App

## 项目概述

**Moodlet** - 一个简洁、温暖的心情记录 iOS 应用

- **核心功能**：每日心情打卡 + 备注记录 + 历史数据可视化
- **设计理念**：简约、温暖、有情感温度
- **技术栈**：SwiftUI + Swift

## 项目结构

```
moodlet-app-ios/
├── moodlet-app-ios/
│   ├── moodlet_app_iosApp.swift    # App 入口
│   ├── ContentView.swift            # 主视图
│   └── Assets.xcassets/             # 资源文件
├── moodlet-app-iosTests/            # 单元测试
└── moodlet-app-iosUITests/          # UI 测试
```

## 设计稿位置

设计稿和素材位于：`/Users/jiangshen/创意/Moodlet/`

- `文档/MOODLET-README.md` - 完整项目文档
- `页面/moodlet-login.html` - 登录页面设计
- `页面/moodlet-record.html` - 心情记录页面设计
- `页面/moodlet-calendar.html` - 历史日历页面设计
- `素材/moodlet-icons.zip` - 7个心情 SVG 图标

## 心情系统

### 7种心情（情绪光谱排列）

| 心情 | 英文 | 主色 | 背景色 | 情感 |
|------|------|------|--------|------|
| 欢喜 | Joyful | #FFC107 | #FFF9E6 | 阳光温暖 |
| 平静 | Calm | #4FC3F7 | #E8F6FB | 天空宁静 |
| 平淡 | Neutral | #D4A373 | #FFF8F0 | 岁月静好 |
| 疲惫 | Tired | #9575CD | #F3E5F5 | 紫色倦怠 |
| 悲伤 | Sad | #78909C | #ECEFF1 | 灰蓝忧郁 |
| 焦虑 | Anxious | #FF9800 | #FFF3E0 | 橙色不安 |
| 烦躁 | Frustrated | #EF5350 | #FFEBEE | 红色愤怒 |

**排列逻辑**：Joyful → Calm → Neutral → Tired → Sad → Anxious → Frustrated
- 默认停留在 Neutral（平淡）
- 向左滑是积极情绪，向右滑是消极情绪

## 主要页面

### 1. 登录页面
- Apple Sign In 登录
- 简洁的欢迎界面
- Tagline: "今天的你，还好吗？"

### 2. 心情记录页面（主页面）
- 左右滑动选择心情
- 50字符备注输入："今天想对自己说些什么？"
- "Capture" 按钮保存（非 Save）
- 确认弹窗 + 对勾动画
- 状态指示器显示今日已记录

### 3. 历史日历页面
- 月历视图展示历史记录
- 每天显示心情图标
- 悬停提示 + 点击查看详情
- 可切换月份（不能查看未来）

### 4. 数据统计页面（待开发）
- 图表展示心情趋势
- 柱状图/饼图/折线图

## 数据结构

```swift
struct MoodRecord {
    let date: Date
    let mood: MoodType  // joyful, calm, neutral, tired, sad, anxious, frustrated
    let note: String    // max 50 characters
    let timestamp: Date
}
```

## 关键设计决策

1. **名称选择**：Moodlet = Mood + let（小心情），俏皮可爱
2. **默认心情**：Neutral 在中间，不给用户心理暗示
3. **负面情绪更多**：2正面 + 1中性 + 4负面，更真实的情绪分布
4. **用词温度**："Capture" 比 "Save" 更有情感温度

## 待开发功能

- [ ] 数据统计页面（图表）
- [ ] 底部导航栏
- [ ] 数据持久化（UserDefaults / Core Data）
- [ ] 每日提醒
- [ ] 深色模式
- [ ] 数据导出

## 开发命令

```bash
# 构建项目
xcodebuild -project moodlet-app-ios.xcodeproj -scheme moodlet-app-ios build

# 运行测试
xcodebuild test -project moodlet-app-ios.xcodeproj -scheme moodlet-app-ios -destination 'platform=iOS Simulator,name=iPhone 16'
```

## 注意事项

- 使用 SwiftUI 开发，最低支持 iOS 17
- 遵循 Apple Human Interface Guidelines
- 保持简约设计风格，避免过度设计
- 配色严格按照设计稿中的颜色值
