---
id: git-rebase-untracked
title: git rebase 应用删除文件提交时误删工作区同名 untracked 文件
type: windows_pitfall
importance: 5
metadata:
  type: windows_pitfall
  symptom: untracked + gitignored 的关键文件（如 ~/.windows-pitfalls/kb.yaml），在执行 git pull --rebase 应用"git rm 该文件"的提交后从工作区消失，导致运行时报文件缺失
  root_cause: rebase 切换到目标提交并 apply 删除 diff 时按路径删除文件，不区分 tracked/untracked 的同名文件；untracked 文件无版本保护
  solution: 关键运行时文件避免做成 untracked + gitignored；程序提供 fallback（如 pyrite 本地 kb.yaml 缺失时读包内模板）；rebase 前先备份 untracked 文件
  environment: &id001
    tool: git
  severity: low
symptom: untracked + gitignored 的关键文件（如 ~/.windows-pitfalls/kb.yaml），在执行 git pull --rebase 应用"git rm 该文件"的提交后从工作区消失，导致运行时报文件缺失
root_cause: rebase 切换到目标提交并 apply 删除 diff 时按路径删除文件，不区分 tracked/untracked 的同名文件；untracked 文件无版本保护
solution: 关键运行时文件避免做成 untracked + gitignored；程序提供 fallback（如 pyrite 本地 kb.yaml 缺失时读包内模板）；rebase 前先备份 untracked 文件
environment: *id001
severity: low
---

## 症状

untracked + gitignored 的关键文件（如 ~/.windows-pitfalls/kb.yaml），在执行 git pull --rebase 应用"git rm 该文件"的提交后从工作区消失，导致运行时报文件缺失

## 根因

rebase 切换到目标提交并 apply 删除 diff 时按路径删除文件，不区分 tracked/untracked 的同名文件；untracked 文件无版本保护

## 解决

关键运行时文件避免做成 untracked + gitignored；程序提供 fallback（如 pyrite 本地 kb.yaml 缺失时读包内模板）；rebase 前先备份 untracked 文件

