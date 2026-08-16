---
title: "Godot 4でUIが突然動かなくなった。var := が引き起こすパースエラーの罠"
emoji: "🛠"
type: "tech"
topics: ["godot"]
published: true
---

Godot 4でゲームを作っていると、こんな体験をすることがある。

「さっきまで動いてたのに、シーンを開いても何も動かない」

UIを少し修正してF5を押したら、ボタンを押しても反応がない。ノードは存在しているし、エラーダイアログも出ていない。シーンツリーも正常に見える。なのに、何も動かない。

これはGDScriptの**型推論パースエラー**が原因の可能性が高い。しかもこのエラー、かなり気づきにくい。

---

### 症状

- シーンは開ける

- エディタ上でエラーは表示されない（赤波線なし）

- F5で起動しても、そのシーンのスクリプトが一切動かない

- _ready() すら呼ばれていない

**「画面が出るのに動かない」というパターン**がこのバグの特徴。

---

### 原因
GDScript 4 では := を使った型推論が便利だが、**三項演算子と null を組み合わせると型が確定できずパースエラーになる**。

```
# これがパースエラーになる
var vbox := pt.get_child(0) if pt.get_child_count() > 0 else null
var layer := get_node_or_null("Layer") if visible else null
```
エラーメッセージ:

```
Parse Error: Cannot infer the type of "vbox" variable because the value doesn't have a set type.
```
:= は右辺の型からローカル変数の型を推論する。しかし null はどの型でもないため、推論が失敗する。

このエラーが起きると**スクリプトファイル全体がロードされない**。そのため、_ready() も _process() も呼ばれず、ノードはただ存在するだけになる。

---

### エラーログの場所
エディタのアウトプットパネルに出ないことがある。その場合はログファイルを確認する。

```
%APPDATA%\Godot\app_userdata\\logs\godot.log
```
ここに以下のようなログが残っている:

```
SCRIPT ERROR: Parse Error: Cannot infer the type of "vbox" variable because the value doesn't have a set type.
   at: GDScript::reload (res://scenes/deploy_scene.gd)
```
このログを見れば、どのファイルの何行目かがわかる。

---

### 修正方法
:= をやめて、**明示的な型注釈**を付ければ解決する。

```
# 修正後
var vbox: Node = pt.get_child(0) if pt.get_child_count() > 0 else null
var layer: CanvasLayer = get_node_or_null("Layer") if visible else null
```
ポイントは var 変数名: 型名 =（コロンとイコールの間にスペースなし）。

型がわからない場合は Node か Object にしておけばとりあえず動く。

---

### Variant型でも同じ罠がある
実は := を使わなくても、Variant 型を推論させようとすると別の問題が起きる。

```
# これも地雷
var data := some_func()  # some_func() が Variant を返す場合
```
GDScript 4 は Variant 型を := で推論させることができない。このパターンに当たったら同様に var data: Variant = some_func() と明示する。

---

### まとめ
**パターン** | **問題** | **修正**

- var x := expr if cond else null | null の型が未定でパースエラー | var x: Node = ...

- var x := func_returning_variant() | Variant型は推論不可 | var x: Variant = ...

**:= は便利だが、右辺に null や Variant が絡む場合は使わない。** これだけ覚えておけば同じ地雷は踏まずに済む。

Godot 4でUIが急に動かなくなったら、まずログファイルを確認してみてほしい。

---

この記事はGodot 4.6 / GDScript 4で確認した内容です。

---

この記事は note で公開したものを、加筆せずに転載しています（原典: https://note.com/lazy_engineer/n/nfd3732d7eead ）。