# 实施计划

## 概述
实现 PlayBook 仓库（子模块）推送代码后自动通知 Playbook-Door 仓库（父仓库）进行子模块同步的功能。

## 任务列表

- [x] 1. 创建 PlayBook 仓库的通知工作流
  - 在 PlayBook 仓库中创建 `.github/workflows/notify-parent.yml` 文件
  - 配置工作流在 push 到 main 分支时触发
  - 实现向 Playbook-Door 发送 repository dispatch 事件的逻辑
  - _需求: 1.1, 1.2, 4.1, 4.2_

- [x] 2. 配置 GitHub Personal Access Token
  - 创建具有 `repo` 和 `workflow` 权限的 Personal Access Token
  - 在 PlayBook 仓库中添加 `PARENT_REPO_TOKEN` 密钥
  - 验证 token 权限设置正确
  - _需求: 6.1, 6.2, 6.3_

- [x] 3. 实现 API 调用逻辑
  - 编写 curl 命令调用 GitHub Repository Dispatch API
  - 配置正确的 API 端点和请求头
  - 实现 client_payload 数据结构，包含仓库信息和时间戳
  - _需求: 4.1, 4.2, 4.3, 4.4_

- [x] 4. 添加提交信息传递功能
  - 获取最新提交的 commit message 和 SHA
  - 将提交信息包含在 dispatch 事件的 client_payload 中
  - 确保特殊字符在 JSON 中正确转义
  - _需求: 7.1, 7.2, 7.3, 7.4_

- [x] 5. 实现错误处理和日志记录
  - 添加 API 请求成功/失败的状态检查
  - 实现清晰的中文日志消息输出
  - 添加认证失败和网络错误的错误处理
  - _需求: 5.1, 5.2, 5.3, 5.4_

- [x] 6. 配置路径过滤（可选优化）
  - 设置工作流只在特定文件变化时触发
  - 配置监控 `**/__meta__.txt` 文件变化
  - 配置监控新目录创建
  - _需求: 2.1, 2.2, 2.3, 2.4_

- [x] 7. 创建测试和验证流程
  - 编写测试脚本验证 API 调用格式
  - 创建测试提交验证端到端流程
  - 验证 Playbook-Door 能正确接收 dispatch 事件
  - 确认子模块同步和工作流触发正常工作
  - _需求: 1.3, 1.4, 3.1, 3.2, 3.3, 3.4_

- [x] 8. 添加手动触发支持
  - 配置 workflow_dispatch 触发器
  - 添加手动触发的说明文档
  - 测试手动触发功能正常工作
  - _需求: 1.1_

- [x] 9. 创建故障排除文档
  - 编写常见问题的排查步骤
  - 创建 token 权限检查清单
  - 添加工作流调试指南
  - 提供 API 请求失败的解决方案
  - _需求: 5.2, 6.4_