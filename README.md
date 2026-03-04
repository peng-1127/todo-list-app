# 📝 Todo List

一个现代化的任务管理应用，由 OpenClaw Agents Team 开发。

## ✨ 功能特性

### v2.1 最新版本
- ✅ **任务优先级** - 高/中/低，颜色标识
- ✅ **任务分类** - 工作/生活/学习/购物
- ✅ **截止日期** - 日期时间选择器
- ✅ **过期提醒** - 红色闪烁动画
- ✅ **智能过滤** - 全部/待完成/已完成/已过期/今天
- ✅ **数据持久化** - localStorage 自动保存

## 🚀 快速开始

### 方式 1: 直接打开
```bash
# Windows
start index.html

# macOS
open index.html

# Linux
xdg-open index.html
```

### 方式 2: 本地服务器
```bash
# Python
python -m http.server 8000

# Node.js
npx serve .

# 访问 http://localhost:8000
```

## 📁 文件说明

| 文件 | 说明 | 版本 |
|------|------|------|
| `index.html` | 主应用 (优先级 + 分类) | v2.0 |
| `index-v2.1.html` | 完整版 (含截止日期) | v2.1 |
| `README.md` | 说明文档 | - |

## 🎨 使用技巧

1. **添加任务**: 输入文字，选择优先级和分类，点击添加
2. **设置截止日期**: 点击日期选择器，选择到期时间
3. **筛选任务**: 点击顶部过滤按钮
4. **完成任务**: 点击复选框
5. **删除任务**: 点击红色删除按钮

## 📊 统计信息

应用会自动显示：
- 总任务数
- 已完成任务数
- 已过期任务数

## 🔧 技术栈

- **前端**: HTML5 + CSS3 + Vanilla JavaScript
- **存储**: localStorage
- **构建**: 无需构建，开箱即用

## 📝 更新日志

### v2.1 (2026-03-04)
- 🎉 新增截止日期功能
- 🎉 新增过期提醒
- 🎉 新增智能过滤器
- 🎉 优化 UI 动画

### v2.0 (2026-03-04)
- 🎉 新增任务优先级
- 🎉 新增任务分类
- 🎉 优化响应式设计

## 🐛 问题反馈

请在 GitHub Issues 报告问题：
https://github.com/peng-1127/todo-list-app/issues

## 📄 许可证

MIT License

---

**Made with ❤️ by OpenClaw Agents Team**
