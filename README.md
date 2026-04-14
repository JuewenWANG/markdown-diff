# Markdown Diff 对比工具 —— 使用说明

## 📁 文件位置

工具文件：`markdown-diff/index.html`（单文件，零依赖）

---

## 🚀 快速启动

### 方法一：本地 HTTP 服务器（推荐）

```bash
cd markdown-diff
python3 -m http.server 8766
```

然后访问：`http://localhost:8766/index.html`

### 方法二：直接用浏览器打开

双击 `index.html` 文件即可（使用 `file://` 协议打开）。

---

## 📊 在 Excel / WPS 中使用

### 核心原理

工具支持通过 URL 参数 `?old=xxx&new=xxx` 传入两段 Markdown 文本，打开链接即自动对比。

在 Excel 中，用 `HYPERLINK` + `ENCODEURL` 公式拼接出这个链接即可。

### 步骤

1. **启动本地服务器**（见上方），记住地址如 `http://localhost:8766/index.html`

2. **在 Excel 中设置数据**：
   - A 列 = 旧版 Markdown
   - B 列 = 新版 Markdown

3. **在 C 列写公式**（以第 2 行为例）：

```
=HYPERLINK("http://localhost:8766/index.html?old="&ENCODEURL(A2)&"&new="&ENCODEURL(B2), "查看Diff")
```

4. **点击 C 列的"查看Diff"链接**，浏览器自动打开并显示两版内容的差异。

### ⚠️ 注意事项

| 问题 | 解决方案 |
|------|----------|
| Excel 版本无 `ENCODEURL` 函数 | 升级到 Excel 2013+，或使用 WPS（支持该函数） |
| Markdown 内容过长（URL 超限） | 见下方"长内容处理方案" |
| `file://` 协议下链接不可点击 | 请使用 HTTP 服务器方式部署 |

---

## 📏 长内容处理方案

URL 有长度限制（约 2000-8000 字符，取决于浏览器），如果 Markdown 很长，可以用以下方案：

### 方案：Hash 模式

把数据放在 URL 的 `#` 后面（不受服务器限制，但仍有浏览器限制）：

```
=HYPERLINK("http://localhost:8766/index.html#"&ENCODEURL("{""old"":"""&SUBSTITUTE(A2,"""","\\""")&""",""new"":"""&SUBSTITUTE(B2,"""","\\""")&"""}"), "查看Diff")
```

### 超长内容建议

对于非常长的文档，建议直接打开工具页面手动粘贴对比，不走 URL 参数。

---

## 🎨 功能特性

- ✅ **颜色高亮**：绿色 = 新增，红色 = 删除
- ✅ **行级 + 词级对比**：不仅标出改了哪行，还标出行内具体改了哪些词
- ✅ **双视图模式**：统一视图（上下排列）/ 左右对比视图
- ✅ **统计面板**：显示新增/删除/相同行数
- ✅ **纯前端**：所有处理在浏览器完成，不上传任何数据
- ✅ **零依赖**：单个 HTML 文件，无需安装任何包

---

## 🔗 URL 参数说明

| 参数 | 说明 | 示例 |
|------|------|------|
| `old` | 旧版 Markdown 文本（URL 编码） | `?old=hello` |
| `new` | 新版 Markdown 文本（URL 编码） | `&new=world` |
| `view` | 视图模式：`unified`（默认）或 `side` | `&view=side` |

---

## 示例链接

```
http://localhost:8766/index.html?old=%23+Title%0Aold+content&new=%23+Title%0Anew+content
```
