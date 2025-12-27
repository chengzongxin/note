好，那我按你说的 **「jsdoctor 暂时用端口」**，给你一份**最终版、可直接上线、不冲突的完整 Nginx 配置**。  
这个方案 **只有一个 443**，jsdoctor 走 **HTTPS + 端口 8889**，最稳妥。

---

# ✅ 最终 Nginx 配置方案（推荐）

## 一、整体结构说明

- **temu_app**
    
    - 域名：[https://www.chengfamily.site](https://www.chengfamily.site)
        
    - 后端：127.0.0.1:8888
        
- **jsdoctor**
    
    - 访问方式：https://www.chengfamily.site:8889
        
    - 后端：127.0.0.1:8000
        
- **证书**：仅使用 `www.chengfamily.site` 一张证书
    
- **不会有 443 冲突**
    

---

## 二、temu_app 配置（80 → 443）

📄 `/etc/nginx/sites-available/temu_app.conf`

`server {     listen 80;     listen [::]:80;     server_name www.chengfamily.site;      # HTTP 强制跳 HTTPS     return 301 https://$host$request_uri; }  server {     listen 443 ssl http2;     listen [::]:443 ssl http2;      server_name www.chengfamily.site;      ssl_certificate /etc/letsencrypt/live/www.chengfamily.site/fullchain.pem;     ssl_certificate_key /etc/letsencrypt/live/www.chengfamily.site/privkey.pem;     include /etc/letsencrypt/options-ssl-nginx.conf;     ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;      root /var/www/html_temu;     index index.html index.htm;      location / {         try_files $uri /index.html;     }      location /api/ {         proxy_pass http://127.0.0.1:8888/api/;         proxy_set_header Host $host;         proxy_set_header X-Real-IP $remote_addr;         proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;         proxy_set_header X-Forwarded-Proto $scheme;     }      location /uploads/ {         proxy_pass http://127.0.0.1:8888/uploads/;         proxy_set_header Host $host;         proxy_set_header X-Real-IP $remote_addr;          expires 1h;         add_header Cache-Control "public";     } }`

---

## 三、jsdoctor 端口 HTTPS 配置（8889）

📄 `/etc/nginx/sites-available/jsdoctor.conf`

`server {     listen 8889 ssl;     listen [::]:8889 ssl;      server_name _;      ssl_certificate /etc/letsencrypt/live/www.chengfamily.site/fullchain.pem;     ssl_certificate_key /etc/letsencrypt/live/www.chengfamily.site/privkey.pem;     include /etc/letsencrypt/options-ssl-nginx.conf;     ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;      root /var/www/html;     index index.html index.htm;      location / {         try_files $uri /index.html;     }      location /api/ {         proxy_pass http://127.0.0.1:8000/api/;         proxy_set_header Host $host;         proxy_set_header X-Real-IP $remote_addr;         proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;         proxy_set_header X-Forwarded-Proto $scheme;     } }`

---

## 四、启用配置（一步不漏）

`# 1. 禁用默认配置 sudo rm -f /etc/nginx/sites-enabled/default  # 2. 启用新配置 sudo ln -s /etc/nginx/sites-available/temu_app.conf /etc/nginx/sites-enabled/ sudo ln -s /etc/nginx/sites-available/jsdoctor.conf /etc/nginx/sites-enabled/  # 3. 测试配置 sudo nginx -t  # 4. 重新加载 sudo systemctl reload nginx`

---

## 五、访问方式确认

|服务|地址|
|---|---|
|temu_app|[https://www.chengfamily.site](https://www.chengfamily.site)|
|temu_api|https://www.chengfamily.site/api|
|jsdoctor|https://www.chengfamily.site:8889|
|jsdoctor_api|https://www.chengfamily.site:8889/api|

---

## 六、你现在的配置是「规范 + 可扩展」

✔ 443 只用一次  
✔ HTTPS 正确  
✔ jsdoctor 随时可迁移到子域名  
✔ Certbot 不会炸  
✔ Spring Boot 端口清晰

---

## 七、下一步我可以帮你做的

- 🚀 一键 **systemd 管理 jsdoctor / core-service**
    
- 🔄 jsdoctor 从端口 **平滑切换子域名**
    
- 📦 Nginx + Spring Boot **生产安全加固**
    
- 🧾 自动续签证书验证
    

你现在可以直接照这份配置执行，**如果 `nginx -t` 报错，把错误原样贴出来我帮你秒修**。



如果要把jsdoctor迁移到子域名，需要做什么，要在申请一个子域名证书吗

这是个**非常好的问题**，而且你现在的架构**正好处在最容易迁移的阶段** 👍  
我按**“结论先行 → 实操步骤”**给你讲清楚。

---

## 一、结论先行（你最关心的）

### ❓要不要重新申请证书？

**👉 是的，需要把「子域名加入证书」**

但有两个好消息：

1. **不用单独一张新证书**
    
2. **Certbot 可以在原证书上“扩展域名”**
    

也就是说，你现在的证书：

`www.chengfamily.site`

可以升级成：

`www.chengfamily.site jsdoctor.chengfamily.site`

**还是一张证书，一个文件路径。**

---

## 二、迁移 jsdoctor 到子域名需要做的 4 件事

### ✅ 第 1 步：DNS 添加子域名（必须）

在你的 DNS 控制台（阿里云 / 腾讯云 / DNSPod 等）新增一条：

`记录类型：A 主机记录：jsdoctor 记录值：你的服务器公网 IP`

等待生效（通常 1～5 分钟，最长几小时）

验证：

`ping jsdoctor.chengfamily.site`

---

### ✅ 第 2 步：用 Certbot 给证书「加域名」

⚠️ **不是 new cert，是 expand / reissue**

推荐命令（nginx 插件）：

`sudo certbot --nginx \   -d www.chengfamily.site \   -d jsdoctor.chengfamily.site`

Certbot 会：

- 自动修改证书
    
- 覆盖原来的 `/etc/letsencrypt/live/www.chengfamily.site/`
    
- 不影响 temu_app
    

成功后你会看到：

`Congratulations! Your certificate and chain have been saved at: ...`

👉 **路径不变，域名变多了**

---

### ✅ 第 3 步：新增 jsdoctor 的 Nginx server（443）

新建配置文件：

📄 `/etc/nginx/sites-available/jsdoctor.conf`

`server {     listen 80;     server_name jsdoctor.chengfamily.site;     return 301 https://$host$request_uri; }  server {     listen 443 ssl http2;     server_name jsdoctor.chengfamily.site;      ssl_certificate /etc/letsencrypt/live/www.chengfamily.site/fullchain.pem;     ssl_certificate_key /etc/letsencrypt/live/www.chengfamily.site/privkey.pem;     include /etc/letsencrypt/options-ssl-nginx.conf;     ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;      root /var/www/html;     index index.html index.htm;      location / {         try_files $uri /index.html;     }      location /api/ {         proxy_pass http://127.0.0.1:8000/api/;         proxy_set_header Host $host;         proxy_set_header X-Real-IP $remote_addr;         proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;         proxy_set_header X-Forwarded-Proto $scheme;     } }`

启用并重载：

`sudo ln -s /etc/nginx/sites-available/jsdoctor.conf /etc/nginx/sites-enabled/ sudo nginx -t sudo systemctl reload nginx`

---

### ✅ 第 4 步：下线端口方案（可选）

确认新地址 OK 后，删除原来的 8889 配置：

`sudo rm /etc/nginx/sites-enabled/jsdoctor_port.conf sudo systemctl reload nginx`

---

## 三、最终访问方式对比

|阶段|访问地址|
|---|---|
|当前|https://www.chengfamily.site:8889|
|迁移后|https://jsdoctor.chengfamily.site|

---

## 四、常见误区（帮你避坑）

### ❌ 误区 1：以为一个证书只能一个域名

👉 **错**，Let’s Encrypt 支持多域名（SAN）

### ❌ 误区 2：再申请一张新证书

👉 **没必要，反而更乱**

### ❌ 误区 3：两个 server 共用 `server_name _;`

👉 HTTPS 下**必炸**

---

## 五、迁移成功的标志

- `https://jsdoctor.chengfamily.site` 可访问
    
- 浏览器证书里能看到 **两个域名**
    
- `certbot renew --dry-run` 正常
    

---

## 六、我可以继续直接帮你的

如果你愿意，我可以下一步直接帮你：

- 🔐 **生成 certbot 扩展命令（按你当前环境）**
    
- 🧹 **把端口配置清理干净**
    
- 🔁 **给你做一份“端口 → 子域名”无感迁移方案**
    

你只需要一句话：  
👉 **“我现在就要迁移” 或 “等稳定后再迁移”**