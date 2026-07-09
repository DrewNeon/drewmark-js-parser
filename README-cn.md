<p align="center">
  <img src="images/logo.png" alt="朱码标志">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/License-MIT-yellow?style=flat-square" alt="MIT 许可证">
  <img src="https://img.shields.io/badge/Dependencies-0-success?style=flat-square" alt="零依赖">
  <img src="https://img.shields.io/badge/Vanilla-JS-F7DF1E?logo=javascript&logoColor=black&style=flat-square" alt="Vanilla JS">
  <img src="https://img.shields.io/badge/Size-%3C64KB-blue?style=flat-square" alt="体积">
</p>

# 朱码 JS 解析器

为[朱码](../../../../drewneon/drewmark)量身定制的解析器，基于**原生 JavaScript**（Vanilla JS）开发，零依赖。将符合朱码语法的文本解析输出为标准 HTML。

---

## 快速开始

```html
<script src="js/drewmark-parser.min.js"></script> <!-- 引用朱码 JS 解析器 -->
<script>
  const html = drewmarkParser(content); // 将朱码原文解析为 HTML
</script>
```

仅两行代码即可实现解析功能。

---

## 完整示例

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
</head>
<body>
  <main>
    <div id="source" hidden>
# 标题
这是一段包含**粗体**和%%斜体%%的文本。
    </div>
  </main>
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

---

## 参数

```js
drewmarkParser(content, options?)
```

| 参数      | 类型                  | 必填 | 说明            |
| --------- | -------------------- | ---- | -------------- |
| `content` | `string \| string[]` | 是   | 朱码原文        |
| `options` | `object`             | 否   | 配置项（见下表） |

---

## 可选参数（options）

| 参数                 | 类型       | 默认值  | 说明                                 |
| ------------------- | ---------- | ------- | ----------------------------------- |
| `enable_emoji`      | `boolean`  | `false` | 是否解析表情语法（`::smile::` 🡒 😄） |
| `enable_style`      | `boolean`  | `false` | 是否解析样式块和样式属性               |
| `enhance_codeblock` | `boolean`  | `true`  | 是否为代码块附加语言标签和复制按钮     |
| `enhance_progress`  | `boolean`  | `true`  | 是否在进度条后附加数值显示             |
| `disable_syntax`    | `string[]` | `[]`    | 禁用指定的语法或语法组                |

---

## 禁用语法示例

```js
// 禁用整个语法组
drewmarkParser(content, { disable_syntax: ['headings', 'lists', 'embeds'] });

// 禁用具体语法项
drewmarkParser(content, { disable_syntax: ['highlight', 'audio', 'video', 'deflist'] });

// 混合使用
drewmarkParser(content, { disable_syntax: ['text-decors', 'table', 'link'] });
```

支持的全部语法名及别名请参见[完整文档](docs/doc-cn.md#45-禁用语法)。

---

## 特性

- 完整支持朱码 v2.3 全部语法
- 兼容 Markdown 表格和表情语法
- 零第三方依赖——纯原生 JavaScript
- 精简压缩构建（不足 100kb）

---

## 文档

完整文档请参阅 [`docs/doc-cn.md`](docs/doc-cn.md)。

English docs: [docs/doc.md](docs/doc.md)

---

## 相关项目

* [朱码](../../../../drewneon/drewmark)（语法规范）
* [朱码 JS 编辑器](../../../../drewneon/drewmark-js-editor)（所见即所得编辑器）
* [朱码 JS 转换器](../../../../drewneon/drewmark-js-converter)（三种格式互转）

---

## 许可证

MIT
