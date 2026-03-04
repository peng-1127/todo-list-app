---
title: "[Feature] P0 - 任务优先级功能"
labels: ["feature", "p0", "v2.0"]
assignee: "peng-1127"
status: open
---

# 🎯 功能需求：任务优先级

**提出者**: @01-pm  
**分配给**: @05-fullstack  
**优先级**: 🔴 P0  
**预计时间**: 2 小时

---

## 📋 需求描述

为任务添加优先级设置，支持高/中/低三个等级，并用颜色区分。

---

## 🎨 UI 设计

### 优先级选择器

```
┌─────────────────────────────────┐
│ 新任务                          │
├─────────────────────────────────┤
│ 任务标题：[________________]    │
│                                 │
│ 优先级：⚪ 高  ⚪ 中  ⚪ 低       │
│                                 │
│        [取消]  [添加]           │
└─────────────────────────────────┘
```

### 任务列表显示

```
┌─────────────────────────────────┐
│ 🔴 高优先级任务 1               │ ← 红色左边框
│ 🟡 中优先级任务 2               │ ← 橙色左边框
│ 🟢 低优先级任务 3               │ ← 绿色左边框
└─────────────────────────────────┘
```

### 颜色规范

| 优先级 | 颜色 | Hex | 用途 |
|--------|------|-----|------|
| 高 | 🔴 红色 | `#ef4444` | 紧急且重要 |
| 中 | 🟡 橙色 | `#f59e0b` | 重要不紧急 |
| 低 | 🟢 绿色 | `#10b981` | 日常任务 |

---

## 💻 技术实现

### 数据结构

```javascript
{
  id: "uuid",
  title: "任务标题",
  priority: "medium", // high | medium | low
  completed: false,
  createdAt: "2026-03-04T08:00:00Z"
}
```

### CSS 样式

```css
.task-item {
  padding: 15px;
  border-left: 4px solid #e0e0e0;
}

.task-item.priority-high {
  border-left-color: #ef4444;
  background: linear-gradient(to right, #fef2f2, transparent);
}

.task-item.priority-medium {
  border-left-color: #f59e0b;
  background: linear-gradient(to right, #fffbeb, transparent);
}

.task-item.priority-low {
  border-left-color: #10b981;
  background: linear-gradient(to right, #ecfdf5, transparent);
}
```

### JavaScript 实现

```javascript
class TaskManager {
  // 添加任务
  addTask(title, priority = 'medium') {
    const task = {
      id: this.generateId(),
      title: title,
      priority: priority,
      completed: false,
      createdAt: new Date().toISOString()
    };
    this.tasks.push(task);
    this.save();
    this.render();
    return task;
  }
  
  // 修改优先级
  setPriority(taskId, priority) {
    const task = this.getTask(taskId);
    task.priority = priority;
    task.updatedAt = new Date().toISOString();
    this.save();
    this.render();
  }
  
  // 获取按优先级排序的任务
  getTasksByPriority() {
    const priorityOrder = { high: 0, medium: 1, low: 2 };
    return this.tasks.sort((a, b) => 
      priorityOrder[a.priority] - priorityOrder[b.priority]
    );
  }
}
```

---

## ✅ 验收标准

- [ ] 创建任务时可以选择优先级
- [ ] 任务列表显示优先级颜色
- [ ] 可以修改任务的优先级
- [ ] 支持按优先级排序
- [ ] 默认优先级为"中"
- [ ] 移动端显示正常

---

## 🧪 测试用例

### TC-001: 创建高优先级任务
1. 打开应用
2. 输入任务标题
3. 选择"高"优先级
4. 点击添加

**预期**: 任务添加到列表，显示红色左边框

### TC-002: 修改优先级
1. 找到任务
2. 点击优先级图标
3. 从"中"改为"高"

**预期**: 任务边框颜色从橙色变为红色

### TC-003: 按优先级排序
1. 创建 3 个不同优先级的任务
2. 选择"按优先级排序"

**预期**: 任务顺序为 高→中→低

---

## 📝 开发记录

### 进度
- [ ] 设计完成
- [ ] 数据结构修改
- [ ] UI 实现
- [ ] 功能实现
- [ ] 测试通过
- [ ] 代码审查
- [ ] 合并发布

### 开发者
@05-fullstack

### 预计完成
2026-03-04

---

## 📎 关联

- 关联 Issue: #000 (升级计划)
- 依赖 Issue: 无
- 阻塞 Issue: 无

---

**创建时间**: 2026-03-04  
**最后更新**: 2026-03-04  
**状态**: 🟡 待开发
