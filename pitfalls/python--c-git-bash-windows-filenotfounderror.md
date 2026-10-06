---
id: python--c-git-bash-windows-filenotfounderror
title: python -c 里写中文路径在 Git Bash/Windows 下变乱码报 FileNotFoundError
type: windows_pitfall
importance: 5
metadata:
  type: windows_pitfall
  symptom: 'bash 的 cp/cmp 用中文文件名正常，但 python -c "...open(/h/.../nsfw-专用镜头.json)" 报 FileNotFoundError: .../nsfw-ר�þ�ͷ.json（路径被按 GBK 解码成乱码）'
  root_cause: python -c 的字符串由 Python 按系统 ANSI 代码页(GBK)解码 argv/源码，而 Git Bash 传的是 UTF-8 字节，中文路径因此被错误解码
  solution: 三种任一：① 用 heredoc 把脚本喂给 python（python - <<PY ... PY，PYTHONUTF8=1）；② 设 PYTHONUTF8=1 PYTHONIOENCODING=utf-8；③ 中文路径的文件操作交给 bash 的 cp/cmp/ls，python 只处理已验证存在的路径或经 glob
  environment: &id001
    tool: python/Git Bash
  severity: medium
symptom: 'bash 的 cp/cmp 用中文文件名正常，但 python -c "...open(/h/.../nsfw-专用镜头.json)" 报 FileNotFoundError: .../nsfw-ר�þ�ͷ.json（路径被按 GBK 解码成乱码）'
root_cause: python -c 的字符串由 Python 按系统 ANSI 代码页(GBK)解码 argv/源码，而 Git Bash 传的是 UTF-8 字节，中文路径因此被错误解码
solution: 三种任一：① 用 heredoc 把脚本喂给 python（python - <<PY ... PY，PYTHONUTF8=1）；② 设 PYTHONUTF8=1 PYTHONIOENCODING=utf-8；③ 中文路径的文件操作交给 bash 的 cp/cmp/ls，python 只处理已验证存在的路径或经 glob
environment: *id001
severity: medium
---

## 症状

bash 的 cp/cmp 用中文文件名正常，但 python -c "...open(/h/.../nsfw-专用镜头.json)" 报 FileNotFoundError: .../nsfw-ר�þ�ͷ.json（路径被按 GBK 解码成乱码）

## 根因

python -c 的字符串由 Python 按系统 ANSI 代码页(GBK)解码 argv/源码，而 Git Bash 传的是 UTF-8 字节，中文路径因此被错误解码

## 解决

三种任一：① 用 heredoc 把脚本喂给 python（python - <<PY ... PY，PYTHONUTF8=1）；② 设 PYTHONUTF8=1 PYTHONIOENCODING=utf-8；③ 中文路径的文件操作交给 bash 的 cp/cmp/ls，python 只处理已验证存在的路径或经 glob

