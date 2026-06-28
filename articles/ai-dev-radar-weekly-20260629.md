---
title: "AIスコアで選ぶ 今週のGitHub注目リポジトリ 10選（2026-06-29週）"
emoji: "📡"
type: "tech"
topics: ["ai", "github", "llm", "生成ai"]
published: true
---

# リライト文

GitHubで注目を集めるAI関連プロジェクトを、独自のスコアリング手法で分析し、今週のトップ10リポジトリをランキング形式でお届けします。MCPサーバーの実装例として話題の「badchars/darknet-mcp-server」をはじめ、開発者から高い関心を集めているプロジェクトの動向を紹介します。AIとオープンソースの最新トレンドを把握するために、ぜひご覧ください。

スコアはClaude APIによる独自評価（新規性・成長速度・実用性など7軸・100点満点）です。

## 今週のランキング

| 順位 | リポジトリ | スコア |
| --- | --- | --- |
| 1 | [badchars/darknet-mcp-server](https://github.com/badchars/darknet-mcp-server) | 78 |
| 2 | [raiyanyahya/recall](https://github.com/raiyanyahya/recall) | 76 |
| 3 | [shy3130/tickflow-stock-panel](https://github.com/shy3130/tickflow-stock-panel) | 76 |
| 4 | [mindmuxai/brain.md](https://github.com/mindmuxai/brain.md) | 76 |
| 5 | [hirokawaguchi/open-genai](https://github.com/hirokawaguchi/open-genai) | 76 |
| 6 | [GraeLefix/GITVERSE](https://github.com/GraeLefix/GITVERSE) | 74 |
| 7 | [sums001/Windows-Copilot-API](https://github.com/sums001/Windows-Copilot-API) | 72 |
| 8 | [Johell1NS/browser-search](https://github.com/Johell1NS/browser-search) | 72 |
| 9 | [Huxiaoyou97/ManvoTV](https://github.com/Huxiaoyou97/ManvoTV) | 72 |
| 10 | [Reyzowter/Hello-Agents](https://github.com/Reyzowter/Hello-Agents) | 72 |

## 1. badchars/darknet-mcp-server（78点）

66個のツールを集約したMCP（Model Context Protocol）サーバーで、ブリーチデータ、ランサムウェア追跡、Tor .onion アクセス、マルウェア分析などダークウェブ情報を一元化。Claude等のAIエージェントに脅威インテリジェンス機能を統合できます。

- MCPの急成長期に特化型サーバーとしてセキュリティ領域を開拓
- 66ツール統合により、OSINT/脅威インテリジェンス作業の自動化を実現
- ダークウェブアクセスのようなニッチだが高需要な機能をAIに統合

実用性: 高 / 収益化: 中 / 日本市場: 中

https://github.com/badchars/darknet-mcp-server

## 2. raiyanyahya/recall（76点）

Claude Codeのセッション間でプロジェクト情報を永続化するオフラインメモリツール。トークン消費を削減しながら、プライバシーを保ちつつ開発効率を向上させる。

- Claude Code統合で実務開発の直接的な痛点解決
- 完全オフライン動作でプライバシー・コスト両立
- 急速な成長（313スターは比較的新規プロジェクト特性）
- Anthropic周辺エコシステムの拡張機能として差別化

実用性: 高 / 収益化: 中 / 日本市場: 中

https://github.com/raiyanyahya/recall

## 3. shy3130/tickflow-stock-panel（76点）

自托管型のA株向け量化分析プラットフォーム。銘柄選定・監視・バックテスト機能を一体化。TickFlowデータとTushareなど複数データソースに対応。

- A株市場向けの統合量化プラットフォーム - 日本でも需要が高い分野
- 直近140日でStar221達成 - 急速な成長軌道
- LLM/AI-Agent対応で自動化・拡張性が高い
- 自托管・ゼロ運維アーキテクチャで事業化しやすい

実用性: 高 / 収益化: 高 / 日本市場: 中

https://github.com/shy3130/tickflow-stock-panel

## 4. mindmuxai/brain.md（76点）

Claude CodeやCodexなどのAIエージェントに持続的なプロジェクトメモリを提供するCLIツール。マークダウンベースの設定で、プロジェクト決定・要件・制約を永続化し、エージェントの一貫性を確保。依存ライブラリ不要でシンプル。

- AI開発の実務課題『コンテキスト喪失』への直接的ソリューション
- ゼロ依存・マークダウンベースで導入障壁が極めて低い
- エージェント活用が進む2025年の需要増加局面で高い時間的タイミング

実用性: 高 / 収益化: 中 / 日本市場: 中

https://github.com/mindmuxai/brain.md

## 5. hirokawaguchi/open-genai（76点）

デジタル庁の「源内(GENAI)」をローカルLMで再実装した非公式プロジェクト。SAML認証・RAG・音声変換・画像生成をオンプレで完結でき、政府機関や大企業の情報セキュリティ要件に対応。

- 日本の政府AI基盤をローカル化する初の実践的実装 - 官公庁/大企業の閉域環境運用ニーズに直結
- エンタープライズグレード機能一式(SAML/RAG/Whisper/SD)を統合 - 実務導入の複雑性を軽減
- デジタル庁との関連性により日本市場での信頼度・認知度が高い - 政策連携可能性も存在

実用性: 高 / 収益化: 高 / 日本市場: 高

https://github.com/hirokawaguchi/open-genai

## 6. GraeLefix/GITVERSE（74点）

GitHubリポジトリを自動解析し、アーキテクチャ図・ASCII設計図・AI再構築プロンプトに変換するツール。Claude/GPTなどのLLMで既存コードベースを理解・再実装する際の最適なプロンプト生成を実現。

- LLM時代のコード理解効率化 - 複雑なコードベースをAIが消化可能な形式に自動変換
- リバースエンジニアリング×AI プロンプト化 - 既存プロジェクト継承や学習コストを大幅削減
- 急速な成長 - わずか131スターながら直近46件の増加で高い関心度

実用性: 高 / 収益化: 中 / 日本市場: 中

https://github.com/GraeLefix/GITVERSE

## 7. sums001/Windows-Copilot-API（72点）

Windows CopilotをリバースエンジニアリングしてOpenAI互換APIに変換。GPT-4/5モデルをAPI課金なしで利用可能。REST APIで簡単統合できる。

- 無料でGPT-4/5にアクセス可能な穴を発見・実装
- 短期間で213スター増加する高い成長性
- OpenAI互換で既存アプリへの統合が容易

実用性: 高 / 収益化: 低 / 日本市場: 中

https://github.com/sums001/Windows-Copilot-API

## 8. Johell1NS/browser-search（72点）

SearXNG、Camofox、CloakBrowserを組み合わせたAIエージェント向けスキル。セルフホスト可能で無料・無制限にウェブ検索とブラウジングを実現。保護回避機能で高い検索の自由度を提供。

- AIエージェントの検索能力を大幅に拡張できる統合ツール
- セルフホスト型で運用コスト無料、APIレート制限なし
- 複数の技術（検索エンジン、ブラウザ自動化、プロキシ）を組み合わせた実装

実用性: 高 / 収益化: 中 / 日本市場: 中

https://github.com/Johell1NS/browser-search

## 9. Huxiaoyou97/ManvoTV（72点）

無限キャンバス＋AI漫則脚本工場を備えたAIネイティブ創作プラットフォーム。テキストプロンプトから動画・ストーリーボード・脚本を自動生成。ショートドラマ制作の民主化を目指す。

- 無限キャンバスUIと脚本自動生成の組み合わせで、短動画制作の完全パイプライン化を実現
- AI Video×AI Agent×ストーリーボード生成の統合により、従来は高度な編集スキルが必要だった領域を大幅に簡略化
- 短編ドラマ市場（中国で急成長）への直接対応で、ニッチながら高い市場ポテンシャル

実用性: 中 / 収益化: 高 / 日本市場: 中

https://github.com/Huxiaoyou97/ManvoTV

## 10. Reyzowter/Hello-Agents（72点）

LangChain・OpenAI等を使用したAIエージェントシステムの包括的な実装チュートリアル。基礎から本番環境対応のマルチエージェント構築まで段階的に学習できる実践的なリソース。

- 直近79 Starsという急速な成長率（累計127の62%増加）から実装者の関心が高い
- LangChain・RAG・マルチエージェント等の現在需要の高い技術スタックをカバー
- 基礎から本番実装まで体系的に学べる実践的チュートリアルの不足を補完

実用性: 高 / 収益化: 中 / 日本市場: 中

https://github.com/Reyzowter/Hello-Agents


---

この記事は毎朝自動更新している [note版マガジン](https://note.com/lazy_engineer/m/m03a726b1673c) のクロス投稿です。

スコア85点以上の「当たり案件」については、収益化アイデア・日本市場での使い方まで踏み込んだ深掘り版をnoteで公開しています。
→ [AI Dev Radar 深掘りマガジン](https://note.com/lazy_engineer/m/m5b63f28ca6ad)
