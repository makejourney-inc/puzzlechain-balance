# 配信用バランス値

MJ のゲームタイトル向けに、ゲーム内の数値をアプリの更新なしで差し替えるための置き場です。

`balance.json` をアプリが起動時に取得し、**書かれている項目だけ**を上書きします。

どのタイトルのものかは、対象タイトル側のリポジトリと MJ Platform で管理します。

> **公開設定について**
> このリポジトリは public です。アプリは認証情報を持たないため、
> private にすると `raw.githubusercontent.com` が匿名アクセスに 404 を返し、
> アプリが取得できなくなります。**private にしないでください。**

## 更新のしかた

1. Unity でタイトルのプロジェクトを開き、`Tools > (タイトル名) > 4. Tuning Window` を開く
2. 「マスターデータ (BalanceConfig)」で値を調整する
3. 「配信用 JSON」の「現在の値を JSON として書き出す」を押す（クリップボードにも入ります）
4. このリポジトリの `balance.json` を、その内容で置き換えて push する

手で書かないでください。Unity から書き出した形のまま置き換えます。

アプリ同梱の初期値（`BalanceBaked.json`）をそのままコピーしないでください。
用途が違います。必ず Tuning Window から書き出したものを使います。

## 書き換えるときの決まり

- **変えたい項目だけ**書けば動きます。書いていない項目は今の値のままです。
- `version` は変更のたびに1つ増やしてください。どの版が配信されているかの確認に使います。
- 数値のところに文字列（`"60"` や `"六十"`）を書かないでください。アプリが丸ごと無視します。
- 極端な値を書いても遊べる範囲に丸められます。壊れはしませんが、意図した値にはなりません。

## 壊れたときは

この JSON が読めない・おかしい場合、アプリはアプリ内に同梱した初期値で動きます。
`balance.json` を消しても、直前の版に戻しても、プレイに支障は出ません。

取得は起動時に投げっぱなしで行い、5秒で諦めます。失敗してもアプリの起動は止まりません。
取得結果は端末に保存しないので、次の起動でまた取りに行きます。

## 項目

現在の `balance.json` は **トップレベル 37項目**です。
内訳は「単一値 32項目」と「配列 5項目」です。

配列の中身まで数えると、`skills` 6件・`bombs` 1件・`tileScores` 6件・
`costumePrices` 2件・`unitExpToNextLevel` 9段になります。

定義の大元はタイトル側の `Assets/Scripts/Balance/BalanceConfig.cs` です。
そちらには配信していない項目も含まれるため、数は一致しません。

### 単一値（32項目）

| 分類 | 項目 |
|---|---|
| 版番号 | `version` |
| 制限時間 | `timeLimit`, `warningTime` |
| 盤面・チェーン | `spawnCount`, `spawnInterval`, `minChainLength`, `linkDistanceMultiplier` |
| スコア係数 | `chainMultiplierBase`, `chainMultiplierPerTile` |
| 大玉 | `largeThreshold`, `largeMaxChain`, `largeScale`, `largeValue`, `maxLarge`, `probabilisticLarge`, `largeProbability` |
| 爆弾の扱い | `allowBombInduction`, `bombChainMultiplier`, `bombDragMode` |
| Fever | `feverRequiredClears`, `feverDuration`, `feverScoreMultiplier`, `feverTimeBonus`, `feverComboWindow` |
| コイン | `coinsPerCleared`, `coinsPerScore`, `coinsMinimumPerPlay` |
| 単位の育成 | `unitMaxLevel`, `unitScoreBonusPerLevel`, `unitSkillBonusPerLevel`, `gachaDuplicateExp` |
| 順位の報告 | `maxReportableScore` |

### 配列（5項目）

| 項目 | 件数 | 中身 |
|---|---|---|
| `skills` | 6 | `id` / `requiredGauge` / `strength` / `duration` |
| `bombs` | 1 | `id` / `minChain` / `maxChain` / `radiusInDiameters` / `scoreMultiplier` |
| `tileScores` | 6 | `id` / `scoreValue` |
| `costumePrices` | 2 | `id` / `coinPrice` |
| `unitExpToNextLevel` | 9段 | レベルを1つ上げるのに必要な経験値 |

`skills` / `bombs` / `tileScores` / `costumePrices` は `id` で突き合わせます。順番は問いません。
`unitExpToNextLevel` だけは並び順そのものが意味を持ちます（先頭が Lv1→Lv2）。
