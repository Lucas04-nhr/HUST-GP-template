# Windows（WSL）+ TinyTeX：biber 问题快速修复指南（Windows 简明教程）

## 概要

使用本模板在 Windows（通过 WSL）上编译时，可能出现参考文献无法生成（无 .bbl、引用编号缺失、参考文献列表为空）的问题。通常原因是 biber 未安装或路径不可见。本文给出从零开始在 WSL（Ubuntu）中安装 TinyTeX 并确保 biber 可用的简明流程。

> 说明：文中示例使用 sample.tex 作为演示用的主 .tex 文件；实际操作时请将其替换为你要编译的文件名（例如 mypaper.tex）。

## 快速定位（最小化操作）
在有 R 与 TinyTeX 的系统上，先尝试：
```bash
tlmgr install biber
biber --version
biber sample
xelatex sample.tex
```
若仍无效，按下文完整流程操作（推荐在全新 WSL 环境中逐步执行）。

## 测试用例（在工作目录新建 sample.tex）
下面的 sample.tex 将内嵌参考文献，便于测试 biber 是否正常工作。

```tex
% sample.tex
\documentclass{article}
\usepackage{fontspec}
\usepackage[backend=biber, style=authoryear]{biblatex}

\begin{filecontents*}{references.bib}
@article{sample1,
  author={Author, A and Author, B},
  title={Sample Article 1},
  year={2020},
  journal={Journal of Examples}
}

@book{sample2,
  author={Writer, C},
  title={Sample Book 2},
  year={2021},
  publisher={Example Press}
}
\end{filecontents*}

\addbibresource{references.bib}

\begin{document}
引用示例：\parencite{sample1} 和 \textcite{sample2}
\printbibliography
\end{document}
```

## 全流程（推荐按顺序执行）

1. WSL 安装与重置（可选，针对遇到配置问题时）
```bash
# 查看与重置 WSL（视需要执行）
wsl --list --verbose
wsl --terminate Ubuntu
wsl --unregister Ubuntu
wsl --install -d Ubuntu  # 可使用镜像以加速
exit # 返回 PowerShell
wsl --set-default Ubuntu
wsl --list --verbose
wsl
```

2. 使用国内 ***HUST*** 镜像（可选，加速包安装）
```bash
curl -sSfL https://mirrors.hust.edu.cn/get | sh -s -- autodeploy -y
```

3. 安装字体与刷新缓存（如果需要中文/Windows 常用字体）
```bash
sudo apt-get install ttf-mscorefonts-installer
sudo apt install fonts-noto-cjk
sudo fc-cache -f -v
```

4. 安装 R 与 TinyTeX（推荐使用 R 来安装 TinyTeX）
```bash
sudo apt-get update
sudo apt-get install python3-pip
sudo apt-get install r-base

# 进入 R 环境安装 tinytex
R
install.packages('tinytex')
tinytex::tlmgr_repo('http://mirrors.tuna.tsinghua.edu.cn/CTAN/') # 切换镜像
tinytex::install_tinytex()
# 如需重装： tinytex::uninstall_tinytex(); tinytex::install_tinytex()
update.packages(ask = FALSE, checkBuilt = TRUE)
tinytex::tlmgr_update()
q()
```

5. 确保 PATH 包含 TinyTeX 可执行文件（如 tlmgr 未找到）
```bash
ls ~/.TinyTeX/bin/x86_64-linux/
# 若需要，编辑 ~/.bashrc 并追加：
# export PATH="$HOME/.TinyTeX/bin/x86_64-linux:$PATH"
source ~/.bashrc
```

6. 安装并检查 biber、测试编译
```bash
tlmgr install biber
xelatex --version
biber --version

# 先进入 R 环境自动安装可能需要的宏包
R
tinytex::xelatex('sample.tex')
q()

# 在工作目录下：
xelatex sample.tex      # 第一次生成辅助文件
biber sample            # 这一步应该生成 .bbl 文件
xelatex sample.tex      # 参考文献正常显示
```

## 常见故障及排查建议

- 如果 biber 命令不存在：检查 TinyTeX 是否正确安装，以及 PATH 是否包含 TinyTeX 的 bin 路径。可通过 ls 查看目录并在 ~/.bashrc 中添加 PATH。
- 若 xelatex 报告缺少宏包，可在 R 控制台运行 tinytex::xelatex('sample.tex')，TinyTeX 会尝试自动下载并安装所需宏包；也可选择手动安装。更多维护说明（英文）：https://yihui.org/tinytex/#maintenance
- 如果 biber 报错但已安装：确认 biber 版本与 biblatex backend 配置匹配（通常使用 biber 而非 bibtex）。检查工作目录下的 .bcf/.bib/.aux 文件是否正常生成。
- 如果使用 VSCode/TeXstudio 等编辑器：确认编辑器使用的 TeX 发行版是 WSL 中的 TinyTeX，或将编辑器配置为调用 WSL 中的命令。

## 备注

- 本文主要解决 biber 在 WSL/TinyTeX 环境中不可用的问题。其他编译问题请先尝试通过 R 的 tinytex 自动安装缺失宏包，再排查具体错误信息。

示例重试流程（快速参考）：
```bash
# cd 到工作目录
xelatex mylatex.tex
biber mylatex
xelatex mylatex.tex

# 若失败，先让 tinytex 自动安装缺包（在 R 中）
R
tinytex::xelatex("mylatex.tex")
q()
# 再次按上面的顺序编译
```

返回主教程：[README.md](../README.md)