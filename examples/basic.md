# 照查使用示例

## 示例 1：React 登录组件

### 输入产物

用户完成了一个登录表单组件：

```tsx
// LoginForm.tsx
export const LoginForm = () => {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');

  const handleSubmit = async () => {
    const res = await fetch('/api/login', {
      method: 'POST',
      body: JSON.stringify({ email, password })
    });
    if (res.ok) {
      window.location.href = '/dashboard';
    }
  };

  return (
    <form>
      <input value={email} onChange={e => setEmail(e.target.value)} />
      <input type="password" value={password} onChange={e => setPassword(e.target.value)} />
      <button onClick={handleSubmit}>登录</button>
    </form>
  );
};
```

### 照查报告

```markdown
## 照查报告

**产物**: src/components/LoginForm.tsx
**分析视角**: 安全 | 异常处理 | 用户体验 | 无障碍

---

### [安全视角]
- 🔴 HIGH: 密码输入框缺少 `autocomplete="current-password"` 属性
- 🔴 HIGH: 表单提交未做 CSRF 防护
- 🟡 MEDIUM: 建议在提交前对邮箱格式做前端校验

### [异常处理视角]
- 🔴 HIGH: 网络错误时无任何反馈，用户不知道发生了什么
- 🟡 MEDIUM: 缺少 loading 状态，用户可能重复点击
- 🟡 MEDIUM: 未处理 401/403 等具体错误码

### [用户体验视角]
- 🟡 MEDIUM: 密码强度无提示
- 🟢 LOW: 登录按钮缺少 loading 状态反馈
- 🟢 LOW: 建议添加"记住我"选项

### [无障碍视角]
- 🟡 MEDIUM: input 缺少 label 关联
- 🟡 MEDIUM: 缺少 aria-live 区域用于播报错误信息

---

### 优化建议汇总

| # | 视角 | 严重度 | 建议 |
|---|------|--------|------|
| 1 | 安全 | 🔴 HIGH | 添加 autocomplete 属性 |
| 2 | 安全 | 🔴 HIGH | 添加 CSRF token |
| 3 | 异常处理 | 🔴 HIGH | 添加网络错误提示 |
| 4 | 安全 | 🟡 MEDIUM | 邮箱前端校验 |
| 5 | 异常处理 | 🟡 MEDIUM | 添加 loading 状态 |
| 6 | 异常处理 | 🟡 MEDIUM | 处理具体错误码 |
| 7 | 无障碍 | 🟡 MEDIUM | input 关联 label |
| 8 | 无障碍 | 🟡 MEDIUM | 添加 aria-live 区域 |
| 9 | 用户体验 | 🟡 MEDIUM | 密码强度提示 |
| 10 | 用户体验 | 🟢 LOW | 添加"记住我" |

**执行**: `y` 全部 | `n` 跳过 | `1,3` 指定项
```

---

## 示例 2：数据库迁移脚本

### 输入产物

```sql
-- migration_add_user_table.sql
CREATE TABLE users (
  id INT PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(255)
);
```

### 照查报告

```markdown
## 照查报告

**产物**: migrations/001_add_user_table.sql
**分析视角**: 数据完整性 | 回滚安全 | 性能 | 向后兼容

---

### [数据完整性视角]
- 🔴 HIGH: email 字段缺少 UNIQUE 约束
- 🔴 HIGH: 缺少 created_at/updated_at 时间戳字段
- 🟡 MEDIUM: id 建议使用自增或 UUID，而非手动指定

### [回滚安全视角]
- 🔴 HIGH: 缺少对应的回滚脚本（DROP TABLE）
- 🟡 MEDIUM: 建议添加迁移版本号管理

### [性能视角]
- 🟡 MEDIUM: email 字段建议添加索引（用于登录查询）
- 🟢 LOW: 可考虑添加 name 字段的索引（用于搜索）

### [向后兼容视角]
- 🟡 MEDIUM: 未考虑已有数据的迁移方案
- 🟡 MEDIUM: 缺少字段默认值说明

---

### 优化建议汇总

| # | 视角 | 严重度 | 建议 |
|---|------|--------|------|
| 1 | 数据完整性 | 🔴 HIGH | email 添加 UNIQUE 约束 |
| 2 | 数据完整性 | 🔴 HIGH | 添加 created_at/updated_at |
| 3 | 回滚安全 | 🔴 HIGH | 添加回滚脚本 |
| 4 | 数据完整性 | 🟡 MEDIUM | id 使用自增/UUID |
| 5 | 性能 | 🟡 MEDIUM | email 添加索引 |
| 6 | 回滚安全 | 🟡 MEDIUM | 添加版本号管理 |
| 7 | 向后兼容 | 🟡 MEDIUM | 考虑数据迁移方案 |
| 8 | 向后兼容 | 🟡 MEDIUM | 添加默认值说明 |
| 9 | 性能 | 🟢 LOW | name 添加索引 |

**执行**: `y` 全部 | `n` 跳过 | `1,3` 指定项
```

---

## 示例 3：CLI 部署脚本

### 输入产物

```bash
#!/bin/bash
# deploy.sh

echo "开始部署..."
npm run build
scp -r dist/* server:/var/www/app/
echo "部署完成"
```

### 照查报告

```markdown
## 照查报告

**产物**: scripts/deploy.sh
**分析视角**: 错误处理 | 幂等性 | 跨平台 | 日志追踪

---

### [错误处理视角]
- 🔴 HIGH: 任意命令失败后脚本仍继续执行
- 🔴 HIGH: scp 失败无回滚机制，可能导致生产环境不一致
- 🟡 MEDIUM: 缺少具体的错误信息输出

### [幂等性视角]
- 🔴 HIGH: 重复执行可能导致文件覆盖冲突
- 🟡 MEDIUM: 未检查目标目录是否存在
- 🟡 MEDIUM: 缺少锁机制防止并发部署

### [跨平台视角]
- 🟡 MEDIUM: scp 参数在 macOS 和 Linux 可能有差异
- 🟢 LOW: 建议使用更通用的部署工具

### [日志追踪视角]
- 🟡 MEDIUM: 缺少时间戳
- 🟡 MEDIUM: 缺少详细日志级别控制
- 🟢 LOW: 建议输出部署耗时统计

---

### 优化建议汇总

| # | 视角 | 严重度 | 建议 |
|---|------|--------|------|
| 1 | 错误处理 | 🔴 HIGH | 添加 `set -e` 和错误检查 |
| 2 | 错误处理 | 🔴 HIGH | 添加回滚机制 |
| 3 | 幂等性 | 🔴 HIGH | 添加并发锁 |
| 4 | 错误处理 | 🟡 MEDIUM | 优化错误信息 |
| 5 | 幂等性 | 🟡 MEDIUM | 检查目标目录 |
| 6 | 跨平台 | 🟡 MEDIUM | 处理平台差异 |
| 7 | 日志追踪 | 🟡 MEDIUM | 添加时间戳 |
| 8 | 日志追踪 | 🟡 MEDIUM | 添加日志级别 |
| 9 | 日志追踪 | 🟢 LOW | 输出耗时统计 |

**执行**: `y` 全部 | `n` 跳过 | `1,3` 指定项
```

---

## 示例 4：无优化建议的情况

如果产物质量已经很好，照查会输出：

```markdown
## 照查报告

**产物**: src/utils/formatDate.ts
**分析视角**: 边界处理 | 性能 | 类型安全

经过分析，未发现明显优化点。代码质量良好。

✅ 照查完成
```
