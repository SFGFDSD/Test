# JISHENG OS 应用开发指南

> 适用版本：V10.5.2+

## 1. 简介

JISHENG OS 支持用户通过简单的 HTML 标记声明应用。当一个 HTML 文件包含特定的应用声明标记时，JISHENG OS 会将其识别为独立应用，而不是在浏览器中打开。应用窗口不会显示浏览器地址栏，提供更加原生的应用体验。

你只需要一个 HTML 文件，在文件开头添加一行特定的注释标记，就可以让它成为 JISHENG OS 应用。

---

## 2. 快速开始

### 2.1 最小示例

创建一个新的 HTML 文件，在第一行添加应用声明标记：

```html
<!-- JISHENG-OS-APP: {"name":"我的应用"} -->
<!DOCTYPE html>
<html lang="zh-CN">
<head><meta charset="UTF-8"><title>我的应用</title></head>
<body>
    <h1>Hello JISHENG OS!</h1>
</body>
</html>
```

将这个文件上传到 JISHENG OS 桌面，双击即可以作为独立应用打开——没有浏览器地址栏，窗口标题显示为"我的应用"。

---

## 3. 应用标记格式

### 3.1 标记语法

应用声明使用 HTML 注释格式，必须放在文件的最前面（建议放在第一行）：

```html
<!-- JISHENG-OS-APP: {JSON配置} -->
```

### 3.2 配置字段

标记内的 JSON 对象支持以下字段：

| 字段 | 类型 | 必填 | 说明 |
|------|------|:----:|------|
| `name` | string | **是** | 应用名称，显示在窗口标题栏和桌面图标下方 |
| `icon` | string | 否 | 自定义图标，传入完整的 SVG 元素代码（含 `<svg>` 标签）。省略则使用默认图标 |
| `width` | number | 否 | 窗口初始宽度（像素），默认 `700` |
| `height` | number | 否 | 窗口初始高度（像素），默认 `500` |

---

## 4. 完整示例

### 4.1 带完整配置的应用

以下是一个带完整配置的示例，包含自定义图标和窗口尺寸：

```html
<!-- JISHENG-OS-APP: {
  "name": "差异对比",
  "width": 1100,
  "height": 700,
  "icon": "<svg class='icon-svg' viewBox='0 0 24 24' fill='none' stroke='currentColor' stroke-width='2'><path d='M12 3v18M3 12h18M8 8l-5 4 5 4M16 8l5 4-5 4'/></svg>"
} -->
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>差异对比</title>
    <style>
        body { font-family: sans-serif; padding: 20px; }
    </style>
</head>
<body>
    <h1>差异对比工具</h1>
</body>
</html>
```

### 4.2 自定义图标说明

`icon` 字段接受完整的 SVG 元素代码。建议使用 `24x24` 视口的简洁图标，与 JISHENG OS 系统图标风格保持一致。

**推荐风格：**
- 使用 `stroke` 线条风格
- `stroke-width` 设为 `2`
- `fill` 设为 `none`
- 视口 `viewBox` 为 `0 0 24 24`

**示例图标（差异对比图标）：**

```json
"icon": "<svg class='icon-svg' viewBox='0 0 24 24' fill='none' stroke='currentColor' stroke-width='2'><path d='M12 3v18M3 12h18M8 8l-5 4 5 4M16 8l5 4-5 4'/></svg>"
```

**省略图标时使用默认图标：**

```html
<!-- JISHENG-OS-APP: {"name":"记事本"} -->
```

---

## 5. 部署方式

完成开发后，有两种方式将应用部署到 JISHENG OS：

### 5.1 方式一：上传到桌面

直接将 HTML 文件拖拽到 JISHENG OS 桌面上，或通过文件管理器上传。双击文件即可作为应用打开。

### 5.2 方式二：放入仓库

将 HTML 文件放入 JISHENG OS 项目仓库的应用目录中，其他用户可以通过仓库获取并使用你的应用。

---

## 6. 注意事项

- 标记必须是**有效的 JSON 格式**，否则会被忽略，文件将作为普通 HTML 在浏览器中打开
- `name` 字段是**必填**的，缺少则不会被识别为应用
- 标记必须出现在文件的**前 200 行**内（系统只搜索前 200 行）
- 应用运行在 iframe 中，支持 JavaScript、表单提交、弹出窗口（sandbox 策略）
- 应用窗口**不含浏览器地址栏和导航按钮**，提供更原生的应用体验
- 建议为应用设置合适的初始窗口尺寸，提升用户体验

---

## 7. 示例应用

仓库中包含以下示例应用，可以作为参考：

| 应用名称 | 文件名 | 功能说明 |
|----------|--------|----------|
| 差异对比 | `差异对比工具.html` | HTML 文件差异对比，支持行级/字符级/词级对比 |
| 计算器 | `test-jisheng-app.html` | 基础计算器应用，演示最小配置 |
