# Dockerfile Templates

通用的 Dockerfile 模板集合，提供各种项目场景的最佳实践参考。

## 模板列表

| 目录 | 基础镜像 | 适用场景 |
|------|---------|----------|
| `python-with-uv/` | python | Python 项目（推荐，构建快） |
| `python-with-pip/` | python | Python 项目（传统方式） |
| `php-fpm/` | php-fpm | PHP 项目 |
| `nginx/` | nginx:alpine | 静态网站 / 反向代理 |
| `node/` | node:alpine | Node.js 项目 |
| `go/` | scratch | Go 项目 |
| `rust/` | scratch | Rust 项目 |
| `cron-light/` | alpine | 定时任务（轻量版） |
| `cron-full/` | debian-slim | 定时任务（完整版） |

## 使用方式

1. 复制对应模板的 Dockerfile 到你的项目
2. 根据需要调整 `CMD` 命令
3. 按需修改构建参数默认值

## 模板特性

- **多阶段构建** — 减小镜像体积
- **BuildKit 缓存** — 加速依赖安装
- **非 root 用户** — 安全运行
- **层缓存优化** — 代码变更时快速重建
- **版本参数化** — 灵活指定 Python/工具版本

## 构建参数

```bash
# uv 版本
docker build \
  --build-arg UV_VERSION=0.5.7 \
  --build-arg PYTHON_VERSION=3.13.7 \
  -t my-app .

# pip 版本
docker build \
  --build-arg PYTHON_VERSION=3.13.7 \
  -t my-app .
```

## CMD 示例

根据你的项目需要调整启动命令：

```dockerfile
# FastAPI (uvicorn)
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]

# FastAPI (gunicorn + uvicorn workers)
CMD ["gunicorn", "main:app", "-w", "4", "-k", "uvicorn.workers.UvicornWorker", "-b", "0.0.0.0:8000"]

# Flask
CMD ["gunicorn", "-w", "4", "-b", "0.0.0.0:5000", "app:app"]

# Django
CMD ["gunicorn", "myproject.wsgi:application", "-b", "0.0.0.0:8000"]

# 通用 Python 脚本
CMD ["python", "main.py"]
```
