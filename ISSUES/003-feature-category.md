---
title: "[Feature] P0 - 任务分类功能"
labels: ["feature", "p0", "v2.0"]
assignee: "peng-1127"
status: open
---

# 📁 功能需求：任务分类

**提出者**: @01-pm  
**分配给**: @05-fullstack  
**优先级**: 🔴 P0  
**预计时间**: 2 小时

---

## 📋 需求描述

支持为任务添加分类标签（如工作、生活、学习、购物等），便于任务管理和筛选。

---

## 🎨 UI 设计

### 分类选择器

```
┌─────────────────────────────────┐
│ 新任务                          │
├─────────────────────────────────┤
│ 任务标题：[________________]    │
│                                 │
│ 分类：[💼 工作 ▼]               │
│                                 │
│        [取消]  [添加]           │
└─────────────────────────────────┘
```

### 分类管理

```
┌─────────────────────────────────┐
│ 📁 分类管理                     │
├─────────────────────────────────┤
│ 💼 工作          [编辑] [删除]  │
│ 🏠 生活          [编辑] [删除]  │
│ 📚 学习          [编辑] [删除]  │
│ 🛒 购物          [编辑] [删除]  │
│                                 │
│ [+ 添加新分类]                  │
└─────────────────────────────────┘
```

### 预设分类

| 分类 | 图标 | 颜色 |
|------|------|------|
| 工作 | 💼 | #3b82f6 (蓝色) |
| 生活 | 🏠 | #10b981 (绿色) |
| 学习 | 📚 | #8b5cf6 (紫色) |
| 购物 | 🛒 | #f59e0b (橙色) |
| 健康 | 💪 | #ef4444 (红色) |
| 其他 | 📌 | #6b7280 (灰色) |

---

## 💻 技术实现

### 数据结构

```javascript
// 分类定义
{
  id: "work",
  name: "工作",
  icon: "💼",
  color: "#3b82f6",
  createdAt: "2026-03-04T08:00:00Z"
}

// 任务对象
{
  id: "uuid",
  title: "任务标题",
  category: "work", // 分类 ID
  priority: "medium",
  completed: false,
  createdAt: "2026-03-04T08:00:00Z"
}
```

### 分类管理器

```javascript
class CategoryManager {
  constructor() {
    this.categories = this.loadCategories() || this.getDefaultCategories();
  }
  
  getDefaultCategories() {
    return [
      { id: 'work', name: '工作', icon: '💼', color: '#3b82f6' },
      { id: 'personal', name: '生活', icon: '🏠', color: '#10b981' },
      { id: 'study', name: '学习', icon: '📚', color: '#8b5cf6' },
      { id: 'shopping', name: '购物', icon: '🛒', color: '#f59e0b' },
      { id: 'health', name: '健康', icon: '💪', color: '#ef4444' },
      { id: 'other', name: '其他', icon: '📌', color: '#6b7280' }
    ];
  }
  
  addCategory(name, icon, color) {
    const category = {
      id: this.generateId(),
      name,
      icon,
      color,
      createdAt: new Date().toISOString()
    };
    this.categories.push(category);
    this.save();
    return category;
  }
  
  getCategoryById(id) {
    return this.categories.find(c => c.id === id);
  }
  
  getTasksByCategory(categoryId) {
    return app.tasks.filter(t => t.category === categoryId);
  }
}
```

### UI 渲染

```javascript
function renderCategoryPicker(selectedId) {
  return `
    <div class="category-picker">
      ${categoryManager.categories.map(cat => `
        <button 
          class="category-option ${cat.id === selectedId ? 'selected' : ''}"
          data-category="${cat.id}"
        >
          <span class="category-icon">${cat.icon}</span>
          <span class="category-name">${cat.name}</span>
        </button>
      `).join('')}
    </div>
  `;
}

function renderTaskCategory(categoryId) {
  const category = categoryManager.getCategoryById(categoryId);
  if (!category) return '';
  
  return `
    <span class="task-category" style="background-color: ${category.color}20; color: ${category.color}">
      ${category.icon} ${category.name}
    </span>
  `;
}
```

---

## ✅ 验收标准

- [ ] 创建任务时可以选择分类
- [ ] 任务列表显示分类标签
- [ ] 可以自定义分类（添加/编辑/删除）
- [ ] 支持按分类筛选任务
- [ ] 预设 6 个常用分类
- [ ] 分类数据持久化

---

## 🧪 测试用例

### TC-001: 创建分类任务
1. 打开应用
2. 输入任务标题
3. 选择"工作"分类
4. 点击添加

**预期**: 任务添加到列表，显示"💼 工作"标签

### TC-002: 添加新分类
1. 打开分类管理
2. 点击"添加新分类"
3. 输入名称"娱乐"，选择图标🎮
4. 保存

**预期**: 新分类出现在分类列表中

### TC-003: 按分类筛选
1. 创建多个不同分类的任务
2. 点击"工作"分类筛选
3. 查看列表

**预期**: 只显示工作分类的任务

---

## 📝 开发记录

### 进度
- [ ] 设计完成
- [ ] 数据结构修改
- [ ] 分类管理 UI
- [ ] 选择器实现
- [ ] 筛选功能
- [ ] 测试通过
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
