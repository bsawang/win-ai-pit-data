---
id: electron-run-as-node-1-electron-node
title: ELECTRON_RUN_AS_NODE=1 导致 Electron 以纯 Node 模式启动
type: windows_pitfall
importance: 5
metadata:
  type: windows_pitfall
  symptom: 启动 electron 应用无 GUI；main.js 里 require("electron") 的 app 为 undefined 报错；process.versions.electron 有值但 process.type 为 undefined
  root_cause: 环境变量 ELECTRON_RUN_AS_NODE=1（Claude Code harness 注入到 bash 环境）让 Electron 以纯 Node 模式启动：不加载 GUI、不做 require("electron") 拦截，require("electron") 返回 npm 包导出的可执行文件路径字符串
  solution: 启动前清除该变量：bash 用 env -u ELECTRON_RUN_AS_NODE electron . ；bat 里 set ELECTRON_RUN_AS_NODE= 再启动；注意 npm 装 electron 只装 JS 壳，二进制首次运行才下载（国内慢可设 ELECTRON_MIRROR=https://npmmirror.com/mirrors/electron/ 后重跑 node node_modules/electron/install.js）
  environment: &id001
    tool: electron
  severity: medium
symptom: 启动 electron 应用无 GUI；main.js 里 require("electron") 的 app 为 undefined 报错；process.versions.electron 有值但 process.type 为 undefined
root_cause: 环境变量 ELECTRON_RUN_AS_NODE=1（Claude Code harness 注入到 bash 环境）让 Electron 以纯 Node 模式启动：不加载 GUI、不做 require("electron") 拦截，require("electron") 返回 npm 包导出的可执行文件路径字符串
solution: 启动前清除该变量：bash 用 env -u ELECTRON_RUN_AS_NODE electron . ；bat 里 set ELECTRON_RUN_AS_NODE= 再启动；注意 npm 装 electron 只装 JS 壳，二进制首次运行才下载（国内慢可设 ELECTRON_MIRROR=https://npmmirror.com/mirrors/electron/ 后重跑 node node_modules/electron/install.js）
environment: *id001
severity: medium
---

## 症状

启动 electron 应用无 GUI；main.js 里 require("electron") 的 app 为 undefined 报错；process.versions.electron 有值但 process.type 为 undefined

## 根因

环境变量 ELECTRON_RUN_AS_NODE=1（Claude Code harness 注入到 bash 环境）让 Electron 以纯 Node 模式启动：不加载 GUI、不做 require("electron") 拦截，require("electron") 返回 npm 包导出的可执行文件路径字符串

## 解决

启动前清除该变量：bash 用 env -u ELECTRON_RUN_AS_NODE electron . ；bat 里 set ELECTRON_RUN_AS_NODE= 再启动；注意 npm 装 electron 只装 JS 壳，二进制首次运行才下载（国内慢可设 ELECTRON_MIRROR=https://npmmirror.com/mirrors/electron/ 后重跑 node node_modules/electron/install.js）

