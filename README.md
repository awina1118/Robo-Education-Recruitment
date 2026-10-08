# Robo-Education-Recruitment

第 21 届浙大机协教学部纳新题作答仓库。主题：**STM32 嵌入式入门——从点灯到让小车跑起来**。

## 仓库内容

| 题目 | 内容 | 位置 |
| --- | --- | --- |
| 4.5.1 内训选题 | STM32 嵌入式入门内训方案（6 讲，每讲 2 小时） | [`topic/内训选题.md`](topic/内训选题.md) |
| 4.5.2 幻灯片制作 | 第 1 讲“GPIO：点亮第一盏灯”授课幻灯片，约 10 分钟 | [`slides/slides.tex`](slides/slides.tex)、[`slides/slides.pdf`](slides/slides.pdf) |
| 4.5.3 GitHub 协作 | 本 README | — |

## 文件结构

```
Robo-Education-Recruitment/
├── README.md
├── .gitignore          # 忽略 LaTeX 编译中间文件
├── topic/
│   └── 内训选题.md
├── slides/
│   ├── slides.tex      # LaTeX Beamer 源文件，XeLaTeX 编译
│   └── slides.pdf      # 导出的 PDF
└── assets/             # 图片等资源（幻灯片的图都用 TikZ 绘制，暂无外部图片）
```

幻灯片编译：`cd slides && xelatex slides.tex`（运行两次以生成目录）。

## 完成情况

- [x] 内训选题：课程定位、面向对象、学习目标、软硬件与经费、教学形式、6 讲课时安排、问题与对策
- [x] 幻灯片：13 页，含型号解读图、开发流程图、LED 电路图、示例代码与寄存器解释、常见错误
- [x] 幻灯片导出 PDF
- [x] README 问答

## 问答

### 1. 本次任务中使用了哪些 Git / GitHub 功能？

- `git init` / `git clone`：建立本地仓库，克隆远程仓库
- `git add`、`git commit`：按“内训选题初稿 → 幻灯片 → README”分阶段提交，每次提交只做一件事，提交信息写清楚改了什么
- `git status`、`git log`、`git diff`：提交前检查改动范围，确认没有把编译中间文件带进去
- `.gitignore`：忽略 `*.aux`、`*.log`、`*.synctex.gz` 等 LaTeX 中间文件
- `git remote`、`git push`：推送到 GitHub
- GitHub 网页：创建仓库、在线预览 Markdown 和 PDF

### 2. 操作过程中遇到了哪些问题？如何解决的？

| 问题 | 原因 | 解决 |
| --- | --- | --- |
| 幻灯片第一次编译报错 `translator.sty not found` | Beamer 依赖的 `translator` 宏包没装，MiKTeX 自动安装时网络报 SSL 错误 | 用 `miktex packages install translator` 手动安装后重新编译 |
| 提交时提示 `LF will be replaced by CRLF` | Windows 上 Git 默认 `core.autocrlf=true`，会转换换行符 | 了解后确认对 Markdown 和 LaTeX 文件没有影响；多人协作时可加 `.gitattributes` 统一换行符 |
| 编译后仓库里出现大量 `.aux`、`.log`、`.nav` 文件 | LaTeX 编译的中间产物 | 添加 `.gitignore`，并用 `git status` 确认只提交源文件和最终 PDF |
| 文件名含中文（`内训选题.md`）在 `git status` 里显示成 `\345\206\205...` | Git 默认对非 ASCII 路径转义 | `git config core.quotepath false` |

### 3. 如果多名教学部成员共同维护这个仓库，应当如何组织文件与修改流程？

**文件组织**

- 按课程分目录，每门课内部结构统一：

  ```
  courses/
  ├── stm32-intro/
  │   ├── README.md        # 课程简介、课时表、负责人
  │   ├── lecture01-gpio/
  │   │   ├── slides.tex / slides.pdf
  │   │   └── code/        # 示例工程
  │   └── lecture02-exti/
  └── arduino-anycar/
  ```

- 根目录 README 维护课程总表（课程名、负责人、状态、最后更新时间）。
- 命名统一用英文小写加连字符，避免中文路径和空格在不同系统上出问题。
- 大文件（视频、原始图片）不进仓库，放网盘，在 README 中给链接。

**修改流程**

1. `main` 分支受保护，不允许直接 push。
2. 每个人从 `main` 新建分支，分支名写清内容，如 `stm32/lecture03-pwm`。
3. 改完后发 Pull Request，至少一名其他成员审阅（检查内容正确性、能否编译、格式是否统一）后再合并。
4. 用 GitHub Issues 记录“某页课件有错”“某课需要更新”之类的任务，并指派负责人。
5. 每学期内训结束后打一个 tag（如 `2026-fall`），方便以后查当时用的版本。

### 4. 本次纳新题中是否使用了 AI 工具？AI 参与了哪些部分？你进行了哪些检查与修改？

使用了 AI。

- **参与部分**：搭建仓库目录结构；起草内训方案和幻灯片的内容与 LaTeX 代码；编译幻灯片。
- **检查与核对**：
  - 对照 STM32F103 数据手册核对型号含义、主频、Flash/RAM 大小
  - 核对蓝色小板原理图中 PC13 LED 的接法（低电平点亮）
  - 核对 BSRR 寄存器高低 16 位的置位/复位含义
  - 逐页查看编译出的 PDF，确认图表没有错位、中文显示正常
  - 估算 10 分钟讲完 13 页是否合理（平均每页约 45 秒，代码页更长）
