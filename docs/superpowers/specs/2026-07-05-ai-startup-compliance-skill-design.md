# AI Startup Compliance Skill Design

## Goal

Create a Codex skill for AI startup compliance review in the science and innovation competition project. The skill should help review business scenarios for AI companies, classify compliance risks, retrieve supporting materials from the local legal corpus, and produce a practical review path with evidence, remediation actions, and residual risk notes.

The competition theme emphasizes risk identification and management for technology enterprises, especially AI companies with high uncertainty, fast iteration, data compliance pressure, AI ethics issues, intellectual property exposure, cybersecurity risk, and overseas expansion risk. The skill should support a demonstrable workflow artifact, not just a written research memo.

## Non-Goals

- Do not use the Pkulaw / 北大法宝 MCP in the implementation or runtime path.
- Do not claim that legal citations have been automatically verified by 北大法宝.
- Do not put every scenario prompt, checklist, and legal framework into one large `SKILL.md`.
- Do not build a generic legal Q&A skill. The skill is scoped to AI startup compliance review workflows.
- Do not provide final legal opinions. Outputs are compliance research and review work product requiring human verification.

## Core Constraints

### Pkulaw MCP Constraint

北大法宝 MCP can be mentioned only as a reference design for what legal retrieval capabilities look like, such as semantic law search, article lookup, and compliance data retrieval. It must not be used as a dependency.

The project implementation should rely on:

- local corpus files under `legal_preference_txt/`;
- manually structured compliance rules;
- source tags and verification status;
- optional local retrieval or embedding search if implemented later.

The output source labels should distinguish:

- `[本地法规库]` for local law or official-document text;
- `[本地案例]` for local case materials;
- `[本地论文/报告]` for academic or industry references;
- `[用户材料]` for user-supplied facts and documents;
- `[模型推理-待核验]` for model-derived statements that need human verification.

### Progressive Disclosure Constraint

`SKILL.md` should be a router and orchestration guide only. It should stay concise and load scenario or stage references only when needed.

Detailed prompts, checklists, scoring rubrics, legal framework summaries, and output templates should live in `references/`, split by stage, scenario, and domain. A typical review should load only the small set of reference files relevant to the current matter.

## Proposed Skill Name

Use:

```text
ai-startup-compliance-review
```

Chinese display name:

```text
AI 初创公司合规审查
```

The frontmatter description should trigger on requests such as:

- AI 公司合规审查
- AI 产品上线审查
- 训练数据合规
- 算法/大模型合规风险
- AI 出海合规
- AI 供应商合同审查
- 科创企业风险识别
- 融资尽调中的数据/IP/模型风险

## Skill Folder Structure

```text
ai-startup-compliance-review/
├── SKILL.md
├── references/
│   ├── 00-source-policy.md
│   ├── 01-intake-router.md
│   ├── 02-risk-scoring.md
│   ├── 03-output-template.md
│   ├── stage-intake.md
│   ├── stage-retrieval.md
│   ├── stage-risk-classification.md
│   ├── stage-remediation.md
│   ├── scenario-financing.md
│   ├── scenario-data-processing.md
│   ├── scenario-product-launch.md
│   ├── scenario-cross-border.md
│   ├── scenario-vendor-contract.md
│   ├── domain-ai-governance.md
│   ├── domain-data-compliance.md
│   ├── domain-ip.md
│   ├── domain-cybersecurity.md
│   └── domain-consumer-marketing.md
└── scripts/
    ├── score_risk.py
    └── check_report_structure.py
```

Only create `scripts/` when deterministic scoring or report validation is actually implemented. The first useful version can be references-only.

## SKILL.md Responsibilities

`SKILL.md` should contain:

1. Frontmatter with name and a comprehensive trigger description.
2. A short purpose statement.
3. The universal workflow:
   - classify the input scenario;
   - load only the matching scenario file;
   - load required stage files;
   - load relevant domain files;
   - use local corpus source policy;
   - produce a structured report.
4. A routing table from scenario to reference files.
5. Guardrails:
   - do not use 北大法宝 MCP;
   - do not make unverified legal authority claims;
   - preserve source tags;
   - distinguish user facts from retrieved sources and model reasoning;
   - high-risk conclusions require factual trigger, authority or source, and remediation action.

`SKILL.md` should not contain the full review checklist for every scenario.

## Reference File Roles

### `00-source-policy.md`

Defines source hierarchy, citation tags, local corpus use, and verification language.

Required rules:

- Treat `legal_preference_txt/官方文件` as the strongest local source.
- Treat `legal_preference_txt/案例` as enforcement or dispute examples, not statutes.
- Treat `legal_preference_txt/论文` and `legal_preference_txt/书籍` as analysis support, not binding authority.
- Mark any model-memory legal statement as `[模型推理-待核验]`.
- Never state that a citation is current unless the source text itself supports currency.

### `01-intake-router.md`

Maps user input to one or more scenarios.

Primary scenarios:

- financing due diligence;
- data processing or training data use;
- product launch;
- cross-border expansion;
- vendor or model contract review.

If multiple scenarios apply, load the dominant scenario first and add cross-scenario checks only where facts trigger them.

### `02-risk-scoring.md`

Defines risk levels and scoring.

Recommended levels:

| Level | Meaning | Typical Action |
|---|---|---|
| L1 Low | routine compliance record, limited exposure | proceed with recordkeeping |
| L2 Medium | compliance gap exists but can be controlled | proceed with conditions |
| L3 High | material legal, regulatory, technical, or ethics exposure | pause or escalate before launch |
| L4 Stop / Red Line | likely prohibited, unverifiable, or structurally unsafe | do not proceed until redesigned |

Risk scoring should combine:

- rule-based triggers;
- likelihood;
- severity;
- controllability;
- affected population;
- deployment stage;
- source confidence.

### `03-output-template.md`

Defines the final report structure:

```markdown
# AI 初创公司合规审查报告

## 1. 一页结论
审查事项、业务场景、总体风险等级、是否建议推进。

## 2. 场景画像
公司角色、产品阶段、模型类型、数据类型、用户群体、目标市场。

## 3. 风险矩阵
| 风险 | 领域 | 等级 | 触发事实 | 来源/依据 | 处理建议 |

## 4. 审查路径
PIA、AIA、数据出境评估、算法备案、供应商审查、外部律师复核等。

## 5. 整改清单
| 优先级 | 整改项 | 负责人 | 验收标准 | 建议期限 |

## 6. 来源与待核验清单
列出本地来源、用户材料、模型推理、待人工核验项。

## 7. 残余风险与复查触发条件
```

### Stage References

- `stage-intake.md`: targeted questions for missing facts.
- `stage-retrieval.md`: how to search or inspect local corpus files and how to label sources.
- `stage-risk-classification.md`: how to apply scoring and red-line rules.
- `stage-remediation.md`: how to convert risks into concrete actions and acceptance criteria.

### Scenario References

`scenario-financing.md` should cover:

- data asset ownership and legality;
- training data provenance;
- model/IP ownership;
- open-source and third-party dependency risk;
- unresolved regulatory or litigation exposure;
- compliance representations likely to appear in financing diligence.

`scenario-data-processing.md` should cover:

- personal information processing;
- sensitive personal information;
- minors;
- training/fine-tuning data;
- data minimization and retention;
- automated decision-making;
- data sharing and outsourcing;
- data export triggers.

`scenario-product-launch.md` should cover:

- product function and affected users;
- data collection changes;
- AI component detection;
- safety and cybersecurity;
- consumer disclosure;
- marketing claims;
- launch decision: clear, conditional, blocked, or escalated.

`scenario-cross-border.md` should cover:

- target markets and affected persons;
- overseas personal data protection regimes;
- cross-border transfer mechanisms;
- provider/deployer role split;
- export control, sanctions, geopolitical, and platform-policy risks where relevant;
- stricter-rule conflict handling.

`scenario-vendor-contract.md` should cover:

- whether vendor trains on customer inputs;
- confidentiality of prompts and uploaded documents;
- model change notice;
- output IP ownership;
- indemnity and liability;
- auditability and incident notice;
- subprocessor or upstream model provider accountability;
- data residency and deletion.

### Domain References

`domain-ai-governance.md` should cover:

- AI system role: provider, deployer, integrator, vendor user;
- high-impact decision scenarios;
- human oversight;
- transparency;
- testing, bias, robustness, hallucination, and incident handling;
- AI impact assessment trigger logic.

`domain-data-compliance.md` should cover:

- Cybersecurity Law, Data Security Law, Personal Information Protection Law, algorithm and generative AI related obligations;
- personal information lifecycle;
- sensitive data;
- important data;
- data export;
- PIA/DPIA-style assessment.

`domain-ip.md` should cover:

- training data copyright;
- generated output ownership;
- open-source model and code licenses;
- employee or contractor invention ownership;
- trade secret controls.

`domain-cybersecurity.md` should cover:

- network security baseline;
- access control;
- logs and audit;
- model/API abuse;
- data leakage;
- incident response.

`domain-consumer-marketing.md` should cover:

- consumer protection;
- misleading AI claims;
- algorithmic pricing or recommendation;
- advertising substantiation;
- minors or vulnerable users.

## Runtime Workflow

1. Parse the user request and identify the scenario.
2. Load `00-source-policy.md`, `01-intake-router.md`, and `02-risk-scoring.md`.
3. Load the dominant scenario file.
4. Load only domain files triggered by facts.
5. Ask narrowly scoped intake questions if required facts are missing.
6. Search or inspect local corpus material when legal or case support is needed.
7. Classify risk using the scoring rubric.
8. Produce the report from `03-output-template.md`.
9. Add a source and verification appendix.

## Example Loading Plans

Product launch involving an AI customer support assistant:

```text
SKILL.md
00-source-policy.md
01-intake-router.md
02-risk-scoring.md
scenario-product-launch.md
domain-ai-governance.md
domain-data-compliance.md
domain-consumer-marketing.md
03-output-template.md
```

Financing diligence for an AI startup with proprietary model claims:

```text
SKILL.md
00-source-policy.md
01-intake-router.md
02-risk-scoring.md
scenario-financing.md
domain-ip.md
domain-data-compliance.md
domain-ai-governance.md
03-output-template.md
```

Vendor API contract review:

```text
SKILL.md
00-source-policy.md
01-intake-router.md
02-risk-scoring.md
scenario-vendor-contract.md
domain-ai-governance.md
domain-data-compliance.md
domain-ip.md
03-output-template.md
```

## Local Corpus Strategy

Use `legal_preference_txt/` as the first retrieval base. Current useful categories include:

- `官方文件` for primary official materials and standards;
- `案例` for enforcement and judicial examples;
- `论文` for risk frameworks, AI governance, and comparative analysis;
- `书籍` for doctrinal or practical background.

The first implementation can rely on keyword search with `rg`. A later implementation can add embeddings, but the skill design should not require embeddings to be useful.

Search terms should be scenario-specific. Examples:

- product launch: `生成式人工智能`, `算法推荐`, `个人信息`, `自动化决策`, `安全评估`;
- data export: `数据出境`, `标准合同`, `个人信息保护影响评估`, `重要数据`;
- financing: `知识产权`, `商业秘密`, `训练数据`, `开源`, `合规尽调`;
- vendor contract: `模型训练`, `数据处理者`, `委托处理`, `输出`, `责任限制`.

## Quality Gates

Before final output:

- Every high or stop-level risk must have a concrete triggering fact.
- Every legal or regulatory assertion must have a source tag or a `[待核验]` tag.
- Every remediation item must include an action and acceptance criterion.
- The overall recommendation must match the highest unresolved risk.
- The report must not mention 北大法宝 as a used source or connected tool.

Optional `scripts/check_report_structure.py` can validate:

- required report sections exist;
- risk matrix rows include level and source;
- no forbidden source claim such as `北大法宝已核验`;
- all L3/L4 risks have remediation.

## Risks and Mitigations

| Risk | Mitigation |
|---|---|
| Context bloat from too many legal checklists | Keep `SKILL.md` lean and load references by scenario/domain |
| Hallucinated legal authority | Source tagging, local corpus retrieval, and explicit pending-verification list |
| Overbroad generic compliance output | Scenario routing and concrete intake questions |
| Under-identifying cross-domain AI risk | Domain trigger table and required cross-checks for AI, data, IP, cybersecurity, and marketing |
| Competition judges see it as only a memo | Produce structured reports, risk matrices, and optional validation scripts as demonstrable artifacts |

## Implementation Phases

### Phase 1: References-only Skill

Create the skill folder, concise `SKILL.md`, and the core reference files:

- source policy;
- intake router;
- risk scoring;
- output template;
- five scenario files.

This phase is sufficient for a working competition demo.

### Phase 2: Local Corpus Retrieval Support

Add guidance or helper scripts for local corpus search. Keep retrieval transparent and source-tagged.

Possible script:

```text
scripts/search_corpus.py
```

The script can search `legal_preference_txt/` and return file path, line snippets, category, and suggested source tag.

### Phase 3: Deterministic Scoring and Report Validation

Add:

- `scripts/score_risk.py` for repeatable scoring from structured facts;
- `scripts/check_report_structure.py` for output QA.

### Phase 4: Prototype UI or Workflow Demo

Build a lightweight interface only after the skill itself works. The UI should show:

- scenario intake;
- selected references;
- risk matrix;
- remediation checklist;
- source verification appendix.

## Open Decisions

- Whether the first implementation should live under the repo for competition submission or under the local Codex skills directory for immediate use.
- Whether to implement corpus search as plain `rg` guidance first or as a helper script.
- Whether to include English/EU/US outbound compliance in the first demo or keep the first demo China-focused with an expansion appendix.
