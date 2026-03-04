# 🐛 GitHub Issues 使用指南

**你的所有 Bug 和功能请求都在 GitHub Issues 上管理！**

---

## 📋 已创建的 Issues

| # | 标题 | 类型 | 状态 | 链接 |
|---|------|------|------|------|
| 1 | 📋 [模板] Bug 报告模板 | 模板 | 🟢 Open | [查看](https://github.com/peng-1127/todo-list-app/issues/1) |
| 2 | 🎯 [P0] 任务优先级功能 | 功能 | ✅ Done | [查看](https://github.com/peng-1127/todo-list-app/issues/2) |
| 3 | 📁 [P0] 任务分类功能 | 功能 | ✅ Done | [查看](https://github.com/peng-1127/todo-list-app/issues/3) |
| 4 | 🚀 [史诗] v2.0 升级计划 | 史诗 | 🟡 In Progress | [查看](https://github.com/peng-1127/todo-list-app/issues/4) |

---

## 🔍 如何提交 Bug

### 方式 1: 使用模板 (推荐)

1. 访问 https://github.com/peng-1127/todo-list-app/issues/new
2. 复制 [Issue #1](https://github.com/peng-1127/todo-list-app/issues/1) 的模板内容
3. 填写 Bug 信息
4. 提交

### 方式 2: 使用 GitHub CLI

```bash
gh issue create --title "Bug: [简短描述]" --body "
## 🐛 问题描述
[详细描述]

## 复现步骤
1. ...
2. ...

## 预期行为
[应该发生什么]

## 实际行为
[实际发生了什么]
"
```

---

## 📝 Issue 模板

### Bug 报告

```markdown
## 🐛 Bug 标题

**发现者**: @你的用户名  
**发现时间**: 2026-03-04  
**严重程度**: 🔴 严重 / 🟡 中等 / 🟢 轻微

### 问题现象
[详细描述]

### 复现步骤
1. 打开应用
2. 点击 XXX
3. 看到错误

### 预期行为
[应该怎样]

### 实际行为
[实际怎样]

### 截图
[上传截图]

### 环境
- 浏览器：Chrome 122
- 系统：Windows 11
```

### 功能请求

```markdown
## 🎯 功能名称

**提出者**: @你的用户名  
**优先级**: 🔴 P0 / 🟡 P1 / 🟢 P2

### 需求描述
[详细描述功能]

### 使用场景
[在什么情况下使用]

### 预期效果
[实现后是什么样子]

### 验收标准
- [ ] 标准 1
- [ ] 标准 2
```

---

## 🏷️ Issue 标签说明

| 标签 | 颜色 | 用途 |
|------|------|------|
| `bug` | 🔴 #d73a4a | Bug 报告 |
| `enhancement` | 🔵 #a2eeef | 功能增强 |
| `p0` | 🟠 #d93f0b | 最高优先级 |
| `v2.0` | 🔵 #0075ca | v2.0 版本 |

---

## 📊 Issue 工作流

```
新建 Issue
    ↓
分配标签和负责人
    ↓
开发中 (状态更新)
    ↓
完成开发 → 添加评论
    ↓
测试验证
    ↓
关闭 Issue
```

---

## 🔔 通知设置

### 关注 Issue
- 点击 Issue 右侧的 **🔔 Bell** 图标
- 选择通知级别：
  - Participating, @mentions and custom
  - Participating and @mentions
  - Ignore

### @提及某人
```markdown
@peng-1127 请查看这个 Bug
```

---

## 📱 访问方式

### Web
https://github.com/peng-1127/todo-list-app/issues

### GitHub CLI
```bash
# 查看所有 Issues
gh issue list

# 查看特定 Issue
gh issue view 2

# 添加评论
gh issue comment 2 --body "评论内容"

# 关闭 Issue
gh issue close 2

# 重新打开
gh issue reopen 2
```

---

## 🎯 当前待办

### 需要测试
- [ ] Issue #2: 任务优先级功能 - 等待测试验证

### 开发中
- [ ] Issue #3: 任务分类功能 - ✅ 已完成

### 待开发
- [ ] Issue #?: 截止日期功能
- [ ] Issue #?: 搜索过滤
- [ ] Issue #?: 深色主题

---

## 💡 最佳实践

1. **一个 Issue 一件事** - 不要在一个 Issue 里混合多个 Bug 或功能
2. **清晰的标题** - 一眼看出问题是什么
3. **详细描述** - 帮助开发者快速理解
4. **及时更新** - 状态变化时更新评论
5. **关联 Commit** - 使用 `#Issue 号` 关联代码提交

---

## 📈 统计

| 状态 | 数量 |
|------|------|
| Open | 4 |
| Closed | 0 |
| 进行中 | 1 |
| 待测试 | 1 |

---

**最后更新**: 2026-03-04  
**维护者**: @开发团队
