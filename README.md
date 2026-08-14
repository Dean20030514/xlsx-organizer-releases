# xlsx-organizer

**离线运行的 Excel / CSV 批量处理工具 · 中文 / English 双语 · Windows 免安装**
**An offline, bilingual Excel/CSV batch tool — your files never leave your machine.**

[**⬇ 下载最新版 / Download latest**](https://github.com/Dean20030514/xlsx-organizer-releases/releases/latest)
 · [**官网 / Website**](https://dean20030514.github.io/xlsx-organizer-releases/)
 · [**更新记录 / All releases**](https://github.com/Dean20030514/xlsx-organizer-releases/releases)

<!--
国内镜像占位：Gitee 仓库建好后，把下面整段取消注释、把 <owner> 换成真实账号名，
并删除本条 HTML 注释。渲染前它对访客完全不可见，所以现在推送也不会露出半成品。

> 🇨🇳 **国内用户**：GitHub 下载不稳定时请走
> [Gitee 镜像](https://gitee.com/<owner>/xlsx-organizer-releases/releases)（国内直连，无需代理），
> 或访问 [Gitee Pages 站点](https://<owner>.gitee.io/xlsx-organizer-releases/)。
> 两边发布同一个安装包，SHA256 一致，可互相校验。
-->

---

## 中文

### 这是什么

xlsx-organizer 把「每个月都要手工重复一遍」的表格活儿变成一次点击：清洗、整理、合并、
核对、出报表、对账。它**不依赖本地 Excel**，也不用 Windows COM——纯 Python 实现，
双击 exe 就能打开图形界面，带参数则是命令行。

**数据完全不出本机。** 没有账号、没有云端、没有遥测；唯一的联网动作是你手动点击的
「检查更新」。

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

十二条流水线共用一套配置与日志，可以串成**多步作业**（配方 / 工作流），内置一个
**月结作业**模板：清洗台账 → 与银行流水对账 → 校验 → 对比上期 → 仅在有差异时导出异常表。

### 图形界面

双击 exe 即可打开本地 Web 图形界面（原生窗口）：卡片式首页、每条流水线一个运行页、
原生文件夹 / 文件选择器、无需写 YAML 的规则编辑器、能从真实文件里采样列名的
**读取表头**按钮、带回滚 / 重放的运行历史，以及中文 / English 一键切换。
移动文件的操作一律「预览 → 确认 → 执行」三步闸门。

### 下载与安装

1. 到 [Releases 页面](https://github.com/Dean20030514/xlsx-organizer-releases/releases/latest)
   下载 `xlsx-organizer-setup-<版本>.exe`（安装包，推荐）或 `xlsx-organizer.exe`（免安装单文件）。
2. **校验完整性**（可选但建议）——同一版本的 `SHA256SUMS.txt` 里有官方哈希：

   ```powershell
   Get-FileHash xlsx-organizer-setup-<版本>.exe -Algorithm SHA256
   ```

3. 安装包为**当前用户安装**，无需管理员权限；可选加入 PATH 与开始菜单快捷方式。

> ⚠️ 当前版本**尚未购买代码签名证书**，Windows SmartScreen 可能提示「未知发布者」。
> 请点击「更多信息 → 仍要运行」，并用上面的 SHA256 核对文件确属官方发布。

**系统要求**：64 位 Windows。图形界面需要 WebView2 运行时——安装包会在缺失时自动引导安装。
macOS / Linux 可从源码构建（纯 Python，跨平台），请联系我们获取。

### 免费版 vs Pro

每个管道免费版都能跑。多数管道每次运行最多处理 20 个文件，mask / enrich 为 5 个，
compare 为 2 个（两期对比免费，对比 3 期及以上需要 Pro），reconcile 为两侧合计
2 个源文件（单文件对单文件免费，文件夹批量需要 Pro）；Pro 解除全部上限。
许可是离线签名文件，**无需账号或激活服务器**，绑定到你注册的机器。

### 隐私

你的表格**从不离开本机**：没有遥测、没有后台联网、没有账号。唯一的网络请求是你手动
触发的「检查更新」（读取本仓库的最新发行版）与随后你确认的安装包下载。
详见[隐私政策](https://dean20030514.github.io/xlsx-organizer-releases/privacy.html)。

### 支持与条款

问题反馈、购买咨询：deanymrq@gmail.com ·
[使用条款 EULA](https://dean20030514.github.io/xlsx-organizer-releases/eula.html) ·
[退款政策](https://dean20030514.github.io/xlsx-organizer-releases/refund.html) ·
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
**multi-step jobs** (recipes / workflows). A built-in **month-close** template
ships with the app: clean the ledger → reconcile it against the bank statement →
validate → compare against last period → export an exceptions summary only when
a discrepancy remains.

### Download

1. Grab `xlsx-organizer-setup-<version>.exe` (installer, recommended) or
   `xlsx-organizer.exe` (single-file, no install) from the
   [latest release](https://github.com/Dean20030514/xlsx-organizer-releases/releases/latest).
2. **Verify the download** against `SHA256SUMS.txt` from the same release:

   ```powershell
   Get-FileHash xlsx-organizer-setup-<version>.exe -Algorithm SHA256
   ```

3. The installer is **per-user** — no admin rights needed. PATH entry and
   Start-menu shortcut are optional tasks.

> ⚠️ Builds are currently **unsigned** (no code-signing certificate yet), so
> Windows SmartScreen may warn about an unknown publisher. Choose
> *More info → Run anyway*, and use the SHA256 above to confirm authenticity.

**Requirements:** 64-bit Windows. The GUI needs the WebView2 runtime — the
installer bootstraps it when missing. macOS / Linux can build from source
(pure Python, cross-platform); contact us for a build.

### Free vs Pro

Every pipeline runs on Free. Most pipelines cap each run at 20 files, mask /
enrich at 5, compare at 2 (a two-period compare is free; comparing 3 or more
periods needs Pro), and reconcile at 2 source files across both sides (file vs
file is free; folder batches need Pro); Pro removes every cap. The license is an
offline signed file — **no account, no activation server** — node-locked to the
machine you register.

### Privacy

Your spreadsheets never leave your machine: no telemetry, no background network
access, no account. The only requests are the update check you trigger manually
(it reads this repository's latest release) and the installer download you then
confirm. See the
[Privacy Policy](https://dean20030514.github.io/xlsx-organizer-releases/privacy.html).

### Support & terms

Questions and purchases: deanymrq@gmail.com ·
[EULA](https://dean20030514.github.io/xlsx-organizer-releases/eula.html) ·
[Refund policy](https://dean20030514.github.io/xlsx-organizer-releases/refund.html) ·
[Support](https://dean20030514.github.io/xlsx-organizer-releases/support.html)

---

## About this repository / 关于本仓库

This repository hosts **release artifacts and the public website only** — the
source repository is private. xlsx-organizer is proprietary commercial software:
it is licensed, not sold. Bundled third-party dependencies (`pandas`,
`openpyxl`, `click`, `pyyaml`, `cryptography`) keep their own open-source
licenses.

本仓库只托管**发行包与官网页面**，源码仓库为私有。xlsx-organizer 是商业软件，
授权使用而非出售；捆绑的第三方依赖各自保留其开源许可证。
