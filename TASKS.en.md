# xlsx-organizer task index

Look up what you want to do: where to click in the GUI, what to type on the command line, and where to read more.

- Commands assume `xlsx-organizer` is on PATH (the installer's default); otherwise go to the folder holding `xlsx-organizer.exe` and use `.\xlsx-organizer`.
- Replace the file and column names with your own. A task with `-c config.yaml` needs rules: run `xlsx-organizer config init` first, then edit the section of that name in `config.yaml`, which carries comments in both languages.
- A "section" under *Read more* is a section of `config.yaml`.
- "Run → Clean" means click **Run** on the left, then the **Clean** tab.
- All twelve pipeline commands take `--dry-run` to preview first; `files`, `workflow run`, `history rollback` and `history retry` only preview unless you add `--apply`. Outputs never overwrite an existing file, and identical files are moved, never deleted.

| I want to… | GUI (where to click) | Command | Read more |
|---|---|---|---|
| Try it on sample files first | Home → Try a sample | `xlsx-organizer demo` | `xlsx-organizer demo --help` |
| Start from a config file | None, the GUI uses forms | `xlsx-organizer config init` | `xlsx-organizer config init --help` |
| List a file's sheets and columns | Run → Clean → Input check → Check inputs | `xlsx-organizer headers exports/orders.xlsx` | `xlsx-organizer headers --help` |
| Tidy the data inside workbooks | Run → Clean → Run clean | `xlsx-organizer clean --input exports --output cleaned` | `clean` section; `xlsx-organizer clean --help` |
| Classify and de-duplicate files, and rename them when `rename.enabled` is on (by default by the letters a file name starts with, the rest go to `_unmatched`) | Run → Organize files → Preview plan → Execute | Preview `xlsx-organizer files --input inbox --output organized`, run `xlsx-organizer files --input inbox --output organized --apply` | `files` section; `xlsx-organizer files --help` |
| Only pick out files with identical content | Home → Duplicate check | Set `strategy: none` in the section, then `xlsx-organizer -c config.yaml files --apply` | `files` section |
| Combine many files into one | Run → Merge → Run merge | `xlsx-organizer merge --input exports --output merged.xlsx` | `merge` section; `xlsx-organizer merge --help` |
| Merge every sheet of each file | Run → Merge, set the sheets to "All sheets (all)" → Run merge | `xlsx-organizer merge --input exports --output merged.xlsx --sheets all` | `merge` section |
| Join files on key columns | Run → Merge, set the merge mode to "Join on key columns (join)", fill "Join key column(s)" → Run merge | `xlsx-organizer merge --input exports --output joined.xlsx --join-on order_id` | `merge` section |
| Check data against rules | Run → Validate → Run validate | `xlsx-organizer -c config.yaml validate --strict` | `validate` section; `xlsx-organizer validate --help` |
| Export validation issues as a to-do list | Run → Validate → Run validate → Export to-do | `xlsx-organizer -c config.yaml validate --todo todo.xlsx` | `xlsx-organizer validate --help` |
| Summarize detail rows into a report | Run → Report → Run report | `xlsx-organizer -c config.yaml report` | `report` section; `xlsx-organizer report --help` |
| Compare two periods | Run → Compare → Run compare | `xlsx-organizer compare --base jan.xlsx --current feb.xlsx --key order_id --output diff.xlsx` | `compare` section; `xlsx-organizer compare --help` |
| Split a workbook into many files | Run → Split → Run split | `xlsx-organizer -c config.yaml split` | `split` section; `xlsx-organizer split --help` |
| See every column of an export at a glance | Run → Profile → Run profile | `xlsx-organizer profile --input exports --output profile.xlsx` | `profile` section; `xlsx-organizer profile --help` |
| Draft validate rules from a profile | Run → Profile, fill "Rules draft file (optional)" → Run profile | `xlsx-organizer profile --input exports --output profile.xlsx --suggest-rules rules-draft.yaml` | `xlsx-organizer profile --help` |
| Turn wide tables into long ones | Run → Unpivot → Run unpivot | `xlsx-organizer -c config.yaml unpivot` | `unpivot` section; `xlsx-organizer unpivot --help` |
| Mask sensitive columns before sharing | Run → Mask → Run mask | `xlsx-organizer -c config.yaml mask` | `mask` section; `xlsx-organizer mask --help` |
| Add columns from a reference table | Run → Enrich → Run enrich | `xlsx-organizer enrich --input exports --output enriched --ref customers.xlsx --key customer_id` | `enrich` section; `xlsx-organizer enrich --help` |
| Reconcile two sides (amount and tolerance go in the section) | Run → Reconcile → Run reconcile | `xlsx-organizer reconcile --left ledger.xlsx --right bank.xlsx --key voucher_no --output reconcile-result.xlsx` | `reconcile` section; `xlsx-organizer reconcile --help` |
| Try month close on sample data | Jobs → Month close → Install with sample data → Preview plan → Execute | `xlsx-organizer workflow install month-close --sample --name try-it`, then `xlsx-organizer workflow run try-it --apply` | `MONTH-CLOSE.md` |
| Run month close on your own data | Jobs → Month close → Install with placeholder paths → Preview plan → Execute | `xlsx-organizer workflow install month-close`, put your files in place, then `xlsx-organizer workflow run month-close --apply` | `MONTH-CLOSE.md` |
| Close another month | Jobs → Month close, fill "Period to close (YYYY-MM)" → Preview plan → Execute | `xlsx-organizer workflow run month-close --apply --param period=2026-09` | `MONTH-CLOSE.md` |
| Look up past runs | History | `xlsx-organizer history list` | `xlsx-organizer history --help` |
| Move files back after a `files` run | History → pick a run → Roll back | Preview `xlsx-organizer history rollback RUN_ID`, run `xlsx-organizer history rollback RUN_ID --apply` | `xlsx-organizer history rollback --help` |
| Redo the failed moves of a `files` run | History → pick a run → Retry failed | Preview `xlsx-organizer history retry RUN_ID`, run `xlsx-organizer history retry RUN_ID --apply` | `xlsx-organizer history retry --help` |
| Clear out old history records | History → Keep newest → Prune | `xlsx-organizer history prune --keep 20` | `xlsx-organizer history prune --help` |
| Run a workflow on a schedule | None, use Windows Task Scheduler | `xlsx-organizer workflow run month-close --apply --strict --report C:\reports\month-close.json` | `xlsx-organizer workflow run --help` |

On a schedule: in Task Scheduler set *Program/script* to the full path of `xlsx-organizer.exe`, *Add arguments* to the arguments from `workflow run` on (left blank, it opens the GUI), *Start in* to the working folder, and run it as the same user who installed the app. `--strict` makes a run that did not do its job exit non-zero. `--report` never overwrites, so a second run writes `month-close_1.json`. Do not schedule a workflow with a `files` step: `--apply` would keep moving your files.
