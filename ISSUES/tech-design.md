---
title: "[设计] v2.0 技术方案设计"
labels: ["design", "technical", "v2.0"]
assignee: "peng-1127"
status: open
---

# 🏗️ Todo-List v2.0 技术方案

**提出者**: @02-tech-lead (架构师)  
**分配给**: @00-coordinator (指挥官)  
**优先级**: 🔴 高

---

## 📋 需求总结

根据 Issue #1 的讨论，v2.0 需要实现以下核心功能：

### P0 - 必须实现
1. 任务优先级 (高/中/低)
2. 任务分类/标签
3. 截止日期
4. 搜索和过滤

### P1 - 重要功能
1. 多主题切换
2. 数据导出/导入
3. 布局切换

### P2 - 锦上添花
1. 统计图表
2. 拖拽排序
3. 成就系统

---

## 🛠️ 技术选型

### 前端框架

**决策**: 保持原生 JavaScript (不引入框架)

**理由**:
- 项目规模适中，原生 JS 足够
- 保持零依赖优势
- 避免构建复杂度
- 便于用户直接使用

### 数据结构设计

```javascript
// 任务对象
{
  id: "uuid",
  title: "任务标题",
  description: "任务描述",
  completed: false,
  priority: "high", // high | medium | low
  category: "work", // 分类 ID
  tags: ["urgent", "important"], // 标签数组
  dueDate: "2026-03-10T10:00:00Z", // 截止日期
  createdAt: "2026-03-04T08:00:00Z",
  updatedAt: "2026-03-04T08:00:00Z",
  completedAt: null,
  subtasks: [], // 子任务
  attachments: [] // 附件
}

// 分类对象
{
  id: "work",
  name: "工作",
  color: "#3b82f6",
  icon: "💼"
}

// 用户设置
{
  theme: "light", // light | dark | purple | blue | green
  layout: "list", // list | card | board
  fontSize: "medium", // small | medium | large
  showCompleted: true,
  sortBy: "createdAt", // createdAt | dueDate | priority
  sortOrder: "desc" // asc | desc
}
```

### 存储方案

**决策**: localStorage + IndexedDB 混合

```javascript
// 简单数据用 localStorage
- 用户设置
- 分类定义
- 最近访问

// 大量数据用 IndexedDB
- 任务数据
- 附件
- 历史记录
```

### 模块结构

```
todo-app/
├── index.html          # 主页面
├── css/
│   ├── base.css       # 基础样式
│   ├── themes.css     # 主题样式
│   └── layouts.css    # 布局样式
├── js/
│   ├── app.js         # 主应用入口
│   ├── store.js       # 数据存储 (IndexedDB)
│   ├── models.js      # 数据模型
│   ├── views/
│   │   ├── ListView.js
│   │   ├── CardView.js
│   │   └── BoardView.js
│   ├── components/
│   │   ├── TaskItem.js
│   │   ├── TaskForm.js
│   │   ├── FilterBar.js
│   │   └── CategoryPicker.js
│   └── utils/
│       ├── date.js
│       ├── storage.js
│       └── helpers.js
└── assets/
    ├── icons/
    └── images/
```

---

## 🎨 UI/UX 设计

### 主题系统

```css
:root {
  /* 浅色主题 */
  --bg-primary: #ffffff;
  --bg-secondary: #f8f9fa;
  --text-primary: #1a1a1a;
  --text-secondary: #666666;
  --accent: #667eea;
}

[data-theme="dark"] {
  /* 深色主题 */
  --bg-primary: #1a1a1a;
  --bg-secondary: #2d2d2d;
  --text-primary: #ffffff;
  --text-secondary: #a0a0a0;
  --accent: #818cf8;
}

[data-theme="purple"] {
  /* 紫色主题 */
  --bg-primary: #1e1b4b;
  --bg-secondary: #312e81;
  --text-primary: #e0e7ff;
  --text-secondary: #a5b4fc;
  --accent: #8b5cf6;
}
```

### 优先级颜色

```css
.priority-high { border-left: 4px solid #ef4444; }
.priority-medium { border-left: 4px solid #f59e0b; }
.priority-low { border-left: 4px solid #10b981; }
```

### 布局切换

1. **列表视图**: 传统列表，适合快速浏览
2. **卡片视图**: 卡片式，显示更多信息
3. **看板视图**: Kanban 板，按状态分组

---

## 🔧 核心功能实现

### 1. 任务优先级

```javascript
class TaskManager {
  setPriority(taskId, priority) {
    const task = this.getTask(taskId);
    task.priority = priority;
    task.updatedAt = new Date().toISOString();
    this.saveTask(task);
    this.render();
  }
  
  getTasksByPriority(priority) {
    return this.tasks.filter(t => t.priority === priority);
  }
}
```

### 2. 分类系统

```javascript
class CategoryManager {
  createCategory(name, color, icon) {
    const category = {
      id: this.generateId(),
      name,
      color,
      icon,
      createdAt: new Date().toISOString()
    };
    this.categories.push(category);
    this.save();
    return category;
  }
  
  getTasksByCategory(categoryId) {
    return this.tasks.filter(t => t.category === categoryId);
  }
}
```

### 3. 截止日期和提醒

```javascript
class DueDateManager {
  setDueDate(taskId, dueDate) {
    const task = this.getTask(taskId);
    task.dueDate = dueDate;
    this.saveTask(task);
    
    // 设置提醒
    this.scheduleReminder(task);
  }
  
  scheduleReminder(task) {
    const now = new Date();
    const due = new Date(task.dueDate);
    const diff = due - now;
    
    if (diff > 0 && diff < 24 * 60 * 60 * 1000) {
      // 24 小时内到期，设置提醒
      setTimeout(() => {
        this.showNotification(task);
      }, diff);
    }
  }
  
  showNotification(task) {
    if (Notification.permission === 'granted') {
      new Notification('任务即将到期', {
        body: task.title,
        icon: '/icon.png'
      });
    }
  }
}
```

### 4. 搜索和过滤

```javascript
class TaskFilter {
  constructor(tasks) {
    this.tasks = tasks;
    this.filters = {
      search: '',
      status: 'all', // all | active | completed
      priority: 'all', // all | high | medium | low
      category: 'all',
      sortBy: 'createdAt',
      sortOrder: 'desc'
    };
  }
  
  applyFilters() {
    return this.tasks.filter(task => {
      // 搜索
      if (this.filters.search) {
        const query = this.filters.search.toLowerCase();
        if (!task.title.toLowerCase().includes(query) &&
            !task.description?.toLowerCase().includes(query)) {
          return false;
        }
      }
      
      // 状态
      if (this.filters.status === 'active' && task.completed) {
        return false;
      }
      if (this.filters.status === 'completed' && !task.completed) {
        return false;
      }
      
      // 优先级
      if (this.filters.priority !== 'all' && 
          task.priority !== this.filters.priority) {
        return false;
      }
      
      // 分类
      if (this.filters.category !== 'all' && 
          task.category !== this.filters.category) {
        return false;
      }
      
      return true;
    }).sort((a, b) => {
      // 排序
      const aVal = a[this.filters.sortBy];
      const bVal = b[this.filters.sortBy];
      const order = this.filters.sortOrder === 'asc' ? 1 : -1;
      return aVal > bVal ? order : -order;
    });
  }
}
```

---

## 📦 数据导出/导入

```javascript
class DataManager {
  exportData(format = 'json') {
    const data = {
      version: '2.0',
      exportedAt: new Date().toISOString(),
      tasks: this.tasks,
      categories: this.categories,
      settings: this.settings
    };
    
    if (format === 'json') {
      const blob = new Blob([JSON.stringify(data, null, 2)], 
                           { type: 'application/json' });
      this.downloadFile(blob, 'todo-backup.json');
    } else if (format === 'csv') {
      const csv = this.tasksToCSV(this.tasks);
      const blob = new Blob([csv], { type: 'text/csv' });
      this.downloadFile(blob, 'todo-tasks.csv');
    }
  }
  
  importData(file) {
    return new Promise((resolve, reject) => {
      const reader = new FileReader();
      reader.onload = (e) => {
        try {
          const data = JSON.parse(e.target.result);
          this.validateData(data);
          this.tasks = data.tasks || [];
          this.categories = data.categories || [];
          this.settings = data.settings || {};
          this.saveAll();
          resolve({ success: true, count: this.tasks.length });
        } catch (err) {
          reject(new Error('Invalid backup file'));
        }
      };
      reader.readAsText(file);
    });
  }
}
```

---

## 🧪 测试策略

### 单元测试
- 数据模型验证
- 过滤器逻辑
- 日期计算

### 集成测试
- CRUD 操作
- 数据持久化
- 导入/导出

### E2E 测试
- 完整用户流程
- 多浏览器兼容
- 响应式布局

---

## 📅 开发计划

| 阶段 | 任务 | 预计时间 | 负责人 |
|------|------|----------|--------|
| Week 1 | 数据结构 + 存储层 | 2 天 | @05-fullstack |
| Week 1 | 任务优先级 + 分类 | 2 天 | @05-fullstack |
| Week 1 | 截止日期 + 提醒 | 1 天 | @05-fullstack |
| Week 2 | 搜索过滤 | 1 天 | @03-frontend |
| Week 2 | 主题系统 | 2 天 | @03-frontend |
| Week 2 | 布局切换 | 2 天 | @03-frontend |
| Week 3 | 数据导出/导入 | 1 天 | @05-fullstack |
| Week 3 | 统计图表 | 2 天 | @03-frontend |
| Week 3 | 测试 + 修复 | 2 天 | @09-qa |

---

## ⚠️ 风险和挑战

1. **IndexedDB 兼容性** - 需要 polyfill 支持旧浏览器
2. **通知权限** - 用户可能拒绝通知权限
3. **数据迁移** - 从 v1.0 localStorage 迁移到 v2.0
4. **性能** - 大量任务时的渲染性能

**缓解措施**:
- 使用虚拟滚动优化长列表
- 提供数据迁移工具
- 渐进式增强，核心功能不依赖新特性

---

## 🎯 成功标准

- [ ] 所有 P0 功能完成并测试通过
- [ ] 性能：1000+ 任务流畅操作
- [ ] 兼容性：Chrome/Firefox/Safari/Edge 最新 2 个版本
- [ ] 代码质量：ESLint 通过，测试覆盖率 > 80%
- [ ] 用户文档完整

---

**@00-coordinator** 请确认技术方案，@05-fullstack 准备开始开发！

---

**创建时间**: 2026-03-04  
**最后更新**: 2026-03-04  
**状态**: 🟡 待评审
