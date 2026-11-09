# 第3回 解答

## 演習

- 第1回（素のLLM）とエージェントの回答の違い：素のLLMは一般論でもっともらしく回答しているが、エージェントは用語集を検索してから回答している
- `--trace` で見えたツール呼び出し（ツール名と引数）：
--- trace ---
[tool call]   search_glossary({'query': 'LoRA'})
[tool call]   search_glossary({'query': 'QLoRA'})
[tool return] search_glossary -> [{"term": "LoRA", "lecture": 2, "definition": "事前学習済みの重みは固定したまま、重みの更新分を2つの小さな行列の積（低ランク行列）で表し、その小さな行列だけを学習するPEFT手法。学習するパラ…
[tool return] search_glossary -> [{"term": "QLoRA", "lecture": 2, "definition": "ベースモデルの重みを4ビットなどに量子化してメモリを節約したうえで、LoRAで学習する手法。より大きなモデルを小さなGPUで扱える。本講座の第2…
[usage] RunUsage(cost=Decimal('0.0008769'), details={'text_prompt_tokens': 1198}, input_text_tokens=1198, input_tokens=1198, output_tokens=207, requests=2, tool_calls=2)
- 1つの質問での LLM 呼び出し回数（`requests`）：2回
- 追加したツールと、docstring に書いた説明：
追加したツール：make_quiz
docstring：指定した回の用語から 1つを選び、確認問題と答えを返す。「第2回の確認問題を出して」のように、復習用の問題を求められたときに使う。

## 宿題

- 取り組んだレベル：基礎
- やったこと：INSTRUCTIONS から「必ず search_glossary で用語集を確認する」の行を消した
- 結果：
前
[model] gemini-3.5-flash-lite
LoRAとQLoRAの主な違いは、ベースモデルの重みを量子化するかどうか（およびそれに伴うメモリ使用量）にあります。

* **LoRA**: 
  事前学習済みの重みを固定したまま、低ランクの小さな行列（アダプタ）だけを学習することで、学習パラメータ数とメモリを削減する手法です。重みは通常の精度（FP16やBF16など）のまま扱います。
* **QLoRA**: 
  LoRAの仕組みをベースにしつつ、ベースモデルの重みを4ビットなどに**量子化**（圧縮）してから学習する手法です。これにより、さらに大幅なメモリ節約が可能になり、限られたGPUリソースでもより大きなモデルを効率よく学習できるようになります。

（第2回）

--- trace ---
[tool call]   search_glossary({'query': 'LoRA'})
[tool call]   search_glossary({'query': 'QLoRA'})
[tool return] search_glossary -> [{"term": "LoRA", "lecture": 2, "definition": "事前学習済みの重みは固定したまま、重みの更新分を2つの小さな行列の積（低ランク行列）で表し、その小さな行列だけを学習するPEFT手法。学習するパラ…
[tool return] search_glossary -> [{"term": "QLoRA", "lecture": 2, "definition": "ベースモデルの重みを4ビットなどに量子化してメモリを節約したうえで、LoRAで学習する手法。より大きなモデルを小さなGPUで扱える。本講座の第2…
[usage] RunUsage(cost=Decimal('0.0008769'), details={'text_prompt_tokens': 1198}, input_text_tokens=1198, input_tokens=1198, output_tokens=207, requests=2, tool_calls=2)

後
[model] gemini-3.5-flash-lite
講座の用語集によると、**LoRA** と **QLoRA** の違いは以下の通りです。

* **LoRA（Low-Rank Adaptation）**
  事前学習済みの重みを固定したまま、重みの更新分を2つの小さな行列（低ランク行列）の積で表し、その小さな行列だけを学習するPEFT（Parameter-Efficient Fine-Tuning）手法です。学習するパラメータを大幅に削減できます。

* **QLoRA（Quantized LoRA）**
  ベースモデルの重みを 4ビットなどに**量子化**してメモリを節約したうえで、LoRAによるファインチューニングを行う手法です。これにより、より大きなモデルをより小さなGPUメモリで扱えるようになります。

（関係する回：第2回）

--- trace ---
[tool call]   search_glossary({'query': 'LoRA'})
[tool call]   search_glossary({'query': 'QLoRA'})
[tool return] search_glossary -> [{"term": "LoRA", "lecture": 2, "definition": "事前学習済みの重みは固定したまま、重みの更新分を2つの小さな行列の積（低ランク行列）で表し、その小さな行列だけを学習するPEFT手法。学習するパラ…
[tool return] search_glossary -> [{"term": "QLoRA", "lecture": 2, "definition": "ベースモデルの重みを4ビットなどに量子化してメモリを節約したうえで、LoRAで学習する手法。より大きなモデルを小さなGPUで扱える。本講座の第2…
[usage] RunUsage(cost=Decimal('0.0009118'), details={'text_prompt_tokens': 1306}, input_text_tokens=1306, input_tokens=1306, output_tokens=208, requests=2, tool_calls=2)
- 気づき・疑問：INSTRUCTIONS から「必ず search_glossary で用語集を確認する」の行を消しても、用語集を検索して回答している
