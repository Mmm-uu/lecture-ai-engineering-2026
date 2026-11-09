# 第2回 解答

## 演習

- 使ったモデル・設定（`result.json` の `config` から）：
"config": {
    "base_model": "Qwen/Qwen3.5-0.8B",
    "revision": "2fc06364715b967f1860aea9cf38778875588b17",
    "seed": 42,
    "num_train_epochs": 20,
    "warmup_steps": 6,
    "lr": 0.0002,
    "lora_r": 16,
    "lora_alpha": 32,
    "max_seq_len": 512,
    "max_new_tokens": 96,
    "gen": {
      "do_sample": false
    }
  }
- 自分で学習を実行したか（`executed_training_myself`）：はい（実行できた範囲：学習前評価、LoRA学習、学習後評価）
- 学習前：`format_rate` =　0.09523809523809523 / `content_rate` =　0.0
- 学習後：`format_rate` =　1.0 / `content_rate` =　0.9047619047619048
- 退行した質問："成績はいつ出ますか"
- 学習後に残った失敗と、その型（根拠なしの質問に断定した / 根拠ありの質問に確認要求で逃げた / 形式の崩れ）："成績はいつ出ますか" : 根拠なしの質問に断定した
- 知識を問う質問（セクション 6）の結果（学習前の分類 → 学習後の分類）と気づいたこと：学習前の分類は全てformat_errorだったが、学習後の分類にはformat_errorがなくなり、needs_reviewまたはassertedとなったこと
- 学習で変わったこと・変わらなかったこと：学習でJSON形式を守る回答が増えた。一方で、FAQにない質問への断定が残った。

## 宿題

- 取り組んだレベル：基礎
- 変えた設定やデータ："lora_r": 8
- 結果（比較表や数字）：
変更前
  "before": {
    "label": "before",
    "n": 21,
    "format_rate": 0.09523809523809523,
    "content_rate": 0.0,
    "both_rate": 0.0,
    "seconds": 26.9
  },
  "after": {
    "label": "after",
    "n": 21,
    "format_rate": 1.0,
    "content_rate": 0.9047619047619048,
    "both_rate": 0.9047619047619048,
    "seconds": 44.2
  }

変更後
  "before": {
    "label": "before",
    "n": 21,
    "format_rate": 0.09523809523809523,
    "content_rate": 0.0,
    "both_rate": 0.0,
    "seconds": 48.3
  },
  "after": {
    "label": "after",
    "n": 21,
    "format_rate": 1.0,
    "content_rate": 0.9523809523809523,
    "both_rate": 0.9523809523809523,
    "seconds": 68.1
  }
- ファインチューニングが向く場面・向かない場面についての考察：今回の演習では、LoRAによるファインチューニング後に、指定されたJSON形式を守る回答が増えた。このことから、ファインチューニングは、出力形式安定させる場面に向いていると考えられる。
一方で、FAQに存在しない質問について、確認を求めずに断定する失敗が残った。このことから、ファインチューニングによって形式を覚えさせても、モデルが知らない情報を正しく判定したり、根拠の有無を保証したりすることは難しいと分かった。
