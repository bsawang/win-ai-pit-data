---
id: pyrite-yaml-frontmatter
title: pyrite 索引解析失败：YAML frontmatter 值含英文冒号空格
type: windows_pitfall
importance: 5
metadata:
  type: windows_pitfall
  symptom: '坑文件 frontmatter 的 title/symptom 值里含 LookupError: unknown encoding: cp936 这类英文冒号空格时，windows-pitfalls index 报 mapping values are not allowed here（line 行号），该条坑被跳过不索引'
  root_cause: 'YAML 严格解析把值里的 : （英文冒号+空格）当作嵌套 mapping 分隔符，导致 frontmatter 解析失败'
  solution: frontmatter 值里的英文冒号空格改成全角 ：；或给值加引号包裹
  environment: &id001
    tool: python
  severity: medium
symptom: '坑文件 frontmatter 的 title/symptom 值里含 LookupError: unknown encoding: cp936 这类英文冒号空格时，windows-pitfalls index 报 mapping values are not allowed here（line 行号），该条坑被跳过不索引'
root_cause: 'YAML 严格解析把值里的 : （英文冒号+空格）当作嵌套 mapping 分隔符，导致 frontmatter 解析失败'
solution: frontmatter 值里的英文冒号空格改成全角 ：；或给值加引号包裹
environment: *id001
severity: medium
---

## 症状

坑文件 frontmatter 的 title/symptom 值里含 LookupError: unknown encoding: cp936 这类英文冒号空格时，windows-pitfalls index 报 mapping values are not allowed here（line 行号），该条坑被跳过不索引

## 根因

YAML 严格解析把值里的 : （英文冒号+空格）当作嵌套 mapping 分隔符，导致 frontmatter 解析失败

## 解决

frontmatter 值里的英文冒号空格改成全角 ：；或给值加引号包裹

