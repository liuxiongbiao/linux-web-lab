# linux-web-lab

## 项目目标

通过在 Ubuntu 上部署一个静态网页，复习 Linux 的目录和文件操作、权限、服务管理、网络请求与故障排查，并记录实践过程，方便以后复习。

## 项目结构

- `site/`：网页源文件。
- `deploy/`：Nginx 站点配置。
- `docs/troubleshooting.md`：故障现象、检查过程、原因和修复记录。

网页源文件保存在项目的 `site/` 目录；Nginx 实际提供的文件位于 `/var/www/linux-web-lab/`。

## 请求过程

访问 `http://127.0.0.1:8080/` → Nginx → `/var/www/linux-web-lab/index.html`

站点配置只监听本机的 `127.0.0.1:8080`，本项目没有配置域名或 HTTPS。

## 部署步骤

### 1. 安装软件

```bash
sudo apt update
sudo apt install nginx curl
```

### 2. 复制网页到 Nginx 的网页目录

```bash
sudo mkdir -p /var/www/linux-web-lab
sudo cp site/index.html /var/www/linux-web-lab/
```

### 3. 安装并启用站点配置

```bash
sudo cp deploy/nginx-site.conf /etc/nginx/sites-available/linux-web-lab
sudo ln -s /etc/nginx/sites-available/linux-web-lab /etc/nginx/sites-enabled/linux-web-lab
```

`ln -s` 只需在首次启用站点时运行一次。检查配置并重载 Nginx：

```bash
sudo nginx -t
sudo systemctl reload nginx
```

### 4. 检查网页

```bash
sudo systemctl status nginx --no-pager
sudo ss -ltnp | grep 8080
curl -i http://127.0.0.1:8080/
```

成功时，`curl` 返回 `HTTP/1.1 200 OK` 和网页内容。

## 更新网页

修改 `site/index.html` 后，把新版本复制到 Nginx 的网页目录：

```bash
sudo cp site/index.html /var/www/linux-web-lab/
```

## 排错记录

遇到过的 `curl` 未安装、404、403 和 Nginx 配置语法错误，详见 [`docs/troubleshooting.md`](docs/troubleshooting.md)。
