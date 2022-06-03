---
type: Linux
tags:
  - gpg
  - linux
  - QoK-B
---

# gpg

## 移除 `gpg` key 的密码

1. 查询需要修改的 `gpg` key：`gpg --list-keys`
2. 进入编辑模式：`gpg --edit-key <需要修改的 key>`
3. 此时进入 `gpg>` 状态等待输入编辑指令，输入 `passwd` 指定编辑密码
4. 弹窗中输入一次旧密码，根据提示将新密码设为空
