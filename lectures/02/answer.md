# 第2回 解答

## 演習

- 使ったモデル・設定（`result.json` の `config` から）：
- 自分で学習を実行したか（`executed_training_myself`）：はい / いいえ（実行できた範囲：　）
- 学習前：`format_rate` =　 / `content_rate` =　
- 学習後：`format_rate` =　 / `content_rate` =　
- 退行した質問：
- 学習後に残った失敗と、その型（根拠なしの質問に断定した / 根拠ありの質問に確認要求で逃げた / 形式の崩れ）：
- 知識を問う質問（セクション 6）の結果（学習前の分類 → 学習後の分類）と気づいたこと：
- 学習で変わったこと・変わらなかったこと：

## 宿題

- 取り組んだレベル：基礎 
- 変えた設定やデータ：num_train_epochsを30に変更した。
- 結果（比較表や数字）：
result = {
    "config": {k: v for k, v in CONFIG.items() if k not in ("work_dir", "drive_dir")},
    "versions": {m.__name__: m.__version__ for m in [transformers, peft, trl, torch]},
    "method": "LoRA（量子化なし）",
    "before": before_summary, "after": after_summary,
    "regressed": regressed,
    "knowledge": knowledge_rows,
    "executed_training_myself": True,   # 講師の結果を使った場合は False にする
}
(WORK / "result.json").write_text(json.dumps(result, ensure_ascii=False, indent=2), encoding="utf-8")
print(json.dumps({k: v for k, v in result.items() if k != "knowledge"}, ensure_ascii=False, indent=2))
if USE_DRIVE:
    import shutil
    for name in ["result.json", "eval_before.json", "eval_after.json", "test.jsonl", "gold.json"]:
        shutil.copy(WORK / name, Path(CONFIG["drive_dir"]) / name)
    print("Drive に保存しました:", CONFIG["drive_dir"])
- ファインチューニングが向く場面・向かない場面についての考察：形式的な物には聞くが事実の判定などは難しい
