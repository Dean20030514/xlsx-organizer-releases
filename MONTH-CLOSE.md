# 月结对账一页上手 / Month close in one page

台账对银行流水、核对本期数据、和上期比较，一条命令跑完。全程离线，原始文件只读。
A ledger against a bank statement, checks, and a comparison with last period, in one
command. Offline; your input files are only ever read.

中文在前，English below.

## 先试一下（1 分钟）

```bash
xlsx-organizer workflow install month-close --sample --name try-it
xlsx-organizer workflow run try-it --apply
```

命令结尾会列出写出的文件。`reconcile-result.xlsx` 有五张表：`summary`、`matched`、
`left_unmatched`、`right_unmatched`、`ambiguous`；示例数据故意留了差异，所以还会多一份
`exceptions-summary.xlsx`。不带 `--apply` 就是预览，什么都不写。

## 换成自己的数据

```bash
xlsx-organizer workflow install month-close
```

把三个文件放到当前文件夹下的这些位置：

| 文件 | 位置 |
|---|---|
| 本期台账 | `month-close/ledger/ledger-current.xlsx` |
| 本期银行流水 | `month-close/bank/bank-current.xlsx` |
| 上期台账 | `month-close/prior/ledger-prior.xlsx` |

作业默认按 `voucher_no`（凭证号）配对、`amount`（金额）核对。列名不同，就改成你的：

```bash
xlsx-organizer workflow export month-close job.yaml
```

打开 `job.yaml`，改 `keys`、`amount`、`required_columns`、`unique_keys`、`non_negative`
里的列名，再导回去：

```bash
xlsx-organizer workflow import job.yaml --force
```

银行流水是收入、支出两列时，在 `reconcile_month` 这一步的 `right` 下加上：

```yaml
right:
  file: ./month-close/bank/bank-current.xlsx
  amount: { in_column: 收入金额, out_column: 支出金额 }
```

只有一列金额加一列方向时，写 `amount: { column: 金额, direction_column: 收支, in_labels: [收入], out_labels: [支出] }`。
台账和流水方向相反时，再加 `flip_sign: true`。无法判断方向的行不会被猜测，只会列在未匹配表里。

## 跑一次月结

```bash
xlsx-organizer workflow run month-close
xlsx-organizer workflow run month-close --apply
```

结果写到 `month-close/output/<期间>/`，期间默认是上个月。

预览时，对账、校验和例外汇总三步会显示“等待前序步骤”：它们要读前面步骤的产物，而预览不写文件。正式运行时它们会读到。

## 下个月

用新导出覆盖台账和流水，把上月台账放到 `month-close/prior/ledger-prior.xlsx`，再跑一次。
每一期有自己的文件夹，互不影响。要补结其他月份：

```bash
xlsx-organizer workflow run month-close --param period=2026-09 --apply
```

期间要写成 `YYYY-MM`：`2026-7` 这样的值会被拒绝，什么都不写，所以不会多出一个 7 月的文件夹。

图形界面里同样可用：打开“内置作业”，选“月结对账”，页面上有期间输入框，也会显示上次运行的结果。

---

## Try it (1 minute)

```bash
xlsx-organizer workflow install month-close --sample --name try-it
xlsx-organizer workflow run try-it --apply
```

The command ends by listing what it wrote. `reconcile-result.xlsx` has five sheets:
`summary`, `matched`, `left_unmatched`, `right_unmatched` and `ambiguous`; the sample
holds differences on purpose, so `exceptions-summary.xlsx` appears too. Without
`--apply` it is a preview that writes nothing.

## Use your own data

```bash
xlsx-organizer workflow install month-close
```

Put three files in these places under the current folder:

| File | Place |
|---|---|
| This period's ledger | `month-close/ledger/ledger-current.xlsx` |
| This period's bank statement | `month-close/bank/bank-current.xlsx` |
| Last period's ledger | `month-close/prior/ledger-prior.xlsx` |

The job pairs on `voucher_no` and checks `amount`. If your headers differ:

```bash
xlsx-organizer workflow export month-close job.yaml
```

In `job.yaml`, change the column names under `keys`, `amount`, `required_columns`,
`unique_keys` and `non_negative`, then bring it back:

```bash
xlsx-organizer workflow import job.yaml --force
```

If the bank statement has money-in and money-out columns, add this under `right` in
the `reconcile_month` step:

```yaml
right:
  file: ./month-close/bank/bank-current.xlsx
  amount: { in_column: Money in, out_column: Money out }
```

For one amount column plus a direction column, write
`amount: { column: Amount, direction_column: Type, in_labels: [CR], out_labels: [DR] }`.
If the two sides count the same money in opposite directions, add `flip_sign: true`.
A row whose direction cannot be told is never guessed: it is listed as unmatched.

## Run a close

```bash
xlsx-organizer workflow run month-close
xlsx-organizer workflow run month-close --apply
```

Results go to `month-close/output/<period>/`. The period defaults to last month.

In the preview, the reconcile, validate and exceptions steps show as waiting for an
earlier step: they read what earlier steps write, and a preview writes nothing. A real
run reads it.

## Next month

Overwrite the ledger and the statement with the new exports, put last month's ledger
at `month-close/prior/ledger-prior.xlsx`, and run it again. Each period has its own
folder. To close another month:

```bash
xlsx-organizer workflow run month-close --param period=2026-09 --apply
```

The period is written `YYYY-MM`: a value such as `2026-7` is refused and nothing is
written, so no second folder for July appears.

The Web GUI does the same: open Jobs, choose Month close; the page has a period field
and shows what the last run did.
