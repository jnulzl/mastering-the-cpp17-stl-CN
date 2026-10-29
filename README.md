# Mastering the C++17 STL（中文翻译版 · LaTeX 工程）

把 Arthur O'Dwyer 的 *Mastering the C++17 STL*（Packt, 2019）全书译为中文，

- 原书：*Mastering the C++17 STL*
- 作者：Arthur O'Dwyer（arthurodwyer.com）
- 中文名：**Mastering the C++17 STL（中文版）**

---

## 目录结构

```
latex_source/                  # 本目录即工程根
├── main.tex                   # 编译入口：\input{book/ccs.tex} + \subfile{book/index.tex}
├── build.bat                  # 一键编译脚本（见「编译」小节说明）
├── fonts/                     # 字体文件（JetBrains Mono 等宽字体，备用于代码块）
├── images/                    # 44 个图片：cover_original.png（原书封面）+ 43 张正文插图
├── main.pdf                   # 编译成品（全书含前置页、12 章正文与索引）
└── book/
    ├── ccs.tex                # 导言区：自定义命令/环境、字体映射、minted v3 缓存配置
    ├── index.tex              # 章节装配（手写静态文件，增删章节改这里）
    ├── manning-pygments-style.tex  # 代码高亮配色
    └── content/
        ├── front/             # 00-cover(原书封面) / 00-title(中文内封) / 01-preface
                               #   / 02-author / 03-reviewer
        ├── main/              # 第 1–12 章（main/01 … main/12）
        └── back/              # 00-index（真实页级索引，双栏 multicols）
```

---

## 环境依赖

| 依赖 | 版本 / 说明 |
|---|---|
| TeX Live | 2026（含 `xelatex`、minted **v3** / `latexminted`、`pygmentize`） |
| Python | 3.x（提供 `pygmentize`，且 `latexminted` 脚本需要可用的 `python.exe`） |
| 中文字体 | **Source Han Serif SC（思源宋体）**、**Microsoft YaHei（微软雅黑）** |
| 拉丁字体 | Cambria、Franklin Gothic Medium、Consolas |
| 等宽字体 | Consolas（Windows 自带）；`fonts/` 另备 JetBrains Mono，可改用 |

> 字体缺失会导致编译报 "font not found"。思源宋体与微软雅黑需本机已安装。

---

## 编译

### 方式 A：命令行（推荐，可控）

在本目录（`latex_source/`）下用命令行编译。**关键**是把 minted v3 的缓存目录指向
工程绝对根（见「已知坑」第 2 条），必须设置 `TEXMF_OUTPUT_DIRECTORY` / `TEXMFOUTPUT`：

```bash
# 1) 设置 PATH：Python 根目录必须在 TeX bin 之前（避免 latexminted 被其它副本遮蔽）
export PATH="/d/anaconda3:/d/ProgramData/texlive/2026/bin/windows:$PATH"
# 2) 让 minted v3 把缓存写到绝对路径（见「已知坑」第 2 条），必须指向本工程绝对根
export TEXMF_OUTPUT_DIRECTORY="D:/Ego/trans/Mastering the C++17 STL/latex_source"
export TEXMFOUTPUT="$TEXMF_OUTPUT_DIRECTORY"

# 3) 跑三遍（封面用 remember picture 需 ≥2 遍才定位；TOC/书签需 3 遍才稳定）
xelatex -shell-escape -interaction=nonstopmode -synctex=1 main.tex
xelatex -shell-escape -interaction=nonstopmode -synctex=1 main.tex
xelatex -shell-escape -interaction=nonstopmode -synctex=1 main.tex
```

- `-shell-escape` 是必须的（minted 调外部高亮器）。
- 首次或 `book/content/**` 有变动后，minted 会重高亮全部代码清单（数分钟）；
  缓存命中时一遍约 1 分钟。
- 用 `-synctex=1` 可保留正反对应，便于从 PDF 跳回源码定位。
- 编译产物：`main.pdf`、以及 `main.aux` / `main.toc` / `main.log` 等。
- 具体页数执行后用 `pdfinfo main.pdf` 查看。

### 方式 B：build.bat（一键）

双击 `build.bat` 即可，它已正确设置 `TEXMF_OUTPUT_DIRECTORY` 并跑满三遍、最后清理 `*.minted`。

> ⚠️ 两点说明：
> 1. `build.bat` 头部注释写着 "C++ Software Design" 是**别书脚本的复制残留**，与本工程无关，
>    不影响实际编译（路径、缓存、三遍 xelatex 均按本项目配置）。
> 2. 它会尝试调用 `..\work\tools\gen_index.py` 做两阶段索引；由于本项目索引是**静态手写**
>    （`book/content/back/00-index.tex`），该脚本不存在，build.bat 会自动跳过索引生成，
>    直接进行正文三遍编译——对本项目完全安全。

---

> 如果环境配置遇到问题或者编译失败可以直接到[releases](https://github.com/jnulzl/mastering-the-cpp17-stl-CN/releases)下载最新的pdf文件
>
## 非官方代码

[mastering-the-cpp17-stl](https://github.com/packtpublishing/mastering-the-cpp17-stl) —— 书中示例代码。
