---
id: python-print-gbk
title: Python 导入期 print 含 ✓/⚠️ 在 GBK 控制台致应用启动失败（不只是日志刷屏）
type: windows_pitfall
importance: 5
metadata:
  type: windows_pitfall
  symptom: 'FastAPI/uvicorn 启动直接崩：UnicodeEncodeError: gbk codec cannot encode character ✓，堆栈落在模块级 print（app/main.py 调用的启动冒烟函数）。同一份代码带 PYTHONIOENCODING=utf-8 跑就正常 —— 测试时设了该变量会掩盖缺陷。'
  root_cause: 中文 Windows stdout 默认 cp936/GBK，编不出 ✓ ⚠️ 等 GBK 外符号。与既有条目 python-emoji-gbk-unicodeencodeerror 同根因，但那条是 logging 输出（非致命、刷屏），本条是**模块导入期的 print** —— 异常发生在 app 对象构造前，uvicorn 直接退出。
  solution: 启动期输出一律只用 ASCII 标记（[ok]/[!!]），不用 ✓ ⚠️ emoji。验证时必须 env -u PYTHONIOENCODING -u PYTHONUTF8 跑，否则本机变量掩盖缺陷。中文本身在 GBK 下可编码，不受影响。
  environment: &id001
    tool: python / uvicorn / Windows GBK
  severity: medium
symptom: 'FastAPI/uvicorn 启动直接崩：UnicodeEncodeError: gbk codec cannot encode character ✓，堆栈落在模块级 print（app/main.py 调用的启动冒烟函数）。同一份代码带 PYTHONIOENCODING=utf-8 跑就正常 —— 测试时设了该变量会掩盖缺陷。'
root_cause: 中文 Windows stdout 默认 cp936/GBK，编不出 ✓ ⚠️ 等 GBK 外符号。与既有条目 python-emoji-gbk-unicodeencodeerror 同根因，但那条是 logging 输出（非致命、刷屏），本条是**模块导入期的 print** —— 异常发生在 app 对象构造前，uvicorn 直接退出。
solution: 启动期输出一律只用 ASCII 标记（[ok]/[!!]），不用 ✓ ⚠️ emoji。验证时必须 env -u PYTHONIOENCODING -u PYTHONUTF8 跑，否则本机变量掩盖缺陷。中文本身在 GBK 下可编码，不受影响。
environment: *id001
severity: medium
---

## 症状

FastAPI/uvicorn 启动直接崩：UnicodeEncodeError: gbk codec cannot encode character ✓，堆栈落在模块级 print（app/main.py 调用的启动冒烟函数）。同一份代码带 PYTHONIOENCODING=utf-8 跑就正常 —— 测试时设了该变量会掩盖缺陷。

## 根因

中文 Windows stdout 默认 cp936/GBK，编不出 ✓ ⚠️ 等 GBK 外符号。与既有条目 python-emoji-gbk-unicodeencodeerror 同根因，但那条是 logging 输出（非致命、刷屏），本条是**模块导入期的 print** —— 异常发生在 app 对象构造前，uvicorn 直接退出。

## 解决

启动期输出一律只用 ASCII 标记（[ok]/[!!]），不用 ✓ ⚠️ emoji。验证时必须 env -u PYTHONIOENCODING -u PYTHONUTF8 跑，否则本机变量掩盖缺陷。中文本身在 GBK 下可编码，不受影响。

