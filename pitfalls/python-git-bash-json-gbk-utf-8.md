---
id: python-git-bash-json-gbk-utf-8
title: Python 输出中文在 Git Bash 乱码 / JSON 以 GBK 字节写出致 UTF-8 消费方解码失败
type: windows_pitfall
importance: 5
metadata:
  type: windows_pitfall
  symptom: 中文 Windows 下 Python 程序把中文写到 stdout：在 Git Bash（MinTTY，UTF-8 终端）里显示为乱码；更隐蔽的是 JSON 输出——写出的字节是 GBK，`--format=json | jq` 或任何按 UTF-8 解码的消费方直接 UnicodeDecodeError（utf-8 codec cant decode byte 0xd6）。重定向到文件同样如此，编码在写入时就已确定。注意与 emoji 崩溃型不同：这里不抛异常，是静默的编码错配。
  root_cause: Python 3.12 的 sys.stdout.encoding 跟随**控制台代码页**（chcp 显示 936），不跟随终端自身的编码。中文 Windows 默认 cp936/GBK，中文能编码成功（所以不抛异常）但写出的是 GBK 字节；Git Bash 的 MinTTY 按 UTF-8 显示，于是乱码。JSON 规范要求交换用 UTF-8，消费方按 UTF-8 解码 GBK 字节即失败。
  solution: CLI 启动时强制 stdout 为 UTF-8：sys.stdout.reconfigure(encoding=utf-8)。要先 getattr 判空再调——测试夹具会把 stdout 换成不支持重配的流。这样 Git Bash 直接正常、JSON 也合规；代价是 cmd.exe 代码页非 65001 时仍乱码，需先 chcp 65001。临时替代：PYTHONUTF8=1 或 PYTHONIOENCODING=utf-8，但只有改了代码才对使用方开箱有效。验证时必须剔除这两个环境变量去跑子进程，否则本机环境会掩盖缺陷。
  environment: &id001
    tool: Python 3.12 / Git Bash
  severity: medium
symptom: 中文 Windows 下 Python 程序把中文写到 stdout：在 Git Bash（MinTTY，UTF-8 终端）里显示为乱码；更隐蔽的是 JSON 输出——写出的字节是 GBK，`--format=json | jq` 或任何按 UTF-8 解码的消费方直接 UnicodeDecodeError（utf-8 codec cant decode byte 0xd6）。重定向到文件同样如此，编码在写入时就已确定。注意与 emoji 崩溃型不同：这里不抛异常，是静默的编码错配。
root_cause: Python 3.12 的 sys.stdout.encoding 跟随**控制台代码页**（chcp 显示 936），不跟随终端自身的编码。中文 Windows 默认 cp936/GBK，中文能编码成功（所以不抛异常）但写出的是 GBK 字节；Git Bash 的 MinTTY 按 UTF-8 显示，于是乱码。JSON 规范要求交换用 UTF-8，消费方按 UTF-8 解码 GBK 字节即失败。
solution: CLI 启动时强制 stdout 为 UTF-8：sys.stdout.reconfigure(encoding=utf-8)。要先 getattr 判空再调——测试夹具会把 stdout 换成不支持重配的流。这样 Git Bash 直接正常、JSON 也合规；代价是 cmd.exe 代码页非 65001 时仍乱码，需先 chcp 65001。临时替代：PYTHONUTF8=1 或 PYTHONIOENCODING=utf-8，但只有改了代码才对使用方开箱有效。验证时必须剔除这两个环境变量去跑子进程，否则本机环境会掩盖缺陷。
environment: *id001
severity: medium
---

## 症状

中文 Windows 下 Python 程序把中文写到 stdout：在 Git Bash（MinTTY，UTF-8 终端）里显示为乱码；更隐蔽的是 JSON 输出——写出的字节是 GBK，`--format=json | jq` 或任何按 UTF-8 解码的消费方直接 UnicodeDecodeError（utf-8 codec cant decode byte 0xd6）。重定向到文件同样如此，编码在写入时就已确定。注意与 emoji 崩溃型不同：这里不抛异常，是静默的编码错配。

## 根因

Python 3.12 的 sys.stdout.encoding 跟随**控制台代码页**（chcp 显示 936），不跟随终端自身的编码。中文 Windows 默认 cp936/GBK，中文能编码成功（所以不抛异常）但写出的是 GBK 字节；Git Bash 的 MinTTY 按 UTF-8 显示，于是乱码。JSON 规范要求交换用 UTF-8，消费方按 UTF-8 解码 GBK 字节即失败。

## 解决

CLI 启动时强制 stdout 为 UTF-8：sys.stdout.reconfigure(encoding=utf-8)。要先 getattr 判空再调——测试夹具会把 stdout 换成不支持重配的流。这样 Git Bash 直接正常、JSON 也合规；代价是 cmd.exe 代码页非 65001 时仍乱码，需先 chcp 65001。临时替代：PYTHONUTF8=1 或 PYTHONIOENCODING=utf-8，但只有改了代码才对使用方开箱有效。验证时必须剔除这两个环境变量去跑子进程，否则本机环境会掩盖缺陷。

