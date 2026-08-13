---
id: vbs-utf-8-bom-wshshell
title: VBS 中文注释+UTF-8无BOM 致缺少对象 WshShell
type: windows_pitfall
importance: 5
metadata:
  type: windows_pitfall
  symptom: '运行 start_xhs_mcp.vbs 报 cscript (3,1) 运行时错误: 缺少对象: WshShell，服务拉不起来'
  root_cause: VBScript 按 ANSI 解析 .vbs；UTF-8 无 BOM 的中文注释多字节序列吞掉换行，Set WshShell = CreateObject(...) 行失效
  solution: VBS 文件保持纯 ASCII（英文注释）+ CRLF；需中文注释则用 GBK/ANSI 或 UTF-8 带 BOM 编码
  environment: &id001
    tool: wscript/cscript
  severity: medium
symptom: '运行 start_xhs_mcp.vbs 报 cscript (3,1) 运行时错误: 缺少对象: WshShell，服务拉不起来'
root_cause: VBScript 按 ANSI 解析 .vbs；UTF-8 无 BOM 的中文注释多字节序列吞掉换行，Set WshShell = CreateObject(...) 行失效
solution: VBS 文件保持纯 ASCII（英文注释）+ CRLF；需中文注释则用 GBK/ANSI 或 UTF-8 带 BOM 编码
environment: *id001
severity: medium
---

## 症状

运行 start_xhs_mcp.vbs 报 cscript (3,1) 运行时错误: 缺少对象: WshShell，服务拉不起来

## 根因

VBScript 按 ANSI 解析 .vbs；UTF-8 无 BOM 的中文注释多字节序列吞掉换行，Set WshShell = CreateObject(...) 行失效

## 解决

VBS 文件保持纯 ASCII（英文注释）+ CRLF；需中文注释则用 GBK/ANSI 或 UTF-8 带 BOM 编码

