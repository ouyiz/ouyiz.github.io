---
title: 问题解决-VScode代码复制到别处每行有间隔
categories:
  - 问题解决
tags:
  - bug
  - VSCode
mathjax: true
date: 2025-04-23 20:56:42
---
## 问题
将vscode的代码复制到有些地方，两行之间会空一行
```html
 <h2>Background Position 示例</h2>

    <div class="box position-center">center center</div>

    <div class="box position-left-top">left top</div>

    <div class="box position-right-bottom">right bottom</div>

    <div class="box position-right-top">right top</div>
```

## 解决：
在粘贴的时候使用`ctrl+shift+v`
会粘贴为纯文本，能防止目标软件加上段落格式和空行。
