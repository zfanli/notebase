---
type: Foam
# tags: Foam
created: 2022-01-31
---

# Daily Notes

通过 `Foam: Open Daily Note` 可以快速访问日记笔记。

- 如果今天的笔记不存在，这个命令会创建一个今天的笔记
- 如果存在，这个命令会打开今天的笔记
- 快捷键 `options` + `d` 可以快速打开今天的笔记
  - 可以通过 VSCode 快捷键配置修改

## 配置

默认日记文件的名称为 `yyyy-mm-dd.md`。这个配置可以从 `settings.json` 中修改。

```json
  "foam.openDailyNote.directory": "journal", // a relative directory path will get appended to the workspace root. An absolute directory path will be used unmodified.
  "foam.openDailyNote.filenameFormat": "'daily-note'-yyyy-mm-dd",
  "foam.openDailyNote.fileExtension": "mdx",
  "foam.openDailyNote.titleFormat": "'Journal Entry, ' dddd, mmmm d",
```

这段配置会创建日记类型的笔记，文件名为 `journal/note-2020-07-25.mdx`，标题为 `Journal Entry, Sunday, July 25`。

### Daily Note 模版

可以通过特殊名词的模版 `.foam/templates/daily-note.md` 自定义模版内容。详细参照 [[foam-note-template]]。

### 启动时创建日记

对于 VSCode 从来不关闭的人来说，这个设置感觉排不上用场。

```json
{
  // ...Other configurations
  "foam.openDailyNote.onStartup": true
}
```
