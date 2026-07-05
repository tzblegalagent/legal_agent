# Source Policy

Use this file before citing, searching, or making legal-authority claims.

## Source Hierarchy

| Source class | Tag | Treatment |
|---|---|---|
| Local official documents, statutes, regulations, standards, regulator materials | `[本地法规库]` | Strongest local authority. Still verify currency before final legal reliance. |
| Local cases, enforcement actions, judicial examples, regulator penalties | `[本地案例]` | Use as examples of enforcement, disputes, or fact patterns. Do not treat as statutes. |
| Local papers, reports, books, commentary, frameworks | `[本地论文/报告]` | Use for analytical support, frameworks, and comparative reasoning. Do not treat as binding. |
| User-provided facts, contracts, policies, screenshots, logs | `[用户材料]` | Use as factual inputs. Flag incompleteness and assumptions. |
| Model inference without source support | `[模型推理-待核验]` | Use only as provisional analysis. Add to verification list. |

## Mandatory Rules

- Do not use Pkulaw, 北大法宝, or a Pkulaw MCP as a source or runtime tool.
- Do not write "北大法宝已核验", "经北大法宝检索", or equivalent verification claims.
- Never state that a rule is current unless the source text or an explicit current-date verification supports it.
- Distinguish law, policy, standards, enforcement cases, and academic commentary.
- Quote sparingly. Prefer source-tagged paraphrase with path or file name when working from local corpus.
- When sources conflict, use the stricter operational recommendation and mark the conflict for human verification.

## Citation Format

Use compact source labels in risk matrices:

```text
[本地法规库: legal_preference_txt/官方文件/<file>.txt]
[本地案例: legal_preference_txt/案例/<file>.txt]
[本地论文/报告: legal_preference_txt/论文/<file>.txt]
[用户材料: <document or user statement>]
[模型推理-待核验]
```

If no source is available, write `[待核验: 缺少本地来源]` instead of inventing authority.
