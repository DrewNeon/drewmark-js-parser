<p align="center">
  <img src="../images/logo.png" alt="DrewMark Logo">
</p>

# DrewMark JS Parser Documentation

---

## Table of Contents

1. [Project Introduction](#1-project-introduction)
2. [Quick Start](#2-quick-start)
3. [Main Features](#3-main-features)
4. [Optional Parameters](#4-optional-parameters)
5. [Security](#5-security)

---

## 1. Project Introduction

[DrewMark](../../../../../drewneon/drewmark) is a full-featured markup language system inspired by [Markdown](https://daringfireball.net/projects/markdown/) and [Showdown](https://github.com/showdownjs/showdown).

The DrewMark JS Parser is a custom-built parser tailored specifically for DrewMark. Developed with Vanilla JavaScript, it requires no third-party libraries to parse plain text conforming to DrewMark syntax and output it in HTML format. The DrewMark JS Parser supports the complete set of DrewMark syntax rules. Please refer to [DrewMark document](../../../../../drewneon/drewmark/docs/doc.md) for detailed syntax specifications.

**Additional Notes**
1. Emoji syntax is compatible with both single and double colons as delimiters, e.g. `:smile:` is equivalent to `::smile::`.
2. Table syntax is compatible with Markdown format, meaning separator rows using hyphens (e.g., `|---|`) will also be parsed correctly.
3. To insert a line break within a table cell, simply use `\n` at the desired position, which will be parsed as `<br>`.
4. If parent tag attributes are mistakenly written within tag syntaxes that do not support them, they will be completely discarded, i.e. they will not be parsed as an attribute of any tag, nor will they appear as raw text content in the output HTML code.

---

## 2. Quick Start

Parsing DrewMark content can be achieved with just two lines of JavaScript code.

```html
  <script src="js/drewmark-parser.min.js"></script> <!-- Include the DrewMark JS Parser -->
  <script>
    const html = drewmarkParser(content); // Parse DrewMark source text into HTML
  </script>
```

You can retrieve the source text according to your specific needs and output the parsed result to the desired location. Below is a complete yet concise webpage example.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
</head>
<body>
  <header></header>
  <main>
    <!-- Provide source text conforming to DrewMark syntax rules -->
    <div id="source" hidden>
# Heading
This is a paragraph containing **bold** and %%italic%% formatting.
  </div>
  </main>
  <footer></footer>
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

In the above example, the content inside the `<main>` tag will be replaced with: `<h1>Heading</h1><p>This is a paragraph containing <strong>bold</strong> and <em>italic</em> formatting.</p>`. Since the DrewMark JS Parser is solely responsible for converting DrewMark source text into HTML format, it does not include any CSS styling. Consequently, the browser will render the content using its default styles. However, a sample css is also provides as `css/drewmark-parser.css`, in which `.drewmark-output` represents the wrapper element of the parsed HTML code, please make necessary changes according to specific needs before use.

---

## 3. Main Function

**Function**: `drewmarkParser()`
**Required Parameter**: `content`
**Type**: string / array
**Usage**:
```Javascript
drewmarkParser(content);
```
**Description**: When using `drewmarkParser()`, you must provide the DrewMark source text, which can be either in plain text format or as an array split by lines.

---

## 4. Optional Parameters

When using the `drewmarkParser()` function, in addition to the required DrewMark source text, you can also adjust the parser's functionality via additional parameters: `drewmarkParser(content, {options})`. Multiple parameters should be separated by commas.

### 4.1 Emoji Parsing

The emoji icons available in DrewMark are sourced from [Showdown](https://github.com/showdownjs/showdown). Special thanks to the Showdown team!

**Parameter Key**: `enable_emoji`
**Type**: boolean
**Default Value**: `false`
**Usage**:
```Javascript
drewmarkParser(content, {enable_emoji: true});
```
**Description**: By default, the DrewMark JS Parser does **not** parse emoji syntax; related source text will be output as-is. To display emojis, you must use this parameter when calling `drewmarkParser()`.

---

### 4.2 Style Parsing

**Parameter Key**: `enable_style`
**Type**: boolean
**Default Value**: `false`
**Usage**:
```Javascript
drewmarkParser(content, {enable_style: true});
```
**Description**: By default, the DrewMark JS Parser does **not** parse style blocks nor style attribute syntax. Source text conforming to these two syntax types will be output as-is. To parse style blocks and style attributes, you must use this parameter when calling `drewmarkParser()`.

---

### 4.3 Code Block Enhancement

**Parameter Key**: `enhance_codeblock`
**Type**: boolean
**Default Value**: `true`
**Usage**:
```Javascript
drewmarkParser(content, {enhance_codeblock: false});
```
**Description**: By default, a `<span>` element indicating the code language and a `<button>` for easily copying the code is appended to code blocks. When not speicifically styled via CSS, the visual presentation of these two elements may not be perfect. If you do not need these enhanced elements, please use this parameter. Regardless of whether this parameter is used, as long as a code language is specified in the appropriate syntax position of the code block, the output `<pre>` tag will always include the ` class="language-[code-language]"` attribute.
**Example**:
*DrewMark Source Text*:
````
```Javascript
    const var = 'value';
```
````
*Parsed Result (Default State)*:
```html
<pre class="language-Javascript">
    <span>Javascript</span>
    <button aria-label="Copy" onclick="drewmarkCopyCode(this)">❐</button>
    <code>
    const var = 'value';
    </code>
</pre>
```
*Parsed Result (when code enhancement **disabled**)*:
```html
<pre class="language-Javascript">
    <code>
    const var = 'value';
    </code>
</pre>
```

---

### 4.4 Progress Bar Enhancement

**Parameter Key**: `enhance_progress`
**Type**: boolean
**Default Value**: `true`
**Usage**:
```Javascript
drewmarkParser(content, {enhance_progress: false});
```
**Description**: Most mainstream browsers display merely a progress bar when rendering a `<progress>` tag, without showing the specific numerical values (`value` and `max`). To provide more comprehensive information, by default, a `<span>` tag containing the `value`, `max`, and percentage is appended immediately after the `<progress>` tag. If you do not need this enhancement, please use this parameter.
**Example**:
*DrewMark Source Text*: `This is a progress bar: ((32//56))`
*Parsed Result (Default State)*:
```html
<p>This is a progress bar:
  <progress value="32" max="56"></progress>
  <span>32/56 (57%)</span>
</p>
```
*Parsed Result (when progress enhancement **disabled**)*:
```html
<p>This is a progress bar:
  <progress value="32" max="56"></progress>
</p>
```

---

### 4.5 Disable Syntax

**Parameter Name**: `disable_syntax`
**Type**: array
**Default Value**: `[]`
**Usage**:
```Javascript
drewmarkParser(content, {disable_syntax: ['group_name', 'syntax_name', ...]});
```
**Description**: Except for the emojis, code blocks, and style attributes mentioned above, the DrewMark JS Parser parses all the other DrewMark syntax by default. If you wish to prevent any specific syntax or syntax groups from being parsed, use this parameter to disable them. You can disable an entire group by its group name, or individual syntax items by their specific names. Some syntax names support alternative aliases.
**Accepted Values**: (Case-sensitive)
├── `headings`                      (Includes heading levels 1–6)
├── `text-decors`                   (Text decoration group)
│   ├── `bode`/`strong`             (Bold)
│   ├── `italic`/`em`               (Italic)
│   ├── `underline`/`ins`           (Underline)
│   ├── `strike`/`del`              (Strikethrough)
│   ├── `highlight`/`mark`          (Highlight)
│   ├── `sup`                       (Superscript)
│   ├── `sub`                       (Subscript)
│   ├── `small`                     (Small text)
│   └── `annotation`/`ruby`         (Ruby annotation / Phonetic guide)
├── `codes`                         (Code group)
│   ├── `code-inline`               (Inline code)
│   └── `code-block`                (Code block)
├── `lists`                         (List group)
│   ├── `lists-unordered`           (Unordered list group)
│   │   ├── `ul-disc`               (Solid disc list)
│   │   ├── `ul-circle`             (Hollow circle list)
│   │   └── `ul-square`             (Square list)
│   ├── `lists-ordered`             (Ordered list group)
│   │   ├── `ol-1`                  (Arabic numerals list)
│   │   ├── `ol-i`                  (Lowercase Roman numerals list)
│   │   ├── `ol-I`                  (Uppercase Roman numerals list)
│   │   ├── `ol-a`                  (Lowercase Latin letters list)
│   │   └── `ol-A`                  (Uppercase Latin letters list)
│   ├── `lists-task`                (Task list group)
│   │   ├── `task-done`             (Completed task list)
│   │   └── `task-undone`           (Incomplete task list)
├── `table`                         (Table)
├── `embeds`                        (Embed group)
│   ├── `link`/`a`                  (Link)
│   ├── `image`/`img`               (Inline image)
│   ├── `figure-block`/`figure`     (Figure block)
│   ├── `audio`                     (Audio)
│   └── `video`                     (Video)
├── `descs`                         (Description group)
│   ├── `deflist`/`dl`              (Definition list)
│   └── `details`                   (Details block)
├── `quotes`                        (Quote group)
│   ├── `quote-inline`              (Inline quote)
│   └── `quote-block`/`blockquote`  (Blockquote)
├── `progress`                      (Progress bar)
├── `hr`                            (Horizontal rule)
├── `attrs`                         (Attribute group)
│   ├── `attr-align`                (Horizontal alignment attribute)
│   ├── `attr-class`                (Class attribute)
│   ├── `attr-dir`                  (Text direction attribute)
│   ├── `attr-id`                   (ID attribute)
│   ├── `attr-lang`                 (Language attribute)
│   ├── `attr-title`                (Title attribute)
│   └── `attr-data`                 (Data attribute)

### 4.6 Parameter Overview

| Parameter Name      | Type     | Default Value | Description                                                            |
| ------------------- | -------- | ------------- | ---------------------------------------------------------------------- |
| `enable_emoji`      | boolean  | `false`       | Whether to parse emoji syntax                                          |
| `enable_style`      | boolean  | `false`       | Whether to parse style block syntax and style attribute                |
| `enhance_codeblock` | boolean  | `true`        | Whether to append code language display and copy button in code blocks |
| `enhance_progress`  | boolean  | `true`        | Whether to append detailed numerical values when parsing progress bars |
| `disable_syntax`    | array    | `[]`          | Disables specified syntax, preventing it from being parsed             |

---

## 5. Security

*DrewMark JS Parser* consistently prioritizes security as a core design objective when parsing plain text into HTML. The parser never directly executes any user input; all output is either escaped or validated to ensure the final generated HTML contains no executable malicious code. Specific measures include the following aspects.

### 5.1 Input Control

**URL Protocol Whitelist**

The parser applies a unified protocol whitelist filter to all URLs within syntaxes such as links, images, audio, video, etc. Only the following protocols are permitted:

- Standard web protocols: `http:` or `https:`
- Email protocol: `mailto:`
- Telephone protocol: `tel:`
- Protocol-less relative and absolute paths (e.g., `path/to/page`, `//host/path`)

Dangerous protocols such as `javascript:`, `data:`, `vbscript:`, and `file:` are blocked, and the URL is replaced with `#`. For example, `(Click){javascript:alert(1)}` is parsed as `<a href="#">Click</a>`, preventing script execution.

**Citation Source Validation**

The source annotation `{<=URL}` in quote syntax is validated via the `isValidUrl` function. It accepts only valid absolute URLs (including protocol and hostname) or relative paths conforming to domain name formats, preventing the injection of malicious citation sources.

### 5.2 Attribute Validation

The parser uses an independent validation module to strictly validate values for all global attributes and parent tag attributes. This module is lazily loaded upon parser initialization and performs specific format checks for each attribute type:

| Global Attribute | Validation Rules                                                                                  |
| ---------------- | ------------------------------------------------------------------------------------------------- |
| `id`             | Non-empty, no whitespace, starts with a letter, contains only letters/numbers/hyphens/underscores |
| `class`          | No commas, no control characters, no empty tokens, no leading/trailing whitespace                 |
| `style`          | No `{`, `}`, or `@` (to prevent CSS injection); every declaration must contain a colon; property names must not contain whitespace; quotes and parentheses must be paired |
| `data-*`         | Key names start with a lowercase letter and contain only letters, numbers, underscores, dots, and hyphens; values must not contain control characters |
| `title`          | Must not contain HTML tags (`<...>`)                                                              |
| `align`          | Accepts only `left`/`center`/`right` (or abbreviations `l`/`c`/`r`)                               |
| `dir`            | Accepts only `ltr`/`rtl`/`auto`                                                                   |
| `lang`           | Conforms to BCP 47 format (`primary-tag-subtag`, 1-8 alphanumeric characters per segment)         |

Attribute values that fail validation are silently discarded and will not appear in the output HTML. Unknown attribute names are also rejected.

**Default Disabling of the `style` Attribute**

The `style` attribute allows direct control over an element's visual presentation. Therefore, even if validated, legitimate CSS can still be exploited for UI hijacking (e.g., positional overlays, clickjacking, content hiding) or resource loading detection (loading external resources via `url()`). For this reason, the `style` attribute is disabled by default and must be explicitly enabled via `enable_style: true`. When enabling this feature, developers should ensure the application context is trusted (e.g. limited to internal documentation systems rather than general applications for the public).

**Validation Module Availability Check**

Due to the complex format of `style` and `data-*` attribute values, the parser relies on a separate validation module to verify their syntax. If the validation module is not loaded (i.e., `window.DrewMarkValidate` is unavailable), these two types of attributes are rejected outright rather than being allowed through. This ensures that even if an abnormal script loading order results in a missing validation module, these attributes will never be output in an unvalidated state.

### 5.3 Output Escaping

**HTML Entity Escaping**

When rendering HTML, the parser escapes **user-inputted text content** (including text nodes, code content, table titles, and etc.) via the `escHtml` function, converting five characters (`&`, `<`, `>`, `"`, `'`) into their corresponding HTML entities (`&amp;`, `&lt;`, `&gt;`, `&quot;`, `&#39;`). This prevents HTML tags in user text from being interpreted and executed by the browser. For example, if a user inputs `**<script>**`, the parser recognizes the `**` markup, extracts the text node `<script>`, escapes it to `&lt;script&gt;`, appends its own generated `<strong>` tags, and finally outputs `<strong>&lt;script&gt;</strong>`.

**Attribute Value Escaping**

All content outputted into HTML attribute values (e.g., `title`, `class`, `style`) is escaped via the `escAttr` function, converting `&`, `<`, `>`, `"`, `'` into HTML entities to prevent attribute value injection (e.g., injecting new attributes by closing attribute quotes with `"`).

**Style Block Content Escaping**

When style block content is outputted inside `<style>` tags, the `</style` sequence is escaped (replaced with `<\/style`). In the HTML specification, `<style>` is a raw text element terminated only by `</style`; this escaping prevents users from prematurely closing the style block and injecting scripts via `</style>...<script>`.

**Code Block Content Escaping**

Code block content is first escaped via `escHtml` before being outputted into `<pre><code></code></pre>`, ensuring HTML tags within the code (e.g., `<div>`) are displayed as text and not interpreted or executed by the browser.

### 5.4 Exception Fallback

The parser wraps its main entry point in a `try-catch` block. If an error is thrown during parsing for any reason (abnormal input, regex backtracking errors, internal rule exceptions, etc.), the parser does not crash nor output blank content. Instead, it falls back to safely outputting the original text escaped via `escHtml` and wrapped in `<pre>` tags. This guarantees that even in extreme edge cases, users see escaped original text rather than an unusable blank page.

### 5.5 Syntax Disabling

Via the `disable_syntax` parameter, developers can precisely disable specific syntax groups or items (e.g., `style`, `code-block`, `list`). Disabling uses exact matching to avoid `'code'` mistakenly disabling `'codes'`. The `style` attribute and style blocks are disabled by default, ensuring no security risks exist even if developers overlook these high-risk factors.

---

*Version: v2.8*