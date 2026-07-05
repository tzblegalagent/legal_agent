# Stage: Retrieval

Use this file before searching or citing local corpus material.

## Local Corpus Base

Search `legal_preference_txt/` first when it exists in the workspace.

Recommended categories:

- `legal_preference_txt/官方文件` -> `[本地法规库]`
- `legal_preference_txt/案例` -> `[本地案例]`
- `legal_preference_txt/论文` -> `[本地论文/报告]`
- `legal_preference_txt/书籍` -> `[本地论文/报告]`

## Search Pattern

Use focused terms from the active scenario:

- Product launch: `生成式人工智能`, `算法推荐`, `个人信息`, `自动化决策`, `安全评估`, `深度合成`
- Data export: `数据出境`, `标准合同`, `个人信息保护影响评估`, `重要数据`, `境外`
- Financing: `知识产权`, `商业秘密`, `训练数据`, `开源`, `合规尽调`, `数据资产`
- Vendor contract: `模型训练`, `数据处理者`, `委托处理`, `输出`, `责任限制`, `删除`
- Marketing: `AI washing`, `误导`, `准确性`, `无偏见`, `宣传`

## Source Handling

- Prefer official or regulator materials for legal duties.
- Use cases to illustrate enforcement or dispute exposure.
- Use papers/books to explain frameworks, not to assert binding duties.
- Record source path and relevant line/snippet.
- If retrieval finds no local support, write `[待核验: 缺少本地来源]`.

## Optional Script

Run:

```bash
python3 skills/ai-startup-compliance-review/scripts/search_corpus.py legal_preference_txt --query "生成式人工智能" --query "个人信息" --limit 8
```

Use returned snippets as starting points, not as automatic legal conclusions.
