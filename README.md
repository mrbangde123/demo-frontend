# demo-frontend

Vue 3 + Vite 极简前端，用于调用 `demo-backend-main` 和 `demo-backend-client` 两个后端服务，验证前后端链路。

## 技术栈

- Vue 3.5
- Vite 6
- 原生 fetch 调用后端（没引入 axios，保持最小依赖）

## 本地开发

### 1. 安装依赖

```bash
npm install
```

### 2. 启动开发服务器

```bash
npm run dev
```

默认监听 `http://localhost:5173/`。

### 3. 打包生产资源

```bash
npm run build
```

产物在 `dist/`，之后会被 Nginx 容器挂载成静态站点。

## 对接的后端接口

| 接口 | 地址 | 说明 |
|---|---|---|
| 主服务健康检查 | `GET http://localhost:8080/health` | 返回服务状态、环境、版本 |
| 子服务信息 | `GET http://localhost:8081/api/client/info` | 返回 client 服务信息 |

## 前置条件

启动前端前，确保两个后端服务都在运行：
- `demo-backend-main` 监听 8080
- `demo-backend-client` 监听 8081

两个后端都需要配置好 CORS，允许 `http://localhost:5173` 访问。

## 目录结构

```
demo-frontend/
├── index.html          # HTML 入口
├── package.json        # 依赖与脚本
├── vite.config.js      # Vite 配置
├── public/             # 静态资源（不参与构建处理）
└── src/
    ├── main.js         # JS 入口
    ├── App.vue         # 根组件
    └── style.css       # 全局样式
```
