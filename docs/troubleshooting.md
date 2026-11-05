## 问题：系统找不到 curl 命令

- 现象：运行 `curl -i http://localhost` 时，系统提示找不到 `curl`。
- 我检查了什么：查看终端提示，确认需要先安装 curl。
- 原因：Ubuntu 中还没有安装 curl 这个 HTTP 请求工具。
- 如何修复：运行 `sudo apt install curl`。
- 我学到什么：curl 是用来发送 HTTP 请求的客户端工具，可以用它检查 Nginx 是否返回网页。

## 问题：网页返回 404

- 现象：访问 `http://127.0.0.1:8080/` 返回 `404 Not Found`。
- 我检查了什么：Nginx 是 `active (running)`，`nginx -t` 也通过了。
- 原因：配置中的 `root` 临时指向了不存在的 `/var/www/not-a-real-site`。
- 如何修复：把 `root` 改回 `/var/www/linux-web-lab`，复制配置、运行 `sudo nginx -t`，再运行 `sudo systemctl reload nginx`。
- 我学到什么：`active (running)` 表示服务在运行，不代表网页路径正确；`404` 表示请求到达了服务器，但找不到对应网页。

## 问题：网页返回 403 Forbidden

- 现象：访问网页时返回 `403 Forbidden`。
- 我检查了什么：用 `ls -l /var/www/linux-web-lab/index.html` 检查网页文件权限，发现权限被临时设成了 `----------`。
- 原因：Nginx 进程无法读取网页文件。
- 如何修复：运行 `sudo chmod 644 /var/www/linux-web-lab/index.html`，再用 `curl -i http://127.0.0.1:8080/` 确认返回 `200 OK`。
- 我学到什么：404 表示找不到网页；403 表示服务器找到了请求，但没有权限访问。

## 问题：Nginx 配置语法检查失败

- 现象：运行 `sudo nginx -t` 时，提示 `unknown directive "lissten"`。
- 我检查了什么：用 `sudo nginx -t` 检查 Nginx 配置。
- 原因：把配置指令 `listen` 拼成了 `lissten`。
- 如何修复：改回 `listen`，复制配置到 `/etc/nginx/sites-available/linux-web-lab`，重新运行 `sudo nginx -t`；通过后再重载 Nginx。
- 我学到什么：重载前先运行 `nginx -t`，可以先发现语法错误，避免加载错误配置。
