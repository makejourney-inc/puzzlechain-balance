# puzzlechain-balance

PuzzleChain の配信用バランス値を置く場所です。

`balance.json` はゲーム内の数値をまとめたもので、アプリを更新せずに差し替えられます。
アプリはこの JSON を取得し、書かれている項目だけを上書きします。

## 更新のしかた

1. Unity で PuzzleChain を開き、`Tools > PuzzleChain > 4. Tuning Window` を開く
2. 「マスターデータ (BalanceConfig)」で値を調整する
3. 「配信用 JSON」の「現在の値を JSON として書き出す」を押す（クリップボードにも入ります）
4. このリポジトリの `balance.json` を、その内容で置き換えて push する

手で書かないでください。Unity から書き出した形のまま置き換えます。

## 書き換えるときの決まり

- **変えたい項目だけ**書けば動きます。書いていない項目は今の値のままです。
- `version` は変更のたびに1つ増やしてください。どの版が配信されているかの確認に使います。
- 数値のところに文字列（`"60"` や `"六十"`）を書かないでください。アプリが丸ごと無視します。
- 極端な値を書いても遊べる範囲に丸められます。壊れはしませんが、意図した値にはなりません。

## 壊れたときは

この JSON が読めない・おかしい場合、アプリはアプリ内に同梱した初期値で動きます。
`balance.json` を消しても、直前の版に戻しても、プレイに支障は出ません。

## 項目

51項目あります。内訳は PuzzleChain 側の `Assets/Scripts/Balance/BalanceConfig.cs` を見てください。

| 分類 | 項目 |
|---|---|
| 版番号 | `version` |
| 制限時間 | `timeLimit`, `warningTime` |
| 盤面・チェーン | `spawnCount`, `minChainLength`, `linkDistanceMultiplier` |
| スコア係数 | `chainMultiplierBase`, `chainMultiplierPerTile` |
| 大玉 | `largeThreshold`, `largeMaxChain`, `largeScale`, `largeValue`, `maxLarge`, `probabilisticLarge`, `largeProbability` |
| 爆弾の扱い | `allowBombInduction`, `bombChainMultiplier`, `bombDragMode` |
| Fever | `feverRequiredClears`, `feverDuration`, `feverScoreMultiplier`, `feverTimeBonus`, `feverComboWindow` |
| スキル | `skills[]`（6種 × 必要ゲージ / 強さ / 継続秒） |
| 爆弾の数値 | `bombs[]`（最低実物数 / 最大実物数 / 爆発半径 / 得点倍率） |
| タイル得点 | `tileScores[]`（6種） |

`skills` / `bombs` / `tileScores` は `id` で突き合わせます。順番は問いません。
