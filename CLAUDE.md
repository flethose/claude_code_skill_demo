# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目概述

这是一个单文件 HTML 前端页面，用于知识库文件导入功能。该页面提供文件上传（支持 PDF 和 Markdown 格式）和实时处理进度监控。

## 如何运行

直接在浏览器中打开 `import.html` 文件即可使用：

```bash
# Windows
start import.html

# 或者直接双击文件打开
```

**注意**：页面需要后端 API 服务运行在 `http://127.0.0.1:8000`，这是在代码中硬编码的（第 402 行）。

## 项目架构

这是一个纯前端单页应用，采用前后端分离架构：

### 前端 (import.html)
- **技术栈**：原生 HTML + CSS + JavaScript（无框架）
- **UI 风格**：科技感深色主题，使用渐变背景、毛玻璃效果、动画
- **核心功能**：
  - 文件拖拽/点击上传
  - 实时进度条显示
  - 处理状态轮询
  - 日志时间线展示

### 后端 API (外部依赖)
页面依赖以下 API 端点（需要 FastAPI 后端支持）：
- `POST /upload` - 文件上传，返回 task_id
- `GET /status/{task_id}` - 查询处理状态，返回：
  - `status`: processing/completed/failed
  - `done_list`: 已完成的节点列表
  - `running_list`: 运行中的节点列表
  - `durations`: 各节点耗时（秒）

## 核心逻辑

### 进度计算
- PDF 文件：8 个处理节点
- MD 文件：7 个处理节点（跳过 pdf_to_md）
- 上传阶段占 1/8 的进度
- 轮询间隔：1.5 秒

### 文件上传流程
1. 用户选择/拖拽文件
2. 调用 `/upload` 上传文件，获取 task_id
3. 开始轮询 `/status/{task_id}` 获取处理状态
4. 根据返回的 done_list 和 running_list 更新进度条和日志
5. 完成或失败时停止轮询

### UI 组件
- **上传区域**：支持点击和拖拽
- **文件卡片**：显示文件名、大小、状态徽章
- **进度条**：根据处理进度动态变色
- **日志详情**：可折叠的时间线，显示各节点执行情况和耗时

## 开发注意事项

- 后端 API 地址硬编码在第 402 行，如需修改请更新 `API_BASE` 常量
- 文件类型限制：仅支持 `.pdf` 和 `.md` 文件（第 389 行）
- 所有样式和逻辑都内联在 HTML 文件中，便于部署和测试
- 使用了现代 CSS 特性（backdrop-filter、渐变、动画），需要较新的浏览器支持

我也不知道加了什么。
