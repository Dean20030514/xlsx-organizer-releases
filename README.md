# xlsx-organizer

**离线运行的 Excel / CSV 批量处理工具 · 中文 / English 双语 · Windows 免安装**
**An offline, bilingual Excel/CSV batch tool — your files never leave your machine.**

[**⬇ 下载最新版 / Download latest**](https://github.com/Dean20030514/xlsx-organizer-releases/releases/latest)
 · [**官网 / Website**](https://dean20030514.github.io/xlsx-organizer-releases/)
 · [**更新记录 / All releases**](https://github.com/Dean20030514/xlsx-organizer-releases/releases)
 · [**月结一页纸 / Month close**](MONTH-CLOSE.md)
 · [**按任务查**](TASKS.zh.md) / [**Find a task**](TASKS.en.md)

> 🇨🇳 **国内用户**：GitHub 下载慢或打不开时，请走
> [**Gitee 镜像**](https://gitee.com/Dean20030514/xlsx-organizer-releases/releases)（国内直连，无需代理）。
> 两边发布的是同一次构建的同一批文件，SHA256 完全一致，可互相校验。

---

## 中文

### 这是什么

xlsx-organizer 把「每个月都要手工重复一遍」的表格活儿变成一次点击：清洗、整理、合并、
核对、出报表、对账。它**不依赖本地 Excel**，也不用 Windows COM——纯 Python 实现，
双击 exe 就能打开图形界面，带参数则是命令行。

**数据完全不出本机。** 没有账号、没有云端、没有遥测；唯一的联网动作是你手动点击的
「检查更新」。

### 下载与安装

1. 到 [Releases 页面](https://github.com/Dean20030514/xlsx-organizer-releases/releases/latest)
   下载 `xlsx-organizer-setup-<版本>.exe`（安装包，推荐）或 `xlsx-organizer.exe`（免安装单文件）。
   要在终端里用免安装版，把同一页的 `xlsx-organizer.com` 和 exe 放在同一个文件夹。
2. **先核对文件**：算出的 SHA-256 要和同一发布页 `SHA256SUMS.txt` 里对应的那一行相同：

   ```powershell
   Get-FileHash .\xlsx-organizer-setup-*.exe -Algorithm SHA256
   ```

3. 安装包为**当前用户安装**，无需管理员权限；可选加入 PATH 与桌面快捷方式，开始菜单快捷方式总会创建。

> ⚠️ **Windows 提示与杀毒软件。** 当前版本**没有代码签名**，SmartScreen 可能提示「未知发布者」。
> 哈希相符后，点「更多信息 → 仍要运行」。如果 360、火绒等杀毒软件拦截了哈希相符的文件，
> 那是误报：可从隔离区恢复，并向该厂商或我们报告（deanymrq@gmail.com）。
> 发布说明里附有每个文件的 VirusTotal 和微步云沙箱报告链接。

**系统要求**：64 位 Windows。图形界面需要 WebView2 运行时——安装包会在缺失时自动引导安装。
macOS / Linux 可从源码构建（纯 Python，跨平台），请联系我们获取。

### 三步拿到第一个结果

都用内置的样本数据，不碰你自己的文件。

1. 打开 xlsx-organizer（开始菜单，或双击 `xlsx-organizer.exe`）。
2. 在主页点**试用示例**。
3. 它会对内置的杂乱数据运行一次真实的清洗；结果里会显示放着清洗后副本的输出文件夹。

喜欢用终端的话（PowerShell 或 cmd），`demo` 会新建一个文件夹，写入 13 个演示场景，
每个功能一个，外加月结作业；文件夹里的 `START-HERE.txt` 列出每一条命令：

```powershell
xlsx-organizer demo
cd xlsx-organizer-demo
xlsx-organizer --config clean.config.yaml clean --dry-run
xlsx-organizer --config clean.config.yaml clean
```

### 接下来

- **做月结**：[MONTH-CLOSE.md](MONTH-CLOSE.md)，一页讲完试跑、换成自己的数据、下个月怎么做。
  图形界面里在「内置作业」→「月结对账」。
- **想做某一件事**：[TASKS.zh.md](TASKS.zh.md) 按任务列出在界面里点哪儿、命令怎么写。
- **查配置字段**：`xlsx-organizer config init` 会写出一份带中英注释的配置文件；
  `xlsx-organizer headers <文件>` 列出文件的工作表和列名。

### 十二条流水线

| 管道 | 做什么 |
|---|---|
| `clean` | 清洗工作簿**内部**数据：删空行空列、去空格、归一表头、去重、排序、按列拆分 |
| `files` | 整理工作簿**文件**本身：按规则分类、批量改名、按内容哈希去重 |
| `merge` | 把多份导出合并成一个新工作簿（追加 / 分表 / 按键连接） |
| `validate` | 按数据质量规则校验（只读），支持跨文件核对，`--strict` 可用于 CI |
| `report` | 把多份导出聚合 / 透视成汇总报表 |
| `compare` | 按主键对比两期（或多期）工作簿，输出新增 / 删除 / 变更 |
| `split` | 把一个工作簿拆成多个文件（`merge` 的逆操作） |
| `profile` | 逐列数据画像（只读）：类型、缺失率、去重数、极值、样例 |
| `unpivot` | 把宽表还原成长表（`report` 透视的逆操作） |
| `mask` | 脱敏敏感列（手机 / 身份证 / 姓名），生成可对外分享的安全副本 |
| `enrich` | 按同名主键补充参照表的列——声明式的 VLOOKUP |
| `reconcile` | 两侧确定性对账（如台账 vs 银行流水），未匹配与存疑行单独列出 |

十二条流水线共用一套配置与日志，可以串成**工作流**，内置一个**月结作业**模板：
清洗台账 → 与银行流水对账 → 校验 → 对比上期 → 仅在有差异时导出异常表。

### 图形界面

双击 exe 即可打开本地 Web 图形界面（原生窗口）：卡片式首页、每条流水线一个运行页、
原生文件夹 / 文件选择器、无需写 YAML 的规则编辑器、能从真实文件里采样列名的
**读取表头**按钮、带回滚 / 回放的运行历史，以及中文 / English 一键切换。
移动文件的操作一律「预览 → 确认 → 执行」三步闸门。

### 完全免费

从 v0.10.0 起，xlsx-organizer **完全免费**：全部功能可用，没有文件数上限，
不需要许可证、账号或激活，个人和商业用途均可。它是免费软件，但**不开源**。
v0.9.0 及更早的版本仍有每次运行的文件数上限，请升级到最新版。

### 隐私

你的表格**从不离开本机**：没有遥测、没有后台联网、没有账号。唯一的网络请求是你手动
触发的「检查更新」（读取本仓库的最新发行版）与随后你确认的安装包下载。
详见[隐私政策](https://dean20030514.github.io/xlsx-organizer-releases/privacy.html)。

### 支持与条款

问题反馈：deanymrq@gmail.com ·
[使用条款 EULA](https://dean20030514.github.io/xlsx-organizer-releases/eula.html) ·
[支持页](https://dean20030514.github.io/xlsx-organizer-releases/support.html)

---

## English

### What it is

xlsx-organizer turns the spreadsheet chores you repeat every month — cleaning,
filing, merging, checking, reporting, reconciling — into one click. It does
**not** depend on a local Excel install or Windows COM: it is pure Python.
Double-click the exe for a GUI; pass arguments and it is a CLI.

**Your data never leaves your machine.** No account, no cloud, no telemetry —
the only network call is the update check you trigger yourself.

### Download

1. Grab `xlsx-organizer-setup-<version>.exe` (installer, recommended) or
   `xlsx-organizer.exe` (single-file, no install) from the
   [latest release](https://github.com/Dean20030514/xlsx-organizer-releases/releases/latest).
   To use the single file from a terminal, keep `xlsx-organizer.com` from the same
   page in the same folder.
2. **Check the file first**: its SHA-256 must equal the matching line of
   `SHA256SUMS.txt` from the same release:

   ```powershell
   Get-FileHash .\xlsx-organizer-setup-*.exe -Algorithm SHA256
   ```

3. The installer is **per-user** — no admin rights needed. A PATH entry and a
   desktop shortcut are optional tasks; the Start-menu shortcut is always created.

> ⚠️ **Windows warnings and antivirus.** Builds are **not code-signed**, so
> SmartScreen may warn about an unknown publisher. Once the hash matches, choose
> *More info → Run anyway*. If 360, Huorong or another antivirus flags a file whose
> hash matches, it is a false positive: restore it from quarantine and report it to
> the vendor or to us (deanymrq@gmail.com). The release notes link each file's
> VirusTotal and ThreatBook reports.

**Requirements:** 64-bit Windows. The GUI needs the WebView2 runtime — the
installer bootstraps it when missing. macOS / Linux can build from source
(pure Python, cross-platform); contact us for a build.

### A first result in three steps

These use built-in sample data and never touch your own files.

1. Open xlsx-organizer (Start menu, or double-click `xlsx-organizer.exe`).
2. On the home screen, click **Try a sample**.
3. It runs a real clean over built-in messy data; the result shows the output folder
   holding the tidied copies.

In a terminal (PowerShell or cmd), `demo` writes 13 scenarios into a new folder, one
per feature plus the month-close job; its `START-HERE.txt` lists every command:

```powershell
xlsx-organizer demo
cd xlsx-organizer-demo
xlsx-organizer --config clean.config.yaml clean --dry-run
xlsx-organizer --config clean.config.yaml clean
```

### Next

- **Close the books**: [MONTH-CLOSE.md](MONTH-CLOSE.md) is one page: try it, use your
  own data, do next month. In the GUI: **Jobs** → **Month close**.
- **Do one particular thing**: [TASKS.en.md](TASKS.en.md) lists, by task, where to
  click and what to type.
- **Look up a config field**: `xlsx-organizer config init` writes a commented config
  file; `xlsx-organizer headers <file>` lists a file's sheets and columns.

### Twelve pipelines

| Pipeline | What it does |
|---|---|
| `clean` | Tidy data *inside* workbooks: drop empty rows/cols, trim, normalize headers, dedup, sort, split by column |
| `files` | Organize the workbook *files*: rule-based classify, batch rename, content-hash dedup |
| `merge` | Combine many exports into one new workbook (append / sheets / join) |
| `validate` | Check workbooks against data-quality rules (read-only), incl. cross-file reconciliation; `--strict` for CI |
| `report` | Aggregate / pivot many exports into a summary report |
| `compare` | Diff two (or N) periods by key → added / removed / changed |
| `split` | Divide a workbook into many files (the inverse of `merge`) |
| `profile` | Per-column data profiling (read-only): type, missing, distinct, min/max/mean, samples |
| `unpivot` | Melt wide tables into long ones (the inverse of `report`'s pivot) |
| `mask` | Mask sensitive columns (phone / ID / name) into safe copies for sharing |
| `enrich` | Add reference-table columns by shared key — the declarative VLOOKUP |
| `reconcile` | Deterministic two-sided matching (ledger vs bank statement); unmatched and ambiguous rows listed for follow-up |

All twelve share one config and logging mechanism and can be chained into
**workflows**. A built-in **month-close** template ships with the app: clean the
ledger → reconcile it against the bank statement → validate → compare against last
period → export an exceptions summary only when a discrepancy remains.

### Web GUI

Double-click the exe for a local web GUI in a native window: a card home screen, one
run tab per pipeline, native folder / file pickers, no-code rule editors, a
**Read headers** button that samples column names from your real files, run history
with rollback / replay, and a 中文 / English toggle. File moves keep a
preview → confirm → execute gate.

### Completely free

From v0.10.0 on, xlsx-organizer is **completely free**: every feature, no
file-count limit, no license key, account or activation, for personal and
commercial use. It is freeware, **not open source**. v0.9.0 and earlier still
cap the files per run — please update to the latest release.

### Privacy

Your spreadsheets never leave your machine: no telemetry, no background network
access, no account. The only requests are the update check you trigger manually
(it reads this repository's latest release) and the installer download you then
confirm. See the
[Privacy Policy](https://dean20030514.github.io/xlsx-organizer-releases/privacy.html).

### Support & terms

Questions: deanymrq@gmail.com ·
[EULA](https://dean20030514.github.io/xlsx-organizer-releases/eula.html) ·
[Support](https://dean20030514.github.io/xlsx-organizer-releases/support.html)

---

## About this repository / 关于本仓库

This repository hosts **release artifacts and the public website only** — the
source repository is private. xlsx-organizer is free proprietary software
(freeware, not open source). Bundled third-party dependencies (`pandas`,
`numpy`, `openpyxl`, `xlrd`, `click`, `pyyaml`) keep their own open-source
licenses.

本仓库只托管**发行包与官网页面**，源码仓库为私有。xlsx-organizer 是免费软件，
但不开源；捆绑的第三方依赖各自保留其开源许可证。
