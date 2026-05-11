# QD-Optimized 更新日志

## v1.0.0-opt (2026-05-11)

基于 qd-today/qd 的优化版本，修复了多个安全漏洞和代码 Bug。

### 🔴 高危安全修复

1. **Dockerfile SSH 密钥泄露** — 移除 SSH 私钥打包，改用 HTTPS 克隆
2. **默认密钥硬编码检测** — COOKIE_SECRET 和 AES_KEY 使用默认值 "binux" 时打印安全警告
3. **PBKDF2 迭代次数提升** — 从 400 次提升至 600,000 次（OWASP 2023 推荐）
4. **SSL 证书验证** — 默认启用 SSL 验证（可通过 VALIDATE_CERT 环境变量关闭）
5. **XSS 修复** — 任务执行结果 HTML 输出使用 xhtml_escape 转义
6. **aes_decrypt 数据截断修复** — 修复 umsgpack ExtraData 异常时的错误截断逻辑

### 🟡 中危修复

7. **WebSocket Origin 校验** — 修复空 domain 时任意 origin 通过的问题，修复 endswith 匹配漏洞
8. **模板未闭合块检测** — parse 方法检测到未闭合的 for/if/while 块时抛出异常
9. **for 循环运算符优先级** — 修复 isinstance 检查的优先级错误

### 📝 环境变量新增

| 变量 | 说明 | 默认值 |
|------|------|--------|
| `VALIDATE_CERT` | 是否验证 SSL 证书 | `True` |
| `PBKDF2_ITERATIONS` | PBKDF2 迭代次数 | `600000` |

### ⚠️ 升级注意

- PBKDF2 迭代次数从 400 改为 600,000，已有用户需要重新设置密码
- SSL 验证默认开启，如果有自签名证书的站点需要设置 `VALIDATE_CERT=False`
