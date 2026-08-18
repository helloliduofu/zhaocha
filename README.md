# 🔍 找茬 (zhaocha)

[![skills.sh](https://skills.sh/b/anthropics/skills)](https://skills.sh/anthropics/skills)

> 🪞 像照镜子一样，从各个角度审视你的工作成果

一个 Claude Code Skill，在代码/方案完成后，AI 自动从多个动态视角分析优化空间，减少人工反复迭代的工作量。

## 💡 核心理念

**不是固定的检查清单，而是动态的思考视角**

不同的任务需要不同的审视角度：
- 🔐 登录表单 → 安全视角 + 异常处理视角 + 用户心智视角
- 🗄️ 数据库设计 → 一致性视角 + 查询性能视角 + 迁移兼容视角
- ⌨️ CLI 工具 → 命令行规范视角 + 错误提示视角 + 管道组合视角

AI 会根据具体任务，动态推断需要哪些思考角度。

## ⚙️ 工作流程

```
完成任务 → AI 分析任务本质 → 动态生成思考视角 → 多视角并行分析 → 输出优化报告 → 用户确认 → 执行优化
```

## 🚀 使用方式

### 🤖 自动触发（默认）

当你完成以下工作时，找茬会自动执行：
- ✍️ 写完代码后
- 📋 完成方案设计后
- ✅ 实现功能后

AI 会自动分析产物，输出优化建议报告。

### 🎯 手动触发

```bash
# 手动触发找茬
/zhaocha

# 或简写
/zc
```

## 📊 报告示例

```markdown
## 🔍 找茬报告

**产物**: src/components/LoginForm.tsx
**分析视角**: 🔐 安全 | 🛡️ 异常处理 | 👤 用户体验 | ♿ 无障碍

---

### 🔐 [安全视角]
- 🔴 HIGH: 密码输入框缺少 autocomplete="off" 属性
- 🟡 MEDIUM: 建议在提交前对邮箱格式做前端校验

### 🛡️ [异常处理视角]
- 🟡 MEDIUM: 网络错误时缺少重试机制
- 🟢 LOW: 建议区分"用户不存在"和"密码错误"的提示

### 👤 [用户体验视角]
- 🟡 MEDIUM: 密码强度提示不够直观
- 🟢 LOW: 登录按钮在请求中缺少 loading 状态

---

### 📋 优化建议汇总

| # | 视角 | 严重度 | 建议 |
|---|------|--------|------|
| 1 | 🔐 安全 | 🔴 HIGH | 添加 autocomplete="off" |
| 2 | 🛡️ 异常处理 | 🟡 MEDIUM | 添加网络错误重试 |
| 3 | 👤 用户体验 | 🟡 MEDIUM | 优化密码强度提示 |
| 4 | ♿ 无障碍 | 🟡 MEDIUM | 添加 aria-label |
| 5 | 🛡️ 异常处理 | 🟢 LOW | 区分错误提示 |
| 6 | 👤 用户体验 | 🟢 LOW | 添加 loading 状态 |

**执行**: `y` 全部 | `n` 跳过 | `1,3` 指定项
```

## ⚙️ 配置

可在项目中创建 `.zhaocharc.json` 自定义配置：

```json
{
  "autoTrigger": true,
  "minSeverity": "low",
  "excludePatterns": ["*.test.ts", "*.spec.ts"],
  "customContext": {
    "framework": "react",
    "style": "functional"
  }
}
```

### 📝 配置项说明

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `autoTrigger` | boolean | `true` | 🤖 是否自动触发 |
| `minSeverity` | string | `"low"` | 🎚️ 最小显示严重度：`high` / `medium` / `low` |
| `excludePatterns` | string[] | `[]` | 🚫 排除的文件模式 |
| `customContext` | object | `{}` | 🎯 自定义上下文，帮助 AI 更好理解项目 |

## 🚫 不触发场景

以下情况不会自动触发：
- 📖 只读操作（仅查看文件）
- 🔧 简单配置修改
- 🙅 用户明确说"不需要检查"

## 🧠 工作原理

1. 🔍 **任务分析**：理解输入产物的本质和领域
2. 💭 **视角生成**：根据任务特征，动态推断需要的思考角度
3. ⚡ **并行分析**：每个视角独立分析，找出优化点
4. 📊 **汇总报告**：统一格式输出，按严重度排序
5. ✅ **执行优化**：用户确认后，执行选中的优化项

## 📦 安装方式

```bash
npx skills add https://github.com/helloliduofu/zhaocha
```

## 📄 License

MIT
