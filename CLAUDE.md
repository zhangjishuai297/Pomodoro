# CLAUDE.md

本文件为 Claude Code (claude.ai/code) 在此仓库中工作时提供指引。

## 项目概览

一个完全可自定义的单文件 HTML 番茄钟。无需构建工具、无任何依赖——直接用浏览器打开 `pomodoro.html` 即可使用。

## 架构

所有代码集中在 `pomodoro.html`（HTML + CSS + JS）。分为三层：

- **状态机**（`state` 变量）：`focus` → `shortBreak` → `focus` … → `longBreak` → `focus`（下一大轮）。切换逻辑位于 `handleSessionComplete()`。当前大轮/番茄号由 `currentRound` / `currentPom` 追踪。
- **计时引擎**：基于 `Date.now()` 计算 `endTime`，即使标签页在后台也能保持准确。`setInterval` 每 200ms 刷新一次显示。核心函数：`startTimer()`、`pauseTimer()`、`resetTimer()`、`skipSession()`。
- **持久化**：设置和每日统计（今日完成番茄数、总专注秒数）存储在 `localStorage` 中。每日统计数据在日期变更时自动归零。

## 关键行为

- **通知权限**：仅在页面加载时请求一次（`init` 块），不会每次点击"开始"都弹窗。
- **音频**：使用 Web Audio API 振荡器（无需音频文件）。3 音符上行提示音，每 2 秒重复直到用户操作。
- **每日统计**：按日期键值存储（`pomodoro_daily_YYYY-MM-DD`）。通过每 60 秒的 `setInterval` 检测跨日。
- **设置键**：`pomodoro_settings`。默认值：专注 25 分钟、短休息 5 分钟、长休息 15 分钟、每轮 4 个番茄、每日 5 轮。

## 常用操作

- **启动**：`open pomodoro.html`
- **检查**：无配置的 lint 工具；可提取 JS 后用 `node --check` 验证语法，或直接在浏览器中打开查看。
- **修改后**：刷新浏览器标签页即可——无需编译构建。
