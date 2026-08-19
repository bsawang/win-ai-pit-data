---
id: comfyui-custom-nodes
title: ComfyUI custom_nodes 包内绝对导入同包模块失败
type: windows_pitfall
importance: 5
metadata:
  type: windows_pitfall
  symptom: '节点从 ComfyUI 消失，后端日志报 ModuleNotFoundError: No module named xxx（模块文件明明存在）'
  root_cause: custom_nodes 的插件是 Python 包，包内模块间用绝对导入（import llm_usage）时，Python 不把包目录加进 sys.path，找不到同包模块；必须用包内相对导入（from . import llm_usage）
  solution: 包内模块互相引用一律用相对导入：from . import llm_usage（而非 import llm_usage）；新增共享模块（如 llm_usage.py）时检查所有引用点
  environment: &id001
    tool: ComfyUI custom_nodes
  severity: medium
symptom: '节点从 ComfyUI 消失，后端日志报 ModuleNotFoundError: No module named xxx（模块文件明明存在）'
root_cause: custom_nodes 的插件是 Python 包，包内模块间用绝对导入（import llm_usage）时，Python 不把包目录加进 sys.path，找不到同包模块；必须用包内相对导入（from . import llm_usage）
solution: 包内模块互相引用一律用相对导入：from . import llm_usage（而非 import llm_usage）；新增共享模块（如 llm_usage.py）时检查所有引用点
environment: *id001
severity: medium
---

## 症状

节点从 ComfyUI 消失，后端日志报 ModuleNotFoundError: No module named xxx（模块文件明明存在）

## 根因

custom_nodes 的插件是 Python 包，包内模块间用绝对导入（import llm_usage）时，Python 不把包目录加进 sys.path，找不到同包模块；必须用包内相对导入（from . import llm_usage）

## 解决

包内模块互相引用一律用相对导入：from . import llm_usage（而非 import llm_usage）；新增共享模块（如 llm_usage.py）时检查所有引用点

