<p align="center">
  <img src="images/logo.png" alt="DrewMark Logo">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/License-MIT-yellow?style=flat-square" alt="MIT License">
  <img src="https://img.shields.io/badge/Dependencies-0-success?style=flat-square" alt="Zero Dependencies">
  <img src="https://img.shields.io/badge/Vanilla-JS-F7DF1E?logo=javascript&logoColor=black&style=flat-square" alt="Vanilla JS">
  <img src="https://img.shields.io/badge/Size-%3C64KB-blue?style=flat-square" alt="Size">
</p>

# DrewMark JS Parser

A custom-built parser for [DrewMark](https://github.com/drewneon/drewmark), developed with **Vanilla JavaScript** — zero dependencies. Parses DrewMark-formatted plain text into standard HTML.

---

## Quick Start

```html
<script src="js/drewmark-parser.min.js"></script> <!-- Include the DrewMark JS Parser -->
<script>
  const html = drewmarkParser(content); // Parse DrewMark source text into HTML
</script>
```

Parsing can be done by just two lines of code.

---

## Complete Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
</head>
<body>
  <main>
    <div id="source" hidden>
# Heading
This is a paragraph with **bold** and %%italic%%.
    </div>
  </main>
  <!-- Include the DrewMark JS Parser -->
  <script src="js/drewmark-parser.min.js"></script>
  <script>
    // Output the parsed result into the <main> tag, replacing the original source text
    document.getElementsByTagName('main')[0].innerHTML =
      drewmarkParser(document.getElementById('source').innerText);
  </script>
</body>
</html>
```

---

## Parameters

```js
drewmarkParser(content, options?)
```

| Parameter | Type                 | Required | Description               |
| --------- | -------------------- | -------- | ------------------------- |
| `content` | `string \| string[]` | Yes      | DrewMark source text      |
| `options` | `object`             | No       | Configuration (see below) |

---

## Options

| Option              | Type       | Default | Description                                        |
| ------------------- | ---------- | ------- | -------------------------------------------------- |
| `enable_emoji`      | `boolean`  | `false` | Parse emoji syntax (`::smile::` 🡒 😄)             |
| `enable_style`      | `boolean`  | `false` | Parse style blocks and style attributes            |
| `enhance_codeblock` | `boolean`  | `true`  | Append language label + copy button to code blocks |
| `enhance_progress`  | `boolean`  | `true`  | Append numeric values after progress bars          |
| `disable_syntax`    | `string[]` | `[]`    | Disable specific syntaxes or syntax groups         |

---

## Disable Syntax

```js
// Disable entire groups
drewmarkParser(content, { disable_syntax: ['headings', 'lists', 'embeds'] });

// Disable specific items
drewmarkParser(content, { disable_syntax: ['highlight', 'audio', 'video', 'deflist'] });

// Mixed
drewmarkParser(content, { disable_syntax: ['text-decors', 'table', 'link'] });
```

See the [full documentation](docs/doc.md#45-disable-syntax) for all supported syntax names and aliases.

---

## Features

- Full DrewMark syntax support (v2.3 spec)
- Compatible with Markdown table and emoji syntax
- No third-party dependencies — pure Vanilla JS
- Lightweight minified build (less than 100kb)

---

## Documentation

See [`docs/doc.md`](docs/doc.md) for the full API reference.

中文文档: [docs/doc-cn.md](docs/doc-cn.md)

---

## Related Projects

[DrewMark](https://github.com/drewneon/drewmark) (syntax specification)
[DrewMark JS Editor](https://github.com/drewneon/drewmark-js-editor) (WYSIWYG editor)
[DrewMark JS Converter](https://github.com/drewneon/drewmark-js-converter) (convert between three formats)

---

## License

MIT
