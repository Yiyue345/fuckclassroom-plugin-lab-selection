# FuckClassroom 实验课选课插件

FuckClassroom 的独立实验课选课插件。插件 ID 为 `lab_selection`。

## 功能

- 按学期查询实验课程
- 查看实验项目、分组、时间、地点与名额
- 整组选课
- 单个实验的组合时间段选课
- 本地自动候补：定期检查可选状态并在满足条件时尝试提交
- 与 FuckClassroom 共用本科教务登录、凭据、WebVPN/代理和会话基础设施

> 自动候补是本地自动任务，不是学校提供的排队队列，也不保证能够选上。

## 兼容性

- FuckClassroom: `>=0.1,<0.2`
- Plugin API: `1`
- Python dependency: `playwright>=1.45`
- Required host plugin: `core_ui`

## 安装

当前 FuckClassroom 插件管理器支持从本机插件目录或 ZIP 安装。

1. 下载本仓库源码或 Release ZIP。
2. 在 FuckClassroom 的“插件管理”页面安装该目录/ZIP。
3. 安装或修复依赖；Playwright 安装后需要 Chromium。
4. 启用“实验课选课”。

插件安装后由 FuckClassroom 放入 `data/plugins/lab_selection/`。候补任务保存在主程序数据目录的 `data/lab_selection/waitlist.json`。

## 开发结构

```text
.
├── plugin.json
├── plugin.py
├── client.py
├── routes.py
├── services.py
├── waitlist.py
├── requirements.txt
├── templates/
│   └── lab_selection/
│       └── index.html
├── static/
│   ├── lab_selection.css
│   └── lab_selection.js
└── docs/
    └── protocol.md
```

`plugin.py` 是插件入口；其余模块通过插件包内相对导入加载。对宿主能力的访问使用 `fuckclassroom.*` 绝对导入。

## 安全约束

上游实验教学系统的选课操作虽然表现为 GET 请求，本插件只会从本地显式 POST 操作触发提交。提交结果不明确时不会自动重试，以避免重复选课。自动候补会在提交前持久化状态并在异常情况下转为人工核对。

接口来源、验证范围和详细行为见 [docs/protocol.md](docs/protocol.md)。
