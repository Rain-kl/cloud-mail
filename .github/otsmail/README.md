# OtsMail 部署与更新

现有 Cloud Mail 工作流用于旧 Worker。OtsMail 工作流从 `otscup/otsmail` 检出固定提交，安装锁文件中的依赖，运行后端测试并构建前端。发布任务使用构建产物，Cloudflare API Token 只在发布步骤可用。

默认基线记录在 `baseline.json`。更新只选稳定的 `web-v*` Release，并比较其提交中的数据库初始化、数据模型、迁移文件和 Durable Object 定义。涉及这些内容的更新要求先在数据库副本验证并准备对应的生产迁移，再更新记录的基线。部署前请核对生产 D1、KV 和 R2 绑定。发布只上传、激活 Worker 版本。

仓库变量 `OTSMAIL_AUTO_UPDATE=true` 开启每日检查。手动运行可以先选择验证，首次验证通过后可选择发布。仓库继续使用原有的 Cloudflare Actions 凭证，Worker 的 JWT 密钥由 Cloudflare 管理。
