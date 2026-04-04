# OutlookManager

OutlookManager 是由 Maishan Inc. 维护的 Outlook 账户与邮件管理面板，提供 Web UI、批量导入、邮件检索、标签管理、API Key 管理和接口文档。

仓库地址：
<https://github.com/Maishan-Inc/OutlookManager>

镜像地址：
`maishanhub/outlookmanager:main`

<a id="quick-links"></a>
## 快速跳转

- [项目预览](#project-preview)
- [docker-compose 一键部署](#deploy-docker-compose)
- [线上部署前必看](#deployment-notes)
- [Railway 在线部署](#deploy-railway)
- [Zeabur 在线部署](#deploy-zeabur)
- [ClawCloud Run 在线部署](#deploy-clawcloud-run)
- [本地 Docker 部署](#deploy-docker)
- [首次使用流程](#first-run)
- [开发与调试](#development)
- [开源说明](#license-note)

<a id="deployment-shortcuts"></a>
## 部署入口

- [Railway 控制台](https://railway.com/)
- [Zeabur 控制台](https://zeabur.com/)
- [ClawCloud Run 控制台](https://run.claw.cloud/)
- [Railway 官方文档](https://docs.railway.com/guides/github)
- [Zeabur 官方文档](https://zeabur.com/docs/deploy-from-git)
- [ClawCloud Run 官方文档](https://docs.run.claw.cloud/)

<a id="deploy-docker-compose"></a>
## docker-compose 一键部署

如果你已经拉取了仓库，直接执行下面命令就可以部署：

```bash
git clone https://github.com/Maishan-Inc/OutlookManager.git
cd OutlookManager
docker-compose up -d
```

部署完成后默认访问：

- Web：<http://127.0.0.1:8073/>

说明：

- 当前仓库内的 [docker-compose.yml](./docker-compose.yml) 默认会拉取镜像 `maishanhub/outlookmanager:main`
- 默认把本地 `./data` 映射到容器 `/app/data`
- 默认端口映射为 `8073:8000`
- 如果你的环境使用新版 Docker Compose 插件，也可以使用 `docker compose up -d`

## 核心特性

- Outlook 账户管理、批量导入、快速检索
- 收件箱 / 垃圾箱邮件查看与详情展示
- 邮件主题、发件人、内容搜索
- API Key 创建、停用、调用记录查看
- 内置 API 文档，适合二次开发与自动化接入
- 默认支持 Docker 部署，适合线上长期运行
- 首次访问支持初始化管理员密码与使用协议确认

<a id="project-preview"></a>
## 项目预览

### 初始化

![同意协议](<docs/images/Agree to the agreement.jpg>)

![设置管理员密码](<docs/images/Set administrator password.jpg>)

### 账户管理

![账户管理](docs/images/account-management.jpg)

![添加账户](<docs/images/add account.jpg>)

![批量添加账户](docs/images/account-add.jpg)

### 邮件与 API

![邮件列表](docs/images/email-list.jpg)

![API 密钥](docs/images/api-key.jpg)

![API 文档](docs/images/api-docs.jpg)

<a id="deployment-notes"></a>
## 线上部署前必看

无论你使用 Railway、Zeabur 还是 ClawCloud Run，部署时都建议保持以下约束：

- 持久化目录挂载到 `/app/data`
- 服务对内端口使用 `8000`
- 首次上线后访问 Web 页面完成初始化
- 如果暴露到公网，请自行配置 HTTPS、访问控制、日志审计和备份
- 若需要长期稳定运行，优先绑定自定义域名

推荐环境变量：

| 变量名 | 推荐值 | 说明 |
| --- | --- | --- |
| `HOST` | `0.0.0.0` | 容器内监听地址 |
| `PORT` | `8000` | 容器内监听端口 |
| `DATA_DIR` | `/app/data` | 数据目录 |
| `ACCOUNTS_FILE` | `/app/data/accounts.json` | 账户数据文件 |
| `PYTHONUNBUFFERED` | `1` | 便于查看日志 |

健康检查地址：

- `GET /api/auth/state`

<a id="deploy-railway"></a>
## Railway 在线部署

Railway 适合直接从 GitHub 仓库快速上线，也适合直接使用 Docker 镜像部署。

跳转链接：

- [打开 Railway 控制台](https://railway.com/)
- [Railway GitHub 部署文档](https://docs.railway.com/guides/github)
- [Railway Dockerfile 文档](https://docs.railway.com/guides/dockerfiles)
- [Railway Volumes 文档](https://docs.railway.com/guides/volumes)

### 方式一：从 GitHub 仓库部署

1. 登录 Railway。
2. 创建新项目并选择 `Deploy from GitHub repo`。
3. 选择仓库 `Maishan-Inc/OutlookManager`。
4. Railway 会检测到仓库内的 `Dockerfile` 并自动构建部署。
5. 为服务添加持久化卷，并挂载到 `/app/data`。
6. 设置环境变量：
   `HOST=0.0.0.0`
   `PORT=8000`
   `DATA_DIR=/app/data`
   `ACCOUNTS_FILE=/app/data/accounts.json`
7. 等待部署完成后打开 Railway 分配的域名。
8. 首次进入页面后完成管理员密码初始化。

### 方式二：直接使用镜像部署

1. 在 Railway 新建服务。
2. 选择 Docker Image 方式。
3. 填入镜像：`maishanhub/outlookmanager:main`
4. 挂载持久化卷到 `/app/data`。
5. 设置与上面一致的环境变量。
6. 发布后访问域名完成初始化。

### Railway 备注

- Railway 是最省事的部署方式之一。
- 只要卷路径正确，服务重启后数据会保留。
- 建议绑定自定义域名并启用 HTTPS。

返回：
- [回到快速跳转](#quick-links)

<a id="deploy-zeabur"></a>
## Zeabur 在线部署

Zeabur 适合可视化部署和 GitHub 仓库直连部署。

跳转链接：

- [打开 Zeabur 控制台](https://zeabur.com/)
- [Zeabur Git 部署文档](https://zeabur.com/docs/deploy-from-git)
- [Zeabur Dockerfile 文档](https://zeabur.com/docs/deploy-from-dockerfile)
- [Zeabur 存储文档](https://zeabur.com/docs/service/storage)

### 方式一：导入 GitHub 仓库

1. 登录 Zeabur。
2. 新建 Project。
3. 选择从 GitHub 导入仓库：
   `https://github.com/Maishan-Inc/OutlookManager`
4. Zeabur 检测到 `Dockerfile` 后会按容器方式构建。
5. 为服务添加持久化存储，挂载目录设为 `/app/data`。
6. 设置环境变量：
   `HOST=0.0.0.0`
   `PORT=8000`
   `DATA_DIR=/app/data`
   `ACCOUNTS_FILE=/app/data/accounts.json`
7. 等待部署完成后，使用分配域名访问。

### 方式二：使用 Docker 镜像

1. 在 Zeabur 创建新服务。
2. 选择容器 / Docker Image 方式。
3. 填入镜像：`maishanhub/outlookmanager:main`
4. 添加卷并挂载到 `/app/data`。
5. 配置环境变量并发布。

### Zeabur 备注

- Zeabur 界面化程度更高，适合不想手动写部署脚本的场景。
- 绑定域名和 HTTPS 也都可以直接在平台内继续配置。

返回：
- [回到快速跳转](#quick-links)

<a id="deploy-clawcloud-run"></a>
## ClawCloud Run 在线部署

按当前 ClawCloud Run 官方文档，推荐使用 `App Launchpad` 以容器 / Docker 镜像方式部署本项目，而不是按 VPS 方式处理。

这里我根据官方文档做的判断是：
- ClawCloud Run 当前更贴近托管容器应用流程
- 这个项目最适合直接使用公开镜像 `maishanhub/outlookmanager:main`
- 数据目录仍然应挂载到 `/app/data`

跳转链接：

- [打开 ClawCloud Run 控制台](https://run.claw.cloud/)
- [ClawCloud Run 文档首页](https://docs.run.claw.cloud/)
- [App Launchpad 文档](https://docs.run.claw.cloud/Container%20Service/App%20Launchpad/)
- [Deploy Container Registry Services 文档](https://docs.run.claw.cloud/Container%20Service/Deploy%20Container%20Registry%20Services/)
- [Configuration Files 文档](https://docs.run.claw.cloud/Container%20Service/Configuration%20Files/)
- [FAQ 文档](https://docs.run.claw.cloud/Container%20Service/FAQ/)

### 推荐方式：App Launchpad + Docker 镜像

1. 登录 ClawCloud Run。
2. 进入 `App Launchpad`。
3. 创建一个新的应用。
4. 选择容器镜像部署方式。
5. 填入镜像：`maishanhub/outlookmanager:main`
6. 将服务端口设置为 `8000`。
7. 配置环境变量：
   `HOST=0.0.0.0`
   `PORT=8000`
   `DATA_DIR=/app/data`
   `ACCOUNTS_FILE=/app/data/accounts.json`
8. 如平台支持持久化存储，请将数据目录挂载到 `/app/data`。
9. 发布后通过平台生成的公网地址访问。
10. 首次进入页面后完成管理员密码初始化。

### 需要注意的点

- 若你不挂载持久化目录，容器重建后数据可能丢失。
- 如果要长期使用，建议绑定域名并启用 HTTPS。
- 如果你需要更复杂的资源配置，可以继续参考 `Configuration Files` 文档。

### 关于 GitHub 仓库直连

我根据当前可检索到的 ClawCloud Run 官方文档，优先给出的是 `App Launchpad + Docker 镜像` 方案，因为这是最直接且与本项目最匹配的官方容器部署路径。

如果你后续要把 ClawCloud Run 再细化成“仓库直连构建”版本，我可以再按你指定的平台界面路径继续补一版。

返回：
- [回到快速跳转](#quick-links)

<a id="deploy-docker"></a>
## 本地 / 自托管 Docker 部署

如果你不走云平台，也可以直接在自己的服务器或本地机器使用 Docker Compose。

```bash
git clone https://github.com/Maishan-Inc/OutlookManager.git
cd OutlookManager
docker compose up -d
```

相关文件：

- [docker-compose.yml](./docker-compose.yml)
- [Dockerfile](./Dockerfile)
- [docker.env.example](./docker.env.example)

<a id="first-run"></a>
## 首次使用流程

1. 打开首页。
2. 阅读并同意使用协议。
3. 设置管理员密码。
4. 登录后台。
5. 添加单个 Outlook 账户，或使用批量导入。
6. 进入邮件页面查看收件箱 / 垃圾箱邮件。
7. 如需程序化调用，可进入 API 密钥页面创建 Key。

<a id="structure"></a>
## 目录结构

```text
OutlookManager/
├─ main.py
├─ static/
│  ├─ index.html
│  └─ favicon.ico
├─ docs/
│  └─ images/
├─ data/
├─ Dockerfile
├─ docker-compose.yml
├─ docker-entrypoint.sh
├─ docker.env.example
├─ requirements.txt
└─ README.md
```

<a id="development"></a>
## 开发与调试

### 本地运行

```bash
pip install -r requirements.txt
python main.py
```

默认访问地址：

- Web：<http://127.0.0.1:8073/>
- API 文档：<http://127.0.0.1:8073/docs>

### 关键接口

- `GET /api/auth/state`
- `POST /api/auth/setup`
- `POST /api/auth/login`
- `POST /api/auth/logout`
- `GET /accounts`
- `POST /accounts`
- `GET /emails/{email_id}`
- `GET /emails/{email_id}/{message_id}`
- `GET /api/api-keys`
- `POST /api/api-keys`

<a id="license-note"></a>
## 开源说明

本项目为 CC BY-NC 4.0 (署名-非商业性使用 4.0 国际) 许可证 开源程序，适合学习、研究、测试与自部署使用。

如果你计划将其用于商业化项目、对外收费服务或深度定制交付，建议先与 Maishan Inc. 沟通商业化方案。

## 联系我们
官网：
<https://www.maishanzero.com>

全球区联系邮箱. 
<support@maishanzero.com>

中国区联系邮箱. 
<maishanemail@qq.com>

友链.linux.do
<https://linux.do>
