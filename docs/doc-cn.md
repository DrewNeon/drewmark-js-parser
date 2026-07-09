<p align="center">
  <img src="../images/logo.png" alt="朱码标志">
</p>

# 朱码 JS 解析器文档

---

## 目录

1. [项目简介](#1-项目简介)
2. [快速开始](#2-快速开始)
3. [主功能](#3-主功能)
4. [可选参数](#4-可选参数)
5. [安全性](#5-安全性)

---

## 1. 项目简介

[朱码](/drewneon/drewmark) 是一款受 [Markdown](https://daringfireball.net/projects/markdown/) 和 [Showdown](https://github.com/showdownjs/showdown) 所启发的全功能型标记语言系统。

*朱码 JS 解析器*是为*朱码*量身定制的一款解析器，基于原生 JavaScript（Vanilla JS）开发，无需任何第三方库即可将符合*朱码*语法的文本解析输出为 HTML 格式。*朱码 JS 解析器*支持*朱码*的全部语法规则，请查看[朱码](/drewneon/drewmark)的文档了解详细的语法规则。

**额外说明**
1. 表情语法兼容前后单冒号，例如`:smile:` = `::smile::`。
2. 表格语法兼容 Markdown 格式，即分隔行使用连字符（ `|---|`）也可以正常解析。
3. 要想在表格的单元格内实现换行，只需在需要的位置使用 `\n` （被解析为 `<br>`）即可，
4. 如果在不支持父标签属性的标签语法中误写了父标签属性，将不被解析为任何标签的属性，输出的 HTML 代码中也不会出现误写的父标签属性原文。

---

## 2. 快速开始

只需两行 Javascript 代码即可实现对*朱码*内容的解析。

```html
  <script src="js/drewmark-parser.min.js"></script> <!-- 引用朱码 JS 解析器 -->
  <script>
    const html = drewmarkParser(content); // 将朱码原文解析为 HTML
  </script>
```

可根据实际情况获取原文并将解析结果输出到理想的位置，以下是一个完整的网页示例。

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
</head>
<body>
  <header></header>
  <main>
    <!-- 提供符合朱码语法规则的原文 -->
    <div id="source" hidden>
# 标题
这是一段包含**粗体**和%%斜体%%的文本。
  </div>
  </main>
  <footer></footer>
  <!-- 引用朱码 JS 解析器 -->
  <script src="js/drewmark-parser.min.js"></script>
  <script>
    // 将解析结果输出到 <main> 标签中覆盖原文
    document.getElementsByTagName('main')[0].innerHTML = 
      drewmarkParser(document.getElementById('source').innerText);
  </script>
</body>
</html>
```

上述示例的 `<main>` 标签中的内容会被替换为：`<h1>标题</h1><p>这是一段包含<strong>粗体</strong>和<em>斜体</em>的文本。</p>`。因为*朱码 JS 解析器*只负责将*朱码*原文解析为 HTML 格式，所以不包含任何样式 CSS 样式设计，浏览器会按默认效果呈现。不过，本项目也提供了一份样式样本—— `css/drewmark-parser.css` ，以`.drewmark-output`作为包裹解析的 HTML 代码的容器，可根据需求自行修改使用。

---

## 3. 主功能

**主函数**：`drewmarkParser()`
**必要参数**：`content`
**类型**：string / array
**使用方法**：
```Javascript
drewmarkParser(content);
```
**说明**：使用 `drewmarkParser()` 必须提供*朱码*原文，既可以是纯文本格式，也可以是按行划分的数组格式。

---

## 4. 可选参数

使用 `drewmarkParser()` 函数时，除了必须提供*朱码*原文以外，还可以通过附加参数来调整解析器的功能：`drewmarkParser(content, {options})`，多个参数之间用逗号分隔。

### 4.1 表情解析

*朱码*中可用的表情图标均来自 [Showdown](https://github.com/showdownjs/showdown)，特此鸣谢！

**参数名**：`enable_emoji`
**类型**：boolean
**默认值**：`false`
**使用方法**：
```Javascript
drewmarkParser(content, {enable_emoji: true});
```
**说明**：默认状态下，*朱码 JS 解析器*并**不**解析表情语法，相关原文会原样输出。如需显示表情，须在调用 `drewmarkParser()` 时使用此参数。

---

### 4.2 样式解析

**参数名**：`enable_style`
**类型**：boolean
**默认值**：`false`
**使用方法**：
```Javascript
drewmarkParser(content, {enable_style: true});
```
**说明**：默认状态下，*朱码 JS 解析器*并**不**解析样式块和样式属性的语法，符合这两种语法的原文会原样输出。如需解析样式块和样式属性，须在调用 `drewmarkParser()` 时使用此参数。

---

### 4.3 代码块增强

**参数名**：`enhance_codeblock`
**类型**：boolean
**默认值**：`true`
**使用方法**：
```Javascript
drewmarkParser(content, {enhance_codeblock: false});
```
**说明**：默认状态下，代码块中会附加一个 `<span>` 用于显示代码语言和一个 `<button>` 便于用户复制代码。如果不特别使用CSS赋予样式，仅靠浏览器默认渲染，增强部分的显示效果未必完美。如不需要这些增强部分，请使用此参数。不论是否使用了此参数，只要在代码块相应的语法位置填写了代码语言，输出的 `<pre>` 标签中都会附加 ` class="language-代码语言` 属性。
**示例**：
*朱码原文*：
````
```Javascript
    const var = 'value';
```
````
*默认状态下的解析结果*：
```html
<pre class="language-Javascript">
    <span>Javascript</span>
    <button aria-label="Copy" onclick="drewmarkCopyCode(this)">❐</button>
    <code>
    const var = 'value';
    </code>
</pre>
```
*使用参数去除增强部分的解析结果*：
```html
<pre class="language-Javascript">
    <code>
    const var = 'value';
    </code>
</pre>
```

---

### 4.4 进度条增强

**参数名**：`enhance_progress`
**类型**：boolean
**默认值**：`true`
**使用方法**：
```Javascript
drewmarkParser(content, {enhance_progress: false});
```
**说明**：主流浏览器在呈现 `<progress>` 标签时只显示进度条，并不显示其代表的具体数字（`value` 和 `max`）。为了更加全面地显示信息，默认状态下，`<progress>` 标签之后会紧跟着附加一个 `<span>` 标签，其中包含 `value`、`max` 和百分比。如不需要此增强部分，请使用此参数。
**示例**：
*朱码原文*：`这是一个进度条：((32//56))`
*默认状态下的解析结果*：
```html
<p>这是一个进度条：
  <progress value="32" max="56"></progress>
  <span>32/56 (57%)</span>
</p>
```
*使用参数去除增强部分的解析结果*：
```html
<p>这是一个进度条：
  <progress value="32" max="56"></progress>
</p>
```

---

### 4.5 禁用语法

**参数名**：`disable_syntax`
**类型**：array
**默认值**：`[]`
**使用方法**：
```Javascript
drewmarkParser(content, {disable_syntax: ['group_name', 'syntax_name', ...]});
```
**说明**：除上述提及的表情、代码块和代码属性以外，*朱码 JS 解析器*默认解析*朱码*其余全部语法。如不希望解析某个或某些语法，请使用此参数禁用目标语法。既可以使用组名禁用该组包含的全部语法，也可以使用语法名单独禁用，部分语法名支持不同写法。
**接受值**：（区分大小写）
├── `headings`                      （包含 1~6 级标题）
├── `text-decors`                   （文本修饰组）
│   ├── `bode`/`strong`             （粗体）
│   ├── `italic`/`em`               （斜体）
│   ├── `underline`/`ins`           （下划线）
│   ├── `strike`/`del`              （删除线）
│   ├── `highlight`/`mark`          （高亮）
│   ├── `sup`                       （上标）
│   ├── `sub`                       （下标）
│   ├── `small`                     （小字体）
│   └── `annotation`/`ruby`         （音标）
├── `codes`                         （代码组）
│   ├── `code-inline`               （行内代码）
│   └── `code-block`                （代码块）
├── `lists`                         （列表组）
│   ├── `lists-unordered`           （无序列表组）
│   │   ├── `ul-disc`               （实心圆列表）
│   │   ├── `ul-circle`             （空心圆列表）
│   │   └── `ul-square`             （方块列表）
│   ├── `lists-ordered`             （有序列表组）
│   │   ├── `ol-1`                  （阿拉伯数字列表）
│   │   ├── `ol-i`                  （小写罗马数字列表）
│   │   ├── `ol-I`                  （大写罗马数字列表）
│   │   ├── `ol-a`                  （小写拉丁字母列表）
│   │   └── `ol-A`                  （大写拉丁字母列表）
│   ├── `lists-task`                （任务列表组）
│   │   ├── `task-done`             （已完成任务列表）
│   │   └── `task-undone`           （未完成任务列表）
├── `table`                         （表格）
├── `embeds`                        （嵌入组）
│   ├── `link`/`a`                  （链接）
│   ├── `image`/`img`               （行内图片）
│   ├── `figure-block`/`figure`     （图片块）
│   ├── `audio`                     （音频）
│   └── `video`                     （视频）
├── `descs`                         （描述组）
│   ├── `deflist`/`dl`              （定义列表）
│   └── `details`                   （详情块）
├── `quotes`                        （引用组）
│   ├── `quote-inline`              （行内引用）
│   └── `quote-block`/`blockquote`  （引用块）
├── `progress`                      （进度条）
├── `hr`                            （水平分隔线）
├── `attrs`                         （属性组）
│   ├── `attr-align`                （水平对齐属性）
│   ├── `attr-class`                （类属性）
│   ├── `attr-dir`                  （文字方向属性）
│   ├── `attr-id`                   （id属性）
│   ├── `attr-lang`                 （语言属性）
│   ├── `attr-title`                （标题属性）
│   └── `attr-data`                 （data属性）

### 4.6 参数总览

| 参数名              | 类型      | 默认值  | 说明                                     |
| ------------------- | -------- | ------- | ---------------------------------------- |
| `enable_emoji`      | boolean  | `false` | 是否解析表情语法                          |
| `enable_style`      | boolean  | `false` | 是否解析样式块和样式属性语法               |
| `enhance_codeblock` | boolean  | `true`  | 是否在解析代码块时附加显示代码语言和复制按钮 |
| `enhance_progress`  | boolean  | `true`  | 是否在解析进度条时附加显示具体数值          |
| `disable_syntax`    | array    | `[]`    | 禁用任意语法，即不解析相应的语法            |

---

## 5. 安全性

*朱码 JS 解析器*在将纯文本解析为 HTML 的过程中，始终将安全性作为核心设计目标。解析器不直接执行任何用户输入，所有输出均经过转义或校验处理，确保最终生成的 HTML 不包含可执行的恶意代码，具体包括以下方面。

### 5.1 输入控制

**URL 协议白名单**

解析器对链接、图片、音频、视频等嵌入语法中的所有 URL 统一执行协议白名单过滤。仅放行以下协议：

- 标准网页协议：`http:` 或 `https:`
- 邮件协议：`mailto:`
- 电话协议：`tel:`
- 无协议的相对和绝对路径：（如 `path/to/page`、`//host/path`）

`javascript:`、`data:`、`vbscript:`、`file:` 等危险协议会被拦截，URL 被替换为 `#`。例如 `(点击){javascript:alert(1)}` 的解析结果为 `<a href="#">点击</a>`，不会触发脚本执行。

**引用来源校验**

引用语法的来源标注 `{<=URL}` 通过 `isValidUrl` 函数校验，仅接受合法的绝对 URL（含协议和主机名）或符合域名格式的相对路径，防止注入恶意引用来源。

### 5.2 属性校验

解析器通过独立的校验模块对所有全局属性和父标签属性的值进行严格校验。校验模块在解析器启动时惰性加载，对每种属性类型执行专属的格式检查：

| 全局属性  | 校验规则                                                                            |
| -------- | ---------------------------------------------------------------------------------- |
| `id`     | 非空、无空白、以字母开头、仅含字母、数字、连字符、下划线                                   |
| `class`  | 不含逗号、无控制字符、无空 token、首尾无多余空格                                       |
| `style`  | 不含 `{`、`}`、`@`（防 CSS 注入）；每条声明必须含冒号；属性名不含空白；引号和括号必须配对 |
| `data-*` | 键名以小写字母开头、仅含字母、数字、下划线、点和连字符；值不含控制字符                    |
| `title`  | 不含 HTML 标签（`<...>`）                                                           |
| `align`  | 仅接受 `left`/`center`/`right`（或缩写 `l`/`c`/`r`）                                 |
| `dir`    | 仅接受 `ltr`/`rtl`/`auto`                                                           |
| `lang`   | 符合 BCP 47 格式（`主标签-子标签`，每段 1-8 个字母数字）                               |

校验不通过的属性值会被静默丢弃，不会出现在输出的 HTML 中。未知属性名同样被拒绝。

**`style` 属性的默认关闭**

`style` 属性允许直接控制元素的视觉表现，因此即使通过校验，合法的 CSS 仍可用于 UI 劫持（定位覆盖、点击劫持、内容隐藏）或资源加载探测（`url()` 加载外部资源）。因此，`style` 属性默认关闭，需通过 `enable_style: true` 显式开启。开发者在开启时应确保应用场景可信（如仅限内部文档系统，而非面向公众的输入）。

**校验模块可用性检查**

由于`style` 和 `data-*` 属性值格式较为复杂，解析器依赖单独的校验模块对其进行格式检查。在校验模块未加载时（`window.DrewMarkValidate` 不可用）会直接拒绝这两类属性，而非放行。这确保了即使脚本加载顺序异常导致校验模块缺失，这两类属性也不会以未校验的状态输出。

### 5.3 输出转义

**HTML 实体转义**

解析器在渲染 HTML 时，会将**用户输入的文本内容部分**（包括文本节点、代码内容、表格标题等）通过 `escHtml` 函数转义，将 `&`、`<`、`>`、`"`、`'` 五个字符转换为对应的 HTML 实体（`&amp;`、`&lt;`、`&gt;`、`&quot;`、`&#39;`）。这防止了用户文本中的 HTML 标签被浏览器解释执行。例如用户输入 `**<script>**`，解析器识别 `**` 标记后提取文本节点 `<script>`，转义为 `&lt;script&gt;`，再由解析器拼接上自己生成的 `<strong>` 标签，最终输出 `<strong>&lt;script&gt;</strong>`。

**属性值转义**

所有输出到 HTML 属性值中的内容（如 `title`、`class`、`style` 等）经过 `escAttr` 函数转义，将 `&`、`<`、`>`、`"`、`'` 转换为 HTML 实体，防止属性值注入（如通过 `"` 闭合属性引号注入新属性）。

**样式块内容转义**

样式块的内容输出到 `<style>` 标签内时，会对 `</style` 序列进行转义（替换为 `<\/style`）。HTML 规范中 `<style>` 是 raw text 元素，唯有 `</style` 能结束它；此转义防止用户通过 `</style>...<script>` 提前闭合样式块并注入脚本。

**代码块内容转义**

代码块的内容先经过 `escHtml` 转义后再输出到 `<pre><code></code></pre>` 中，确保代码中的 HTML 标签（如 `<div>`）以文本形式显示，不会被浏览器解释执行。

### 5.4 异常降级

解析器在主入口处包裹了 `try-catch`。若解析过程中因任何原因（异常输入、正则回溯错误、规则内部异常等）抛出错误，解析器不会崩溃或输出空白，而是降级返回，即将原始文本经过 `escHtml` 转义后包裹在 `<pre>` 标签中安全输出。这保证了即使遇到极端边界情况，用户也能看到转义后的原始文本，而非不可用的空白页面。

### 5.5 语法禁用

通过 `disable_syntax` 参数，开发者可精确禁用特定语法组或语法项（如 `style`、`code-block`、`list` 等）。禁用采用精确匹配，避免 `'code'` 误禁 `'codes'`。`style` 属性和样式块为默认禁用，即便开发者忽略了这些高危因素也没有安全风险。

---

*版本: v2.8*