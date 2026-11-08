# Langflow workflow design (PPIIA)

This is the visual version of the pipeline that `analyzer.py` runs in code. Build it in Langflow (langflow.org) to demonstrate the workflow skill.
It is a design, not an exported flow file.

| # | Langflow component | Settings | Output goes to |
| --- | --- | --- | --- |
| 1 | **File** (PDF) or **URL** loader | Upload a bill PDF or paste a sansad.in link | 2 |
| 2 | **Text cleaner** (Python custom component using `preprocess.clean_text`) | Removes page numbers, joins hyphenated lines | 3, 4, 5, 6 |
| 3 | **Topic classifier** (custom component using `classifier.classify`) | Returns the top sectors | 7 |
| 4 | **Chronology builder** (custom component using `chronology.build_chronology`) | Earlier acts, amendments, committees | 7 |
| 5 | **Risk detector** (custom component using `risk.detect_risks`) | Clause-level risk flags | 9 |
| 6 | **Text splitter / truncator** | Keep the first 45,000 and last 10,000 characters | 7 |
| 7 | **Prompt** component | Paste `llm.PROMPT`; variables: `text`, `sectors`, `acts` | 8 |
| 8 | **Google Generative AI** | Model `gemini-flash-latest`, temperature 0.2, JSON output, your API key as a secret | 9 |
| 9 | **Parser / Chat Output** | Parse the JSON; show summary, impact, risks | end |

Explain in the viva: steps 3-5 are transparent rules (cheap, explainable); step 8 is the only LLM call; the prompt tells the model to use only facts from the bill and to answer "Not stated" otherwise.
