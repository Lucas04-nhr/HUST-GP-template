# windows上从0开始以解决biber缺失问题（~~或许改改也适合mac呢~~）

## Background

在win11使用模板时，无法正确识别参考文献（.bib）。

具体表现为：

- 编译完成后没有.bbl文件
- 引用处无法正确显示序号
- 文末无法自动生成参考文献列表

## Core problem

如果出现上述问题，那么很可能是因为biber没有安装，或者编辑器无法正确识别biber路径。此时需要你手动安装biber，手动使用biber编译.bib文件以生成所需的.bbl文件。

### Quickly resolve
```bash
tlmgr install biber
biber --version 
biber sample
```


如果仍然无法解决问题，本文提供了一种在windows上解决该问题的完整流程。为确保通用性，该流程从全新的wsl（Ubuntu）环境开始，直到能正常完成编译为止。

## Pipline

### Pre: 请提前新建一个文件夹，并在其下创建测试文件 "sample.tex"，内容如下：

```tex
# sample.tex
\documentclass{article}
\usepackage{fontspec}
\usepackage[backend=biber, style=authoryear]{biblatex}

% 嵌入参考文献数据（替代单独的 .bib 文件）
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

\addbibresource{references.bib} % 仍需关联，但数据已嵌入

\begin{document}

引用示例：\parencite{sample1} 和 \textcite{sample2}

\printbibliography

\end{document}
```

### Start: _wsl_ + _tinytex_ 

打开你的powershell，并按顺序执行以下指令，推荐一行一行复制粘贴

```bash
# wsl
wsl --list --verbose
wsl --terminate Ubuntu
wsl --unregister Ubuntu
wsl --install -d Ubuntu # use mirror will faster
exit
wsl --set-default Ubuntu
wsl --list --verbose
wsl

# hust mirror(faster)
curl -sSfL https://mirrors.hust.edu.cn/get | sh -s -- autodeploy -y

# fonts
sudo apt-get install ttf-mscorefonts-installer
sudo fc-cache -f -v

# tinytex
sudo apt-get update
sudo apt-get install python3-pip
sudo apt-get install r-base

# cd wkdir
R
install.packages('tinytex')
tinytex::tlmgr_repo('http://mirrors.tuna.tsinghua.edu.cn/CTAN/') # 一定先换源
tinytex::install_tinytex()
# tinytex::uninstall_tinytex()
update.packages(ask = FALSE, checkBuilt = TRUE)
tinytex::tlmgr_update()

# auto download missing packages
# tinytex::pdflatex('sample.tex') 
tinytex::xelatex('sample.tex') # without references, can not use

# if have unsolved problem
# tinytex::reinstall_tinytex()

q()


# if tlmgr not find (carefully check your path!)
ls ~/.TinyTeX/bin/x86_64-linux/
vi ~/.bashrc # add PATH at tail
# export PATH="$HOME/.TinyTeX/bin/x86_64-linux:$PATH"
source ~/.bashrc

# biber && xelatex
xelatex --version 
biber --version 
tlmgr install biber


# cd wkdir && start
xelatex sample.tex
biber sample
xelatex sample.tex

# congratulations！🎉🎉🎉
```

## Note

本教程专注于解决biber无法使用的问题，如果在编译时出现其他问题，请先尝试通过R安装可能缺失的宏包。

### For example

```bash
# cd wkdir
xelatex mylatex.tex
biber mylatex
xelatex mylatex.tex

# 如果报错，先尝试
R
tinytex::xelatex("mylatex.tex") # 自动安装缺失的宏包
q()

# 再次尝试编译
xelatex mylatex.tex
biber mylatex
xelatex mylatex.tex

```


## [返回主教程传送门](README.md)