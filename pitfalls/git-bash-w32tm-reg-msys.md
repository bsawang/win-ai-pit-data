---
id: git-bash-w32tm-reg-msys
title: Git Bash 下 w32tm/reg 等带 / 参数命令被 MSYS 转路径
type: windows_pitfall
importance: 5
metadata:
  type: windows_pitfall
  symptom: 在 Git Bash 运行 w32tm /query /status 或 reg query ... /v NtpServer 时，报『C:/Program Files/Git/query 未知命令』/『无效语法』
  root_cause: MSYS 把以 / 开头的参数误当作 POSIX 路径转译成 C:/Program Files/Git/...
  solution: 用双斜杠转义如 w32tm //query //status，或执行前 export MSYS_NO_PATHCONV=1 关闭路径转换
  environment: &id001
    tool: w32tm / reg
  severity: medium
symptom: 在 Git Bash 运行 w32tm /query /status 或 reg query ... /v NtpServer 时，报『C:/Program Files/Git/query 未知命令』/『无效语法』
root_cause: MSYS 把以 / 开头的参数误当作 POSIX 路径转译成 C:/Program Files/Git/...
solution: 用双斜杠转义如 w32tm //query //status，或执行前 export MSYS_NO_PATHCONV=1 关闭路径转换
environment: *id001
severity: medium
---

## 症状

在 Git Bash 运行 w32tm /query /status 或 reg query ... /v NtpServer 时，报『C:/Program Files/Git/query 未知命令』/『无效语法』

## 根因

MSYS 把以 / 开头的参数误当作 POSIX 路径转译成 C:/Program Files/Git/...

## 解决

用双斜杠转义如 w32tm //query //status，或执行前 export MSYS_NO_PATHCONV=1 关闭路径转换

