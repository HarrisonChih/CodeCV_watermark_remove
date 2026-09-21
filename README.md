# CodeCV Resume Watermark Remover

一个用于移除 CodeCV 导出简历 PDF 水印的小脚本。

脚本直接在 PDF 内容流 / 资源层移除水印绘制指令，不会把 PDF 转成图片再涂白，因此可以尽量保留原始文本、头像、标签底纹、排版和页面尺寸。

## 功能

- 移除 CodeCV 导出 PDF 中的 `/Pattern` 平铺水印
- 移除大尺寸半透明图片水印（带 `/SMask` alpha、铺满页面中部的斜向 `CodeCV简历` 水印图）
- 保留技能标签的浅色圆角底纹、头像等正常设计元素
- 保留原 PDF 页数、页面尺寸和可抽取文本
- 支持命令行指定输入、输出路径
- 如果不指定输出路径，会自动生成 `*.clean.pdf`

## 支持的水印类型

| 类型 | 特征 | 处理方式 |
|------|------|----------|
| Pattern 平铺水印 | `/Pattern` 颜色空间填充 | 删除对应填充指令与 Pattern 资源 |
| 半透明图片水印 | `/Image` + `/SMask` 且尺寸 ≥1000×600 | 删除其 `Do` 指令与图片资源 |

> 注意：技能标签（如 `985`、`CCF-A`）的浅色圆角底纹是正常设计元素，脚本不会误删。

## 安装

需要 Python 3.10+。

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

也可以直接使用已有 Python 环境安装依赖：

```bash
python3 -m pip install -r requirements.txt
```

## 使用

指定输出文件：

```bash
python3 remove_codecv_watermark.py "带水印简历.pdf" "去水印简历.pdf"
```

不指定输出文件时，默认输出到同目录的 `.clean.pdf` 文件：

```bash
python3 remove_codecv_watermark.py "带水印简历.pdf"
```

运行成功后会看到类似输出：

```text
Removed 1 watermark element(s).
Wrote: 带水印简历.clean.pdf
```

## 快速检查

```bash
python3 remove_codecv_watermark.py --help
```

如果能正常显示帮助信息，说明依赖安装和脚本入口正常。

## 注意事项

- 本脚本针对 CodeCV 导出的 `/Pattern` 平铺水印与大尺寸半透明图片水印。
- 移除到至少一个水印时退出码为 0，未移除到任何水印时退出码为 1。
- 如果 CodeCV 后续改变导出实现，脚本可能需要调整。
