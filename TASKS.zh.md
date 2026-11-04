# xlsx-organizer 任务索引

按要做的事查：图形界面点哪里，命令行敲什么，再往下看哪里。

- 命令假定 `xlsx-organizer` 已在 PATH 上（安装包默认勾选）；否则到 `xlsx-organizer.exe` 所在文件夹，用 `.\xlsx-organizer`。
- 文件名和列名换成你自己的。带 `-c config.yaml` 的任务要写规则：先运行 `xlsx-organizer config init`，再改 `config.yaml` 里的同名小节，文件里带中英文注释。
- 「详见」里的小节就是 `config.yaml` 里的小节。
- 「运行 → 清洗」表示先点左侧的「运行」，再点「清洗」页签。
- 十二个功能命令都可以加 `--dry-run` 先预览；`files`、`workflow run`、`history rollback`、`history retry` 不加 `--apply` 就只预览。输出绝不覆盖已有文件，内容相同的文件只移动、不删除。

| 我想… | 界面（在哪点） | 命令 | 详见 |
|---|---|---|---|
| 用示例文件先试一遍 | 无，只有命令行 | `xlsx-organizer demo` | `xlsx-organizer demo --help` |
| 写一份配置文件起步 | 无，界面用表单填写 | `xlsx-organizer config init` | `xlsx-organizer config init --help` |
| 查文件的工作表和列名 | 运行 → 清洗 → 读取表头 | `xlsx-organizer headers 导出/订单.xlsx` | `xlsx-organizer headers --help` |
| 清洗工作簿内部的数据 | 运行 → 清洗 → 运行清洗 | `xlsx-organizer clean --input 导出 --output 清洗后` | `clean` 小节；`xlsx-organizer clean --help` |
| 把文件分类、改名、去重（默认按文件名开头的英文字母分类，其余进 `_unmatched`） | 运行 → 文件整理 → 预览计划 → 执行 | 预览 `xlsx-organizer files --input 收件箱 --output 已整理`，执行 `xlsx-organizer files --input 收件箱 --output 已整理 --apply` | `files` 小节；`xlsx-organizer files --help` |
| 只挑出内容重复的文件 | 主页 → 重复文件检查 | 小节里设 `strategy: none`，再运行 `xlsx-organizer -c config.yaml files --apply` | `files` 小节 |
| 把多个文件合并成一个 | 运行 → 合并 → 执行合并 | `xlsx-organizer merge --input 导出 --output 合并.xlsx` | `merge` 小节；`xlsx-organizer merge --help` |
| 合并时读每个文件的全部工作表 | 运行 → 合并，工作表选「全部工作表（all）」→ 执行合并 | `xlsx-organizer merge --input 导出 --output 合并.xlsx --sheets all` | `merge` 小节 |
| 按键列连接多个文件 | 运行 → 合并，合并模式选「按键列连接（join）」→ 执行合并 | `xlsx-organizer merge --input 导出 --output 连接.xlsx --join-on 订单号` | `merge` 小节 |
| 按规则校验数据 | 运行 → 校验 → 运行校验 | `xlsx-organizer -c config.yaml validate --strict` | `validate` 小节；`xlsx-organizer validate --help` |
| 把校验问题导出成待办表 | 运行 → 校验 → 运行校验 → 导出待办 | `xlsx-organizer -c config.yaml validate --todo 待办.xlsx` | `xlsx-organizer validate --help` |
| 把明细汇总成报表 | 运行 → 汇总 → 运行汇总 | `xlsx-organizer -c config.yaml report` | `report` 小节；`xlsx-organizer report --help` |
| 对比两期数据 | 运行 → 对比 → 运行对比 | `xlsx-organizer compare --base 1月.xlsx --current 2月.xlsx --key 订单号 --output 差异.xlsx` | `compare` 小节；`xlsx-organizer compare --help` |
| 把工作簿拆成多个文件 | 运行 → 拆分 → 运行拆分 | `xlsx-organizer -c config.yaml split` | `split` 小节；`xlsx-organizer split --help` |
| 看一份导出每列的概况 | 运行 → 概况 → 运行概况 | `xlsx-organizer profile --input 导出 --output 概况.xlsx` | `profile` 小节；`xlsx-organizer profile --help` |
| 由概况生成校验规则草稿 | 运行 → 概况，填「规则草稿文件（可选）」→ 运行概况 | `xlsx-organizer profile --input 导出 --output 概况.xlsx --suggest-rules 规则草稿.yaml` | `xlsx-organizer profile --help` |
| 把宽表转成长表 | 运行 → 逆透视 → 运行逆透视 | `xlsx-organizer -c config.yaml unpivot` | `unpivot` 小节；`xlsx-organizer unpivot --help` |
| 脱敏后再外发 | 运行 → 脱敏 → 运行脱敏 | `xlsx-organizer -c config.yaml mask` | `mask` 小节；`xlsx-organizer mask --help` |
| 按键列从参照表补列 | 运行 → 补列 → 运行补列 | `xlsx-organizer enrich --input 导出 --output 已补列 --ref 客户.xlsx --key 客户编号` | `enrich` 小节；`xlsx-organizer enrich --help` |
| 两侧对账（金额和容差写在小节里） | 运行 → 对账 → 运行对账 | `xlsx-organizer reconcile --left 台账.xlsx --right 流水.xlsx --key 凭证号 --output 对账结果.xlsx` | `reconcile` 小节；`xlsx-organizer reconcile --help` |
| 用示例数据试月结 | 内置作业 → 月结对账 → 用示例数据安装 → 预览计划 → 执行 | `xlsx-organizer workflow install month-close --sample --name try-it`，再 `xlsx-organizer workflow run try-it --apply` | `MONTH-CLOSE.md` |
| 用自己的数据做月结 | 内置作业 → 月结对账 → 用占位路径安装 → 预览计划 → 执行 | `xlsx-organizer workflow install month-close`，放好文件后 `xlsx-organizer workflow run month-close --apply` | `MONTH-CLOSE.md` |
| 补结其他月份 | 内置作业 → 月结对账，填「结账期间（YYYY-MM）」→ 预览计划 → 执行 | `xlsx-organizer workflow run month-close --apply --param period=2026-09` | `MONTH-CLOSE.md` |
| 查过去的运行 | 历史 | `xlsx-organizer history list` | `xlsx-organizer history --help` |
| 把 `files` 移动过的文件移回去 | 历史 → 选一条 → 回滚 | 预览 `xlsx-organizer history rollback 运行ID`，执行 `xlsx-organizer history rollback 运行ID --apply` | `xlsx-organizer history rollback --help` |
| 重做 `files` 里失败的移动 | 历史 → 选一条 → 重试失败项 | 预览 `xlsx-organizer history retry 运行ID`，执行 `xlsx-organizer history retry 运行ID --apply` | `xlsx-organizer history retry --help` |
| 清理旧的历史记录 | 历史 → 保留最新 → 清理 | `xlsx-organizer history prune --keep 20` | `xlsx-organizer history prune --help` |
| 定时运行工作流 | 无，用 Windows 任务计划程序 | `xlsx-organizer workflow run month-close --apply --strict --report C:\reports\month-close.json` | `xlsx-organizer workflow run --help` |

定时运行：任务计划程序的「程序或脚本」填 `xlsx-organizer.exe` 的完整路径，「添加参数」填 `workflow run` 开始的这串参数（留空会打开图形界面），「起始于」填工作目录，并用安装本软件的同一个用户运行。`--strict` 让未达成目的的运行返回非零退出码。`--report` 不覆盖已有文件，第二次运行写成 `month-close_1.json`。含 `files` 步骤的工作流不要定时运行，`--apply` 会一直移动你的文件。
