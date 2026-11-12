# RimMind brand assets

RimMind 组织品牌素材。**SVG 是唯一源文件**，PNG 均由 SVG 导出，供 GitHub 头像、favicon、文档和站点使用。

## 文件清单

| 文件 | 尺寸 | 用途 |
|---|---|---|
| `logo.svg` | 矢量 | 黑稿，浅色背景使用；四周保留约 10% 安全边距并光学居中 |
| `logo-inverse.svg` | 矢量 | 白稿，深色背景使用 |
| `avatar-500.png` | 500×500 | **组织头像上传用**（GitHub 推荐约 500×500） |
| `avatar-1000.png` | 1000×1000 | 高分辨率备用（物料、海报） |
| `avatar-dark-500.png` | 500×500 | 深色版头像（可选） |
| `avatar-transparent-500.png` | 500×500 | 透明底，用于叠加合成 |
| `favicon-32.png` | 32×32 | 浏览器标签页图标（白底、无内边距，小尺寸更饱满） |
| `favicon-64.png` | 64×64 | 高 DPI favicon |

## 设置组织头像

**仓库里的图片不会自动变成组织头像**，需要在 GitHub 设置中手动上传：

1. 打开 <https://github.com/organizations/RimWorld-RimMind-Mod/settings/profile>
2. **Profile picture** → **Upload a photo…** → 选择 `avatar-500.png`
3. 按 1:1 裁剪后保存

GitHub 会把组织头像渲染为**圆角方形**。这里使用白底黑稿，保证在浅色和深色主题下都清晰可辨。

## 在 README 中引用

```html
<picture>
  <source media="(prefers-color-scheme: dark)"
          srcset="https://raw.githubusercontent.com/RimWorld-RimMind-Mod/.github/main/assets/brand/logo-inverse.svg">
  <img src="https://raw.githubusercontent.com/RimWorld-RimMind-Mod/.github/main/assets/brand/logo.svg"
       width="96" alt="RimMind">
</picture>
```

## 规格

- 图形基于 100×100 网格（`viewBox="0 0 100 100"`），由两条左侧弧线、竖直主干 + 三条分支、三个节点圆点组成
- 黑稿 `#000000`；白稿 `#E6EDF3`（GitHub 深色主题前景色）
- 头像优先使用 `avatar-500.png`：GitHub 上传限制为 PNG/JPG/GIF、小于 1 MB、小于 3000×3000
- 小尺寸（≤64px）不要使用带安全边距的版本，改用 `favicon-32.png` 的满版构图，否则细节会糊

## 重新导出

修改矢量源后，用任意 SVG 渲染器导出 PNG（保持 1:1、透明度/背景按上表）：

```bash
# Inkscape 示例
inkscape logo.svg --export-type=png --export-width=500 --export-filename=avatar-500.png
```

> 本目录素材与仓库其他内容一致，按 MIT 许可发布。
