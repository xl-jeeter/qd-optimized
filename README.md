# QD-Optimized 🚀

基于 [qd-today/qd](https://github.com/qd-today/qd) 的安全优化版本。

## 修复的问题

### 🔴 高危安全修复 (6个)

| # | 问题 | 文件 | 修复方式 |
|---|------|------|----------|
| 1 | Dockerfile SSH 私钥泄露 | Dockerfile | 移除 SSH 打包，改用 HTTPS 克隆 |
| 2 | 默认密钥硬编码 "binux" | config.py | 启动时检测并打印安全警告 |
| 3 | PBKDF2 迭代仅 400 次 | config.py | 提升至 600,000 次 (OWASP 推荐) |
| 4 | 禁用 SSL 证书验证 | libs/fetcher.py | 默认启用验证，可通过环境变量关闭 |
| 5 | XSS 注入漏洞 | web/handlers/task.py | 使用 xhtml_escape 转义输出 |
| 6 | aes_decrypt 数据截断 | libs/mcrypto.py | 修复 ExtraData 异常处理逻辑 |

### 🟡 中危修复 (3个)

| # | 问题 | 文件 |
|---|------|------|
| 7 | WebSocket Origin 校验绕过 | web/handlers/subscribe.py |
| 8 | 模板未闭合块静默忽略 | libs/fetcher.py |
| 9 | for 循环运算符优先级错误 | libs/fetcher.py |

## 新增环境变量

```bash
# SSL 证书验证（默认开启）
VALIDATE_CERT=True

# PBKDF2 迭代次数（默认 600000）
PBKDF2_ITERATIONS=600000
```

## 升级注意

- ⚠️ PBKDF2 迭代次数变更，已有用户需重新设置密码
- ⚠️ SSL 验证默认开启，自签名证书站点需设置 `VALIDATE_CERT=False`

## 部署

```bash
# Docker
docker build -t qd-optimized .
docker run -p 8923:8923 -e COOKIE_SECRET=your_random_secret -e AES_KEY=your_random_key qd-optimized

# 或直接使用原版镜像 + 环境变量
docker run -p 8923:8923   -e COOKIE_SECRET=your_random_secret   -e AES_KEY=your_random_key   -e PBKDF2_ITERATIONS=600000   -e VALIDATE_CERT=True   qd-today/qd
```

## 基于

- 原项目: [qd-today/qd](https://github.com/qd-today/qd) (MIT License)
- 优化者: [xl-jeeter](https://github.com/xl-jeeter)
