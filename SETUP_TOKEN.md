# GitHub Personal Access Token 配置指南

## 步骤 1: 创建 Personal Access Token

1. **访问 GitHub Settings**
   ```
   https://github.com/settings/tokens
   ```

2. **创建新的 Token**
   - 点击 "Generate new token" → "Generate new token (classic)"
   - Note: `PlayBook to Playbook-Door Communication`
   - Expiration: `No expiration` 或选择合适的时间（建议至少1年）
   - Scopes: 勾选以下权限：
     - ✅ `repo` (Full control of private repositories)
     - ✅ `workflow` (Update GitHub Action workflows)

3. **复制生成的 Token**
   - ⚠️ 重要：保存好这个 token，只会显示一次
   - Token 格式类似：`ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`

## 步骤 2: 在 PlayBook 仓库中添加 Secret

1. **进入 PlayBook 仓库设置**
   ```
   https://github.com/Space-tang/PlayBook/settings/secrets/actions
   ```

2. **添加新的 Secret**
   - 点击 "New repository secret"
   - Name: `PARENT_REPO_TOKEN`
   - Secret: 粘贴刚才创建的 Personal Access Token
   - 点击 "Add secret"

## 步骤 3: 验证配置

1. **检查 Secret 是否正确添加**
   - 在 PlayBook 仓库的 Settings → Secrets and variables → Actions 页面
   - 应该能看到 `PARENT_REPO_TOKEN` 密钥

2. **验证权限**
   - Token 应该有 `repo` 权限（访问 Playbook-Door 仓库）
   - Token 应该有 `workflow` 权限（触发工作流）

## 故障排除

### 如果 API 请求返回 401 Unauthorized
- 检查 token 是否正确复制到 Secret 中
- 确认 token 没有过期

### 如果 API 请求返回 403 Forbidden
- 检查 token 是否有 `repo` 权限
- 检查 token 是否有 `workflow` 权限
- 确认 token 的创建者有访问 Playbook-Door 仓库的权限

### 如果 Secret 不存在
- 确认在正确的仓库（PlayBook）中添加了 Secret
- 确认 Secret 名称为 `PARENT_REPO_TOKEN`（区分大小写）

## 安全注意事项

- ✅ Token 存储在 GitHub Secrets 中，不会在日志中显示
- ✅ 只给予必要的最小权限
- ⚠️ 定期轮换 token（建议每年更新一次）
- ⚠️ 如果 token 泄露，立即在 GitHub Settings 中撤销