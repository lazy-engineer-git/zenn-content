---
title: "月10万円を目指して、4時間でAPIデバッグツールを作ってみた"
emoji: "🛠"
type: "tech"
topics: ["claudecode", "chrome拡張", "生成ai", "個人開発", "自動化"]
published: true
---

📌 **この記事は「Chrome拡張を作って販売してみたいエンジニア」向けです。**
特に「フリーミアム構成でどうやってライセンス認証を実装するか」でつまずいている人に読んでほしいです。

**この記事で学べること**
- Chrome拡張でfetch/XHRをインターセプトするときのIsolated World問題と解決策
- バックエンド不要でGumroadライセンス認証を完結させる構成
- GumroadライセンスAPIで詰まるproduct_idの正しい取得方法
- テスト用ライセンスキーの入手方法（sample keyが使えない理由）

### なぜAPIデバッグツールを作ったのか
API開発のデバッグをするたびに、DevToolsのNetworkタブを開いてリクエストを追う作業が地味に面倒でした。
「どうせなら自分が使えて、同じ悩みのエンジニアにも使ってもらえるものを作ろう」というのがきっかけです。

副業で月10万円を目指している私にとって、「自分が本当に使うツールか」を先に考えます。
使わないものを売るのは続かないからです。

開発にかかった時間は約4時間。
Claude Codeを使いながら実装したことで、ボイラープレートの記述や調査にかかる時間を大幅に短縮できました。
「楽するために全力で自動化する」というブログコンセプトをそのまま実践した形です。

### 📌 何を作ったのか
Chrome拡張でAPIリクエストをキャプチャ・分析できるデバッグツールです。

fetch/XHRをインターセプトして、ログをサイドパネルに表示します。
curlコマンド生成・リクエスト再送・JSONエクスポート・AI向けプロンプト生成など、API作業の繰り返しを省力化するのが目的です。

フリーミアム構成にしました。

- **無料**: ログ20件まで表示

- **Pro（$12・Gumroad購入）**: 制限解除 ＋ JWT自動注入・環境切り替えなどの上位機能

### 💡 ハマったこと① Isolated WorldとMain World問題
Chrome拡張でfetchをインターセプトしようとすると、最初の壁にぶつかります。

content scriptは「Isolated World」で動いていて、ページのfetchを直接触れません。
でも、chrome APIはIsolated WorldからしかアクセスできないのでMain Worldには置けません。

結局、こう分けることで解決しました。

```
content-main.js → Main World（fetch/XHRのインターセプト担当、chrome API不可）
content-bridge.js → Isolated World（chrome API担当）
```
データの流れはこうなります。

```
[ページ上のfetch/XHR]
↓ インターセプト
content-main.js（Main World）
↓ postMessage
content-bridge.js（Isolated World）
↓ chrome.runtime.sendMessage
background.js（Service Worker）
↓ chrome.storage に保存
サイドパネル に表示
```
実際のコードはこんな感じです。

**content-main.js（Main World）**

```
const _fetch = window.fetch;
window.fetch = async (...args) => {
const res = await _fetch(...args);
window.postMessage({ type: 'API_LOG', url: String(args[0]), status: res.status }, '*');
return res;
};
```
**content-bridge.js（Isolated World）**

```
window.addEventListener('message', (e) => {
if (e.source !== window || e.data?.type !== 'API_LOG') return;
chrome.runtime.sendMessage(e.data);
});
```
この構成に辿り着くまで、「なぜchrome APIが使えない？」と何度も詰まりました。
Chrome拡張の公式ドキュメントはボリュームが多く、この罠が書いてある場所を見つけるのに時間がかかります。

### 💡 ハマったこと② Gumroadのproduct_idが罠だった
フリーミアムにするためにGumroadのライセンス認証を実装しました。
バックエンドなしで、background.jsからGumroad APIを直接叩く構成です。

ここで詰まりました。

Gumroad APIの `product_id` に何を入れればいいか、ドキュメントに明記されていません。
商品URLの末尾にあるpermalink（`clktd`）を入れても動きませんでした。

解決策は少し強引で、**product_idを入れずにAPIを叩く**ことです。
するとエラーメッセージにこう書いてありました。

```
"Please set 'product_id' to 'MQ9VRqHs2QkX6dYgOVx99Q==' in the request."
```
エラーに正しい値が入っていました。
ドキュメントを信じすぎず、レスポンス本体を確認することの大事さを改めて学びました。

また `==` が含まれるため、URLに渡す際は `encodeURIComponent()` が必須です。
これを忘れると認証が通らないので注意してください。

※ 2026年6月8日時点の仕様です。Gumroad APIは非公式なため、仕様が変わった場合は同じ手順で再確認してください。

### 💡 ハマったこと③ テスト用ライセンスキーの取り方
「Gumroadダッシュボードのsample keyでテストできる」と思っていました。
これは間違いで、sample keyはverify APIで弾かれます。

正しい方法は、**自分の商品を$0（または100%オフクーポン）で購入**することです。
メールに届くキーが実際の本番キーと同じ形式で、`"test": true` フラグつきで認証が正常に通りました。

これを知らずにずっとsample keyでテストしようとしていたので、無駄に時間を使いました。

### ✅ 今の状態

- **Chrome Web Store**: 審査提出済み（1〜2週間で承認予定）→ 承認後にストアURLを追記します

- **Gumroad**: 公開済み → https://verdant660.gumroad.com/l/clktd

- **バージョン**: v1.0.0

### 🎯 まとめ
4時間の開発でChrome拡張をフリーミアムで販売する構成まで作りました。

**ハマりポイント3つのまとめ：**

- Isolated/Main World分離 → postMessageでブリッジする2ファイル構成

- Gumroad product_id不明 → product_idなしでAPIを叩いてエラーメッセージから取得

- テスト用ライセンスキー → sample keyは使えない。自分で$0購入する

Chrome拡張を作ったことはありますか？
「試してみたい」「ここはどうしてるの？」があれば、コメントで教えてください。

📅 次回は、Claude Codeで仮想組織を作って開発チームっぽく使う話を書きます。

💡 無料で使いたい方 → Chrome Web Store（審査中・近日公開）
🎯 全機能使いたい方 → Pro版を購入する($12) [https://verdant660.gumroad.com/l/clktd](https://verdant660.gumroad.com/l/clktd)

以下、イメージ画像です
CWS が承認されたタイミングで機能紹介の記事を書きます。

1枚目はアイコン
2,3枚目はツールテスト用のページを用いてAPIデバッグ補助ツールの使用イメージを出しています。

![](https://assets.st-note.com/img/1780845247-uKwCvjdNrXtJbeMmsHWgZGBD.png)

![](https://assets.st-note.com/img/1780845225-gV3v58fOEGRFKxWTYlzaN6I1.png)

![](https://assets.st-note.com/img/1780845225-REeSQCHbwdgD1fAkNcopBYKV.png)
---

**連載ナビ**

◀ 前回: 実録#1・スタート宣言

▶ 次回: 実録#3・Claude Codeで仮想組織を作った話

📚 シリーズ全話: [まとめマガジン](https://note.com/lazy_engineer/m/m03a726b1673c)

この記事が役に立ったら、スキを押してもらえると次の記事の燃料になります。

---

この記事は note で公開したものを、加筆せずに転載しています（原典: https://note.com/lazy_engineer/n/ndcf7890d481a ）。