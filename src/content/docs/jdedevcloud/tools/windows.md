---
title: Windows
---

## Clean Windows

```shell
dism.exe /online /Cleanup-Image /AnalyzeComponentStore
dism.exe /online /Cleanup-Image /StartComponentCleanup
dism.exe /online /Cleanup-Image /StartComponentCleanup /ResetBase
dism.exe /online /Cleanup-Image /SPSuperseded
```
