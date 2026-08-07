---
id: github-auto-merge-pitfalls-kb-yaml
title: GitHub auto-merge 检查只接受 pitfalls 下新增，改 kb.yaml 或删除会被拒
type: windows_pitfall
importance: 5
metadata:
  type: windows_pitfall
  symptom: 数据仓库 PR 改到 pitfalls/ 之外（如 kb.yaml、README）或含删除/修改时，verify-and-merge 检查失败，PR 不合入
  root_cause: auto-merge workflow 的检查逻辑：只允许新增 pitfalls/ 下的文件（diff-filter 排除 D/M），schema 变更属于控制层低频操作，不应走自动合入
  solution: 改 schema/kb.yaml 是代码仓库的控制层变更，手动 merge；数据 PR 只新增 pitfalls/*.md
  environment: &id001
    tool: github
  severity: low
symptom: 数据仓库 PR 改到 pitfalls/ 之外（如 kb.yaml、README）或含删除/修改时，verify-and-merge 检查失败，PR 不合入
root_cause: auto-merge workflow 的检查逻辑：只允许新增 pitfalls/ 下的文件（diff-filter 排除 D/M），schema 变更属于控制层低频操作，不应走自动合入
solution: 改 schema/kb.yaml 是代码仓库的控制层变更，手动 merge；数据 PR 只新增 pitfalls/*.md
environment: *id001
severity: low
---

## 症状

数据仓库 PR 改到 pitfalls/ 之外（如 kb.yaml、README）或含删除/修改时，verify-and-merge 检查失败，PR 不合入

## 根因

auto-merge workflow 的检查逻辑：只允许新增 pitfalls/ 下的文件（diff-filter 排除 D/M），schema 变更属于控制层低频操作，不应走自动合入

## 解决

改 schema/kb.yaml 是代码仓库的控制层变更，手动 merge；数据 PR 只新增 pitfalls/*.md

