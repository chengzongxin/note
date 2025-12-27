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