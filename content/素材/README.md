# Goxue Engine

<p align="center">
  <img src="https://img.shields.io/badge/Go-1.21+-00ADD8?style=flat&logo=go" alt="Go Version">
  <img src="https://img.shields.io/badge/Beego-v2-blue?style=flat" alt="Beego Framework">
  <img src="https://img.shields.io/badge/Vue-3.0-brightgreen?style=flat&logo=vue.js" alt="Vue 3">
  <img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License">
</p>

一个面向儿童的互动式教育平台，支持富文本内容发布、Canvas 动画展示、Mermaid 思维导图，以及完善的内容管理系统。

## ✨ 核心特性

### 📝 内容创作与展示
- **Markdown 编辑器**: 基于 `v-md-editor` 的强大文本编辑功能
- **Canvas 动画**: 支持自定义 JavaScript 动画代码，实现交互式问题解析
  - 重播动画功能
  - 全屏播放支持
  - 响应式画布尺寸适配
- **Mermaid 思维导图**: 支持多种图表类型
  - 思维导图 (mindmap)
  - 流程图 (flowchart)
  - 时序图 (sequence diagram)
  - 甘特图 (gantt)
  - 以及更多...

### 🎨 交互式图表控制
- **缩放功能**: 放大/缩小思维导图
- **拖拽平移**: 自由拖动查看大型图表
- **旋转视图**: 支持 ±90° 旋转查看
- **重置视图**: 一键恢复默认视图
- **全屏模式**: 沉浸式查看体验

### 👤 用户系统
- **微信一键登录**: 集成微信授权
- **角色权限管理**: User / Admin 双角色体系
- **个人中心**: 用户信息管理

### 🛡️ 内容管理
- **审核流程**: 管理员审核机制
- **分类管理**: 内容分类与组织
- **公开访问**: 支持未登录用户浏览文章详情页

## 🏗️ 技术架构

### 后端技术栈
- **语言**: Go 1.21+
- **框架**: Beego v2
- **数据库**: MySQL
- **配置**: `.env` 环境变量管理
- **架构**: 模块化分层架构 (RESTful API)

### 前端技术栈
- **框架**: Vue 3 + TypeScript
- **UI 库**: Element Plus
- **构建工具**: Vite
- **状态管理**: Pinia
- **编辑器**: 
  - `@kangc/v-md-editor` (Markdown)
  - `mermaid` (图表渲染)
- **可视化增强**:
  - `svg-pan-zoom` (SVG 交互控制)
  - Canvas API (动画渲染)

### 移动端 (可选)
- **框架**: unibest (uni-app + Vue 3)
- **多端支持**: 可编译为 H5 或微信小程序

## 📁 项目结构

```
Goxue_Engine/
├── backend/                 # Go + Beego v2 后端
│   ├── conf/               # 应用配置
│   ├── modules/            # 业务模块
│   │   ├── user/          # 用户模块
│   │   ├── content/       # 内容模块
│   │   └── common/        # 公共模块
│   ├── routers/           # API 路由
│   ├── static/            # 静态资源
│   │   └── admin/        # 前端构建产物
│   ├── main.go
│   └── .env              # 环境配置
├── frontend/              # Vue 3 + Element Plus 管理后台
│   ├── src/
│   │   ├── api/          # API 接口
│   │   ├── views/        # 页面组件
│   │   ├── plugins/      # 插件配置 (v-md-editor)
│   │   └── types/        # TypeScript 类型定义
│   ├── vite.config.ts
│   └── package.json
└── mobile/               # uni-app 移动端 (可选)
    ├── src/
    └── package.json
```

## 🚀 快速开始

### 环境要求
- Go 1.24+
- Node.js 16+
- MySQL 8.0+

### 后端启动

1. **配置环境变量**
```bash
cd backend
# 直接编辑现有的 .env 文件
# 配置数据库连接等信息
```

2. **安装依赖**
```bash
go mod tidy
```

3. **初始化数据库**
```bash
# 执行 SQL 初始化脚不需要安装
# mysql -u root -p goxue_db < init.sql
```

4. **编译二进制包**
```bash
# 编译当前平台
# go build -o backend main.go
# 编译 Linux 平台
# GOOS=linux GOARCH=amd64 go build -o backend main.go
# 编译 Windows 平台
# GOOS=windows GOARCH=amd64 go build -o backend.exe main.go
# 编译 macOS 平台
GOOS=darwin GOARCH=amd64 go build -o backend main.go
```

5. **启动服务**
```bash
# 方式一：直接运行编译好的二进制文件
./backend
# 方式二：使用 go run 直接运行
# go run main.go
```

后端服务默认运行在 `http://localhost:8080`

### 前端启动

1. **安装依赖**
```bash
cd frontend
npm install
```

2. **配置 API 地址**
```bash
# 编辑 .env 或 vite.config.ts 中的 API 代理配置
```

3. **开发模式**
```bash
npm run dev
# 访问 http://localhost:5173
```

4. **生产构建**
```bash
npm run build
# 产物输出到 ../backend/static/admin
```

## 📖 API 文档

### 认证相关
- `POST /api/v1/auth/wechat-login` - 微信登录

### 内容管理
- `GET /api/v1/content` - 获取已审核内容列表（公开）
- `GET /api/v1/content/:id` - 获取内容详情（支持未登录访问）
- `POST /api/v1/content` - 创建内容（需登录）
- `PUT /api/v1/content/:id` - 更新内容（需登录）
- `DELETE /api/v1/content/:id` - 删除内容（需登录）

### 分类管理
- `GET /api/v1/category` - 获取分类列表
- `POST /api/v1/category` - 创建分类（管理员）
- `PUT /api/v1/category/:id` - 更新分类（管理员）
- `DELETE /api/v1/category/:id` - 删除分类（管理员）

### 管理员功能
- `GET /api/v1/admin/content/pending` - 待审核内容列表
- `PUT /api/v1/admin/content/:id/audit` - 审核内容
- `GET /api/v1/admin/users` - 用户管理

## 🎯 核心功能演示

### Canvas 动画示例
文章详情页支持嵌入自定义 Canvas 动画，通过 JavaScript 代码实现交互式教学：

```javascript
// Canvas 动画代码示例
const width = canvas.width;
const height = canvas.height;

// 绘制动画逻辑
function animate() {
  ctx.clearRect(0, 0, width, height);
  // 你的动画代码
  requestAnimationFrame(animate);
}
animate();
```

### Mermaid 思维导图示例
在 Markdown 内容中使用 Mermaid 语法：

```markdown
# 文章标题

这是正文内容...

​```mermaid
mindmap
  root((学习计划))
    数学
      加法
      减法
    语文
      阅读
      写作
​```
```

## 🔧 配置说明

### 后端配置 (`.env`)
```env
APP_NAME=GoxueEngine
HTTP_PORT=8080
RUN_MODE=dev

# 数据库配置
DB_DRIVER=mysql
DB_USER=goxue_db
DB_PASSWORD=G3k2n5KYwLaYX3kM
DB_HOST=192.168.1.116
DB_PORT=3306
DB_NAME=goxue_db
DB_CHARSET=utf8
DB_LOC=Local

# 微信配置
WECHAT_APP_ID=your_app_id
WECHAT_SECRET=your_secret
```

### 前端配置 (`.env`)
```env
VITE_API_BASE_URL=http://localhost:8080
VITE_APP_TITLE=Goxue Engine 管理后台
```

## 🤝 贡献指南

欢迎提交 Issue 和 Pull Request！

1. Fork 本仓库
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 开启 Pull Request

## 📝 开发路线图

- [x] 基础架构搭建
- [x] 用户认证系统
- [x] 内容管理系统
- [x] Markdown 编辑器集成
- [x] Canvas 动画支持
- [x] Mermaid 图表支持
- [x] 思维导图交互控制（缩放、拖拽、旋转）
- [x] 文章详情页公开访问
- [x] 响应式设计优化
- [ ] 需求反馈系统
- [ ] 移动端 H5/小程序
- [ ] 评分系统
- [ ] 搜索功能
- [ ] 内容推荐算法

## 📄 许可证

本项目采用 Apache 许可证 - 详见 [LICENSE](LICENSE) 文件

## 👨‍💻 作者

Zhang Erhao

---

⭐ 如果这个项目对你有帮助，请给我们一个 Star！
