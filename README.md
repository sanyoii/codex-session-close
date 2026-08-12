# Codex Session Close Skill

一個 **Only for Codex** 的收工與交接 Skill。它會在工作結束、context 需要切換，或 New Session 接手前，重新核對證據、區分完成／未完成／未驗證狀態，並在必要時留下可執行的 handoff。

## 用途與邊界

這個 Skill 適用於：

- 「收工」、「結束 session」、「寫交接」、「切到 new session」
- `session close`、`wrap up`、`evidence check`、`handoff`
- 工作未完成、context 過長，或需要讓下一個 Codex task 繼續處理

這個 Skill **不適用於**：

- browser/login session、cookie 或 OS shutdown 等技術語境
- 一般摘要或單純進度報告
- 未經授權寫入 Memory、修改 `AGENTS.md`、commit、push、建立或封存 task
- Claude、Claude Code 或 Cowork；本版本依 Codex 的 tool surface、sandbox 與授權模型設計

## 安裝

### 使用 Codex Skill Installer

請 Codex 安裝：

```text
Install the session-close skill from https://github.com/sanyoii/codex-session-close/tree/main/session-close
```

### 手動安裝

把 repository 內的 `session-close/` 資料夾複製到：

```text
$CODEX_HOME/skills/session-close
```

若未設定 `CODEX_HOME`，預設位置通常是：

```text
~/.codex/skills/session-close
```

安裝完成後重新啟動 Codex，讓 Skill 清單重新載入。

## 使用方式

可以直接用自然語言觸發：

```text
收工，請核對目前狀態並在必要時寫交接。
```

```text
/session-close --handoff
```

```text
/session-close --handoff --memory；把這次可重用的 root cause 寫入 Memory。
```

`--memory` 只代表授權依目前 Codex Memory 規則處理；Skill 仍會先查重，並遵守可寫入路徑。單獨使用 `/session-close` 不會自動取得 Memory、Git 或 task 管理權限。

## Repository 結構

```text
codex-session-close/
├─ session-close/
│  ├─ SKILL.md
│  └─ agents/
│     └─ openai.yaml
└─ evals/
   └─ evals.json
```

只有 `session-close/` 是需要安裝的 Skill。`evals/` 保存正向、邊界與反觸發案例，不是 runtime 依賴。

## 驗證

使用 Codex 內建 `skill-creator` validator：

PowerShell：

```powershell
$env:PYTHONUTF8 = "1"
python "$env:USERPROFILE\.codex\skills\.system\skill-creator\scripts\quick_validate.py" .\session-close
```

macOS / Linux：

```bash
PYTHONUTF8=1 python "$HOME/.codex/skills/.system/skill-creator/scripts/quick_validate.py" ./session-close
```

預期結果：

```text
Skill is valid!
```

## 隱私與安全

- 不要把 token、cookie、secret、私人 Memory、真實 handoff 或個人路徑加入 repository。
- `.backup/`、暫存檔與本機測試產物不屬於發布內容。
- Skill 會保留 dirty worktree，不會因為「收工」自行 reset、stash、commit、push 或刪除檔案。

## License

[MIT](LICENSE)
