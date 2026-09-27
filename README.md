# Web Basic Assignment

Bài tập môn **Lập trình Web** sử dụng **WSL2, Docker Compose, Nginx, Node-RED, MariaDB, phpMyAdmin và Cloudflare Tunnel**.

Project triển khai 2 website với 2 domain khác nhau, xây dựng API bằng Node-RED, kết nối MariaDB và dùng JavaScript `fetch()` để gọi API từ website.

---

## Công nghệ sử dụng

- Windows 11 + WSL2 Ubuntu
- Docker / Docker Compose
- Nginx
- Node-RED
- MariaDB
- phpMyAdmin
- Cloudflare Tunnel
- HTML / CSS / JavaScript
- Git / GitHub

---

## Kiến trúc hệ thống

```text
Internet
   ↓
Cloudflare
   ↓
Cloudflare Tunnel
   ↓
Nginx
   ├── web1.htphuc.id.vn → Website 1
   ├── web2.htphuc.id.vn → Website 2
   └── api.htphuc.id.vn
              ↓
           Node-RED
              ↓
           MariaDB

```

## Domain

- Website 1: `https://web1.htphuc.id.vn`
- Website 2: `https://web2.htphuc.id.vn`
- API: `https://api.htphuc.id.vn/api/students`

## Docker Compose Services

Project chạy các service:
nginx,
nodered,
mariadb,
phpmyadmin,
cloudflared,


Khởi động project:

```bash
docker compose up -d
```

Kiểm tra container:

```bash
docker ps
```

## Node-RED API

API sử dụng:
```text
HTTP In
↓
Query Students
↓
MariaDB
↓
Create Student Response
↓
HTTP Response
```

Endpoint:

GET /api/students


Ví dụ JSON trả về:

```json
{
  "ok": 1,
  "msg": "Thành công",
  "students": [
    {
      "id": 1,
      "name": "Hoàng Phúc",
      "money": 123
    },
    {
      "id": 2,
      "name": "David",
      "money": 456
    },
    {
      "id": 3,
      "name": "Alice",
      "money": 789
    }
  ]
}
```

## MariaDB

Database: webdb


Table: students


Node-RED query:

```js
msg.topic = "SELECT id, name, money FROM webdb.students";
return msg;
```

## JavaScript gọi API

Website 1 sử dụng `fetch()` để gọi API:

```js
const response = await fetch(
    "https://api.htphuc.id.vn/api/students"
);

const data = await response.json();
```

Dữ liệu sau đó được hiển thị lên bảng HTML.

## Nginx

Nginx được dùng để:

- Host 2 website với 2 domain khác nhau.
- Reverse proxy API từ `api.htphuc.id.vn` tới Node-RED.

Luồng API:
```text
api.htphuc.id.vn
↓
Nginx
↓
Node-RED
↓
MariaDB

```
## Cloudflare Tunnel

Cloudflare Tunnel giúp public hệ thống ra Internet mà không cần mở port router.

Các hostname:
```text
web1.htphuc.id.vn → nginx:80
web2.htphuc.id.vn → nginx:80
api.htphuc.id.vn → nginx:80
```

## Evidence

Evidence là thư mục được dùng để lưu các ảnh

---
**Hoàng Phúc**
Computer Engineering Student
