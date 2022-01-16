---
type: Daily
created: ${CURRENT_YEAR}-${CURRENT_MONTH}-${CURRENT_DATE}
foam_template: # 日记使用自定义模版会导致文件名不能按照预期设置，暂时不覆盖默认模版
  filepath: journal/$FOAM_TITLE.md
  name: 日记模版
  description: 可在任意位置创建，需要输入笔记类型、标签以及标题
---

# ${3:$TM_FILENAME_BASE}
