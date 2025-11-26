# 适用于 HUST 的本科毕业论文 LaTeX 模板

## 前言

学校似乎只提供了 Word 格式的论文模板，然而 $\LaTeX$ 在排版学术论文方面有着显著优势，尤其是在需要插入大量图片、代码及表格的情况下。

因此，我基于之前各类实验报告的模板，参考着学校要求，创建了一个基于 $\LaTeX$ 的本科毕业论文模板。

## 使用说明

> [!NOTE]
> - 本模板基于 $\LaTeX$ 制作，如需使用建议先行学习相关知识。
> - 软件及字体的安装教程不在此赘述，可参考网络资源。
> - 如果在生命学院大二开设的 R 语言课程中使用 R Markdown 编写过实验报告并且后期没有卸载过 TinyTeX，那么你已经具备了使用本模板的基本环境。
> - 如果不确定是否具有基本环境，请参考 [Rapheal](https://github.com/chide-org)  提供的[Windows 简明教程（含 biber 问题修复）](./doc/wsl_biber_fix.md)

### 先决条件

- 安装 $\LaTeX$ 发行版（推荐使用由 Yihui Xie 维护的 [TinyTeX](https://yihui.org/tinytex/)）。
- 安装 $\LaTeX$ 编辑器（理论上任何文本编辑器都可以对文件进行编辑）。
- 安装必要的字体（如 SimSun、SimHei 等）。

> [!TIP]
> - 本人使用 macOS 系统的 VS Code 作为编辑器，通过 TinyTeX 附带的 XeLaTeX 引擎利用 `latexmk` 进行更自动化的编译。这两个工具都是跨平台的，理论上可以在 Windows 和 Linux 系统上使用。
> - 如果你不想配置环境，也可以将模板上传至 [Overleaf](https://www.overleaf.com/) 或 [FlyLaTeX](https://www.flylatex.cn/) 等在线 $\LaTeX$ 编辑器中进行编辑和编译。学校提供了 FlyLaTeX 编辑器的高级版功能（协作、历史版本等），使用统一身份认证登录后即可免费使用。
> - 根据学院讲座的模板，论文中可能使用到的字体有宋体(SimSun/FandolSong)、黑体(SimHei/FandolHei)等，其中 [Fandol 系列字体](https://ctan.org/tex-archive/fonts/fandol)是 CTAN 官方的默认字体，考虑到生信专业大部分干实验可能需要插入代码，模板中也附带了等宽字体 [Maple Mono NF CN](https://github.com/Subframe7536/maple-font) 的支持，推荐安装上述所有字体以确保论文编译效果最佳。

### 下载模板

前往本仓库的 [Releases](https://github.com/Lucas04-nhr/HUST-GP-template/releases) 页面，下载**最新版本**的压缩包并解压。

> [!CAUTION]
> - 请务必下载 Releases 页面中的压缩包，而非直接克隆或下载本仓库的 ZIP 文件。因为 Releases 页面中的压缩包通过自动化流程排除了仓库中可能存在的中间文件，确保可以顺利编译。
> - 请尽可能使用最新的版本，因为模板会随着时间进行更新和优化。本人只会对最新版本提供**有限的**支持。

### 在编辑器中打开模板并进行编辑修改

使用你喜欢的文本编辑器打开解压后的模板文件夹，建议直接打开整个文件夹以便于管理。此处以 VS Code 为例：

> [!TIP]
> 如果你使用 VS Code，可以安装 [LaTeX Workshop](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop) 插件以获得更好的 $\LaTeX$ 编辑体验。

- `sample.tex` 是模板的主文件，其他文件中包含了参考文献、图片等资源。
- 论文的正文部分位于 `sample.tex` 文件中的 `\begin{document}` 和 `\end{document}` 之间。
- 参考文献存储在 `references.bib` 文件中，使用 BibTeX 格式进行管理。
- 图片资源存储在 `fig` 文件夹中，你可以将自己的图片放入该文件夹并在正文中引用。
- `titlepage.tex` 文件包含论文的原创性声明和版权页，一般不需要进行修改。
- `cnabstract.tex` 和 `enabstract.tex` 文件分别包含中文和英文摘要部分。
- `cover.tex` 文件包含封面部分，请将个人信息部分修改为正确的内容。

> [!CAUTION]
> - 由于在线编辑器的局限性，可能无法使用 `minted` 宏包进行代码高亮显示，特殊代码框样式不受影响。
> - 论文的标题及副标题请在 `sample.tex` 文件的`\begin{document}`之前的对应部分进行修改，会自动被引用到封面和标题页中。
> - 请确保在编辑过程中不要更改模板中的命令和环境定义，除非你非常清楚自己在做什么，否则可能会导致编译错误或排版问题。

#### 一些自定义的命令

在 `sample.tex` 文件中已经包含有部分自定义命令的示例，你可以根据需要进行使用：

```tex
% 插入图片
\begin{figure}[htbp]
  \centering
  \mygraphics{fig/logo.png}
  \piccaption{示例图片}
  \label{fig:example}
\end{figure}

% 插入表格
\begin{table}[htbp]
  \centering
  \tbcaption{示例表格}
  \begin{tabular}{ccc}
    \toprule
    项目 & 数值1 & 数值2 \\
    \midrule
    A & 10 & 20 \\
    B & 30 & 40 \\
    \bottomrule
  \end{tabular}
  \label{tab:example}
\end{table}

% 插入 Tikz 流程图
\begin{figure}[htbp]
  \piccaption{示例流程图}
  \label{fig:flowchart}
  \centering
  \begin{tikzpicture}[node distance=1.8cm, every node/.style={font=\small}]
    \node[process] (start) {开始};
    \node[process, right=of start] (p1) {步骤 1};
    \node[process, right=of p1] (p2) {步骤 2};
    \node[process, below=of p1] (p3) {步骤 3};
    \node[process, right=of p3] (end) {结束};

    \draw[arrow] (start) -- (p1);
    \draw[arrow] (p1) -- (p2);
    \draw[arrow] (p1) -- (p3);
    \draw[arrow] (p3) -- (end);
    \draw[twowayarrow] (p2.south) -- (p3.north);

    % Group box and annotation
    \node[edge, fit=(p1) (p2) (p3), label=above:流程组] {};
  \end{tikzpicture}
\end{figure}

% 插入代码
\begin{minted}[linenos,fontsize=\small,breaklines]{python}
def fibonacci(n):
if n <= 0:
  return []
seq = [0, 1]
while len(seq) < n:
  seq.append(seq[-1] + seq[-2])
return seq[:n]
\end{minted}
```

此外，模板还预定义了多种不同规格的图片大小，便于适应多栏布局：

```tex
% 0.6 倍宽度图片（单栏）
\newcommand{\mygraphics}[1]{\includegraphics[width=0.6\textwidth,height=\textheight,keepaspectratio]{#1}}
% 0.4 倍宽度图片（单栏小图）
\newcommand{\mygraphicssmall}[1]{\includegraphics[width=0.4\textwidth,height=\textheight,keepaspectratio]{#1}}
% 0.5 倍宽度图片（双栏）
\newcommand{\mygraphicstwocols}[1]{\includegraphics[width=0.25\textwidth,height=\textheight,keepaspectratio]{#1}}
% 0.3 倍宽度图片（三栏）
\newcommand{\mygraphicsthreecols}[1]{\includegraphics[width=0.15\textwidth,height=\textheight,keepaspectratio]{#1}}
% 0.2 倍宽度图片（四栏）
\newcommand{\mygraphicsfourcols}[1]{\includegraphics[width=0.1\textwidth,height=\textheight,keepaspectratio]{#1}}
% 0.8 倍宽度图片（子图）
\newcommand{\mygraphicssubfig}[1]{\includegraphics[width=0.8\textwidth,height=\textheight,keepaspectratio]{#1}}
% 0.4 倍宽度图片（子图小图）
\newcommand{\mygraphicssubsubfig}[1]{\includegraphics[width=0.2\textwidth,height=\textheight,keepaspectratio]{#1}}
```

可以在文章中直接使用这些命令来插入不同规格的图片。

```tex
% 如果图片比例特殊可以使用 resizebox 命令调整大小
\resizebox{\textwidth}{!}{
  \begin{figure}[htbp]
    \centering
    \piccaption{两栏图片示例}
    \begin{multicols}{2}
      \mygraphicssmall{fig/logo.png}
      \mygraphicssmall{fig/logo.png}
    \end{multicols}
    \label{fig:twocols}
  \end{figure}
}
```

模板内置了针对中文字号的支持，具体字号命令如下：

```tex
\chuhao % 初号
\xiaochuhao % 小初号
...
\xiaowuhao % 小五号
```

### 编译模板

#### 环境配置

在编辑器中打开 `.latexmkrc` 文件，确保其中的环境变量路径配置正确，将 `'$YOUR_CUSTOM_FILE_PATH'` 替换为你本地安装 `pygments` 以及 `minted` 的路径，否则可能会在有代码插入时出现问题：

```perl
$ENV{'PATH'} = '~/Developer/miniconda3/bin:' . $ENV{'PATH'};
```

#### 编译命令

在编辑器中打开 `sample.tex` 文件后，可以使用以下命令进行编译：

```bash
latexmk sample.tex
```

该命令会根据已经预定义好的`.latexmkrc`文件配置自动调用 XeLaTeX 引擎进行编译，并生成最终的 PDF 文件 `sample.pdf`。

你也可以使用编辑器自带的编译功能（如 VS Code 的 LaTeX Workshop 插件）：按住 `Cmd/Ctrl + Shift + P` 打开命令面板，输入 `LaTeX Workshop: Build with recipe` 并选择编译选项 `latexmk(latexmkrc)` 进行编译。

> [!TIP]
> 你可以在项目根目录下创建 `.vscode/settings.json` 文件，并添加以下内容以指定默认的编译选项及自动编译时机：
> ```json
> {
>   "latex-workshop.latex.recipe.default": "latexmk (latexmkrc)",
>   "latex-workshop.latex.autoBuild.run": "onFileChange",
>   "latex-workshop.latex.autoBuild.interval": 300000,
>   "latex-workshop.latex.autoClean.run": "onSucceeded",
>   "latex-workshop.latex.rootFile.indicator": "\\begin{document}",
>   "latex-workshop.latex.clean.fileTypes": [
>     "%DOCFILE%.aux",
>     "%DOCFILE%.bbl",
>     "%DOCFILE%.blg",
>     "%DOCFILE%.idx",
>     "%DOCFILE%.ind",
>     "%DOCFILE%.lof",
>     "%DOCFILE%.lot",
>     "%DOCFILE%.out",
>     "%DOCFILE%.toc",
>     "%DOCFILE%.acn",
>     "%DOCFILE%.acr",
>     "%DOCFILE%.alg",
>     "%DOCFILE%.glg",
>     "%DOCFILE%.glo",
>     "%DOCFILE%.gls",
>     "%DOCFILE%.fls",
>     "%DOCFILE%.log",
>     "%DOCFILE%.fdb_latexmk",
>     "%DOCFILE%.snm",
>     "%DOCFILE%.synctex(busy)",
>     "%DOCFILE%.synctex.gz",
>     "%DOCFILE%.nav",
>     "%DOCFILE%.vrb"
>   ]
> }
> ```

### 大功告成

到这一步如果没有出现报错，你应该已经成功生成了论文的 PDF 文件 `sample.pdf`，可以在文件夹中找到并打开查看最终效果。

> [!TIP]
> 请在提交论文前仔细检查生成的 PDF 文件，确保所有内容均符合要求，包括图片、表格、参考文献等。
> - 如果发现问题，可以返回到相应的源文件进行修改，然后重新编译生成新的 PDF 文件。
> - 部分报错可以参考后续的[FAQs](#FAQs)部分进行解决，或者前往本仓库的 [Issues](https://github.com/Lucas04-nhr/HUST-GP-template/issues) 页面进行反馈。

## 协助编辑

如果你有更好的想法，欢迎 Fork 本仓库并提交 Pull Request，达到一定质量后将会被合并至主分支，并在[支持者名单](SUPPORTERS.md)中进行记录以示感谢。

## 问题反馈

如果在使用过程中遇到任何问题，欢迎前往本仓库的 [Issues](https://github.com/Lucas04-nhr/HUST-GP-template/issues) 页面进行反馈。

下面列出了一些常见的问题及其解决方法：

### FAQs

1. **代码块无法正确渲染高亮**  
   解决方法：请确保已经正确安装了 `pygments`，并且在 `.latexmkrc` 文件中正确配置了环境变量路径。
2. **行间公式无法正确编号**  
   解决方法：请确保使用的是 `equation` 环境来输入行间公式，而不是使用 `$$ ... $$` 语法。例如：
   ```tex
   \begin{equation}
     E = mc^2
   \end{equation}
   ```
3. **图片无法正确显示**
   解决方法：请确保图片文件路径正确，并且图片格式被支持（如 PNG、JPEG、PDF 等），推荐使用 PDF 格式以获得最佳效果。
4. **参考文献无法正确生成**  
   解决方法：确保已安装 BibTeX，并在编译时正确执行参考文献相关的步骤（使用 `latexmk` 可自动完成）。如遇 Windows 平台上 biber 或相关工具缺失的问题，请参阅 [Rapheal](https://github.com/chide-org) 提供的[Windows 简明教程（含 biber 问题修复）](./doc/wsl_biber_fix.md)获取针对性的解决方法。
5. **字体显示异常**  
   解决方法：请确保已经正确安装了所需的字体，并且在编译时使用了 XeLaTeX 引擎。
6. **无法通过编译**  
   解决方法：请仔细检查编译日志，查找具体的错误信息，并根据提示进行修正。关于缺少宏包的解决办法可参考[这里 (English only)](https://yihui.org/tinytex/#maintenance)。
7. **如何使用“参考文献译文本”或“开题报告”模板？**  
   解决方法：将 `misc/translation` 或者 `misc/proposal` 目录中的相关文件移动并覆盖根目录下的同名文件即可。请注意，“开题报告”相关模板仍在完善中，相关内容将在**不晚于第二学期开学前**进行补充。
8. **如何处理“参考文献译文本”中的参考文献条目？**  
   解决方法：根据学校要求，“参考文献译文本”需要将原始参考文献的参考文献条目一并插入文中，推荐采用 Scopus 进行批量导出 `.bib` 文件并通过正则表达式删去 `note` 字段，相关替换的正则表达式如下，以 VS Code 为例：
   ```regex
   ^\s*note\s*=\s*\{[^}]*\}\s*,?\r?\n?
   ```

>[!TIP]
> 除了 issue 等反馈渠道外，本人还酌情提供有偿的一对一支持服务，可以通过[邮件](https://go.lucas04.top/gp-support)进行联系，具体价格视服务内容而定，原则上不低于各个赞助平台的最高档位。服务内容包括但不限于：环境配置指导、论文排版问题解决等。
>
> > - 成为本项目的贡献者（**贡献代码/提供思路/给予赞助**）可以获得永久免费的支持服务机会，并且会在[支持者名单](SUPPORTERS.md)中进行鸣谢。
> > - 购买付费邮件支持的用户将**不会**被列入[支持者名单](SUPPORTERS.md)，请知悉。

## 投喂

如果你觉得这个模板对你有帮助，欢迎通过[此页面](https://github.com/Lucas04-nhr#-support-me)进行支持，另外，也欢迎点击 Star 来表示对本项目的认可。本人将不定期将达到一定金额的支持者名单更新在[支持者名单](SUPPORTERS.md)中，以表达感谢。
