📢 TG频道：https://t.me/embyfdgltzq 💬 TG交流群：https://t.me/embyfdgljlq

github暂停更新，最新workers请前往TG频道或TG交流群获取。
## 更新日志
- 最新变更见：[`CHANGELOG.md`](./CHANGELOG.md)
- 最新版本见右侧 `Releases`

# Emby Worker Proxy（Cloudflare Workers + D1）

基于 Cloudflare Workers 的 Emby 反向代理与后台管理项目。  
支持 `/admin` 节点管理、第三方播放器兼容、D1 持久化存储、节点级专线头注入（可选）。

---

## 功能特性

- 管理后台：`/admin`
- 节点增删改查（D1 存储）
- 全反代模式（单模式）
- 第三方播放器兼容
- 一键导入/复制代理地址
- 节点缓存与列表缓存优化
- 可选专线头注入（EMOS 相关）

---

## 快速部署

### 1）创建 D1 数据库
在 Cloudflare 控制台创建 D1 数据库（例如：`emby-proxy`）。

### 2）初始化数据表
在 D1 控制台执行 SQL：

CREATE TABLE IF NOT EXISTS proxy_kv (
  k TEXT PRIMARY KEY,
  v TEXT NOT NULL,
  updated_at INTEGER NOT NULL
);

CREATE INDEX IF NOT EXISTS idx_proxy_kv_k ON proxy_kv(k);

### 3）部署 Worker
将仓库内 `worker.js` 部署到 Cloudflare Workers。

### 4）绑定 D1
Worker → 设置 → 绑定 → 添加 D1：

- 绑定名称：`EMBY_D1`
- 数据库：你创建的 D1

### 5）配置变量
Worker → 设置 → 变量与机密：

必填：
- `ADMIN_TOKEN`

### 6）配置域名
将你的业务域名接入 Cloudflare，并把 Worker 路由到对应域名路径（如 `example.com/*`）。

### 7）访问后台
打开：
https://你的域名/admin

使用 `ADMIN_TOKEN` 登录后添加节点。

---

## 节点填写建议

- 请求路径：英文唯一（如 `emby`）
- 显示名称：可中文（如 `公益服`）
- 目标地址：源 Emby 地址
- 代理地址：`https://你的域名/节点路径`
- （新增）当前为自动全反代模式，无需再选择“统一/分离”。（部分服务器暂不支持反代或workers不支持反代该服务器）


---

## 免责声明

本项目仅用于学习与技术测试。  
请遵守当地法律法规及服务条款，使用风险由用户自行承担。

---

## License

MIT
