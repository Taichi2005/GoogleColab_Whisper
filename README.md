# 🎤 GoogleColab_Whisper

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Taichi2005/GoogleColab_Whisper)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![GitHub Stars](https://img.shields.io/github/stars/Taichi2005/GoogleColab_Whisper?style=social)](https://github.com/Taichi2005/GoogleColab_Whisper)

Google Colab上でOpenAI Whisperを使用した**高精度な音声文字起こし**を実現するためのノートブックコレクション。  
YouTube動画やローカルファイルから、無料のGPUを活用して簡単に文字起こしを実行できます。

---

## 📋 目次

- [概要](#-概要)
- [主な機能](#-主な機能)
- [利用可能なノートブック](#-利用可能なノートブック)
- [クイックスタート](#-クイックスタート)
- [技術仕様](#-技術仕様)
- [モデル選択ガイド](#-モデル選択ガイド)
- [詳細設定ガイド](#-詳細設定ガイド)
- [システム要件](#️-システム要件)
- [トラブルシューティング](#-トラブルシューティング)
- [ライセンス](#-ライセンス)
- [貢献](#-貢献)

---

## 📝 概要

このリポジトリは、OpenAI Whisperを利用した文字起こしタスクを、Google Colabの無料GPUで簡単に実行できるように設計されています。  
複数のノートブックが用意されており、YouTube動画のURL指定からローカルファイルの一括処理まで、様々なユースケースに対応しています。

### なぜこのツールを使うのか？

- ✅ **無料で高精度**: Google ColabのT4 GPUを無料で活用
- ✅ **簡単セットアップ**: コードのコピー＆ペーストで即実行
- ✅ **柔軟な入力形式**: YouTube URL、プレイリスト、ローカルファイル対応
- ✅ **AI統合**: Gemini APIによる自動要約・分析機能搭載
- ✅ **高速処理**: 最適化モデルで従来比2-3倍の速度向上
- ✅ **日本語対応**: 日本語音声の高精度認識

---

## 🚀 主な機能

### コア機能

| 機能 | 説明 |
|------|------|
| 🎬 **YouTube動画対応** | URLを入力するだけで動画から文字起こし。プレイリスト一括処理にも対応 |
| 📂 **ローカルファイル一括処理** | Google Drive内の複数ファイル（音声・動画）を自動処理 |
| 🤖 **Gemini AI統合** | 文字起こし結果をGemini APIで要約・分析（9種類のモデルから選択） |
| 🎯 **複数モデル対応** | 用途に応じて最適なWhisperモデルを選択可能（10種類以上） |
| ⚡ **VAD（音声区間検出）** | 無音区間を自動除去して精度向上 |
| 🌐 **多言語対応** | 100以上の言語をサポート（自動検出または手動指定） |
| 📊 **詳細ログ出力** | タイムスタンプ付き処理ログで進捗を確認 |

### サポートされる入力形式

**音声ファイル**: MP3, WAV, M4A, FLAC, OGG, OPUS  
**動画ファイル**: MP4, MOV, AVI, WMV, MKV, FLV, WEBM

---

## 📚 利用可能なノートブック

このリポジトリには以下のノートブックが含まれています。用途に応じて選択してください。

### 🌟 推奨ノートブック（完成版）

#### 1. **動画URLから高精度文字起こし＆Gemini処理（プレイリスト対応版）**
   - **ファイル**: `notebooks/【完成版】動画URLから高精度文字起こし＆Gemini処理実行（プレイリスト対応版）.ipynb`
   - **用途**: YouTubeの動画やプレイリストから文字起こし + AI要約
   - **特徴**:
     - ✅ プレイリスト全体の一括処理
     - ✅ Gemini APIによる自動要約・分析
     - ✅ モデル選択可能（large-v3-turbo推奨）
     - ✅ VADフィルタ搭載
     - ✅ 自動クリーンアップ機能
   - **推奨対象**: YouTube動画の文字起こし＋AI要約が必要な方

#### 2. **高性能文字起こし実行スクリプト（モデル・量子化選択版）**
   - **ファイル**: `notebooks/【完成版】高性能文字起こし実行スクリプト_(モデル・量子化_選択肢追加版).ipynb`
   - **用途**: Google Driveのローカルファイル一括処理
   - **特徴**:
     - ✅ 動画・音声ファイル自動判別
     - ✅ 複数ファイルの一括処理（進捗バー表示）
     - ✅ モデル・量子化設定のカスタマイズ
     - ✅ Gemini統合（オプション）
     - ✅ 日本語ファイル名対応
   - **推奨対象**: ローカルファイルを大量に処理したい方

#### 3. **安定版：動画URLから高精度文字起こし**
   - **ファイル**: `notebooks/【安定版】動画URLから高精度文字起こし実行スクリプト.ipynb`
   - **用途**: シンプルなYouTube動画文字起こし
   - **特徴**:
     - ✅ 最小限の設定で実行
     - ✅ 安定性重視
     - ✅ 初心者向け
   - **推奨対象**: 初めて使う方、シンプルな機能が欲しい方

### 🔧 その他のノートブック

- **高性能文字起こし実行スクリプト_(large_v3_turbo対応版).ipynb**  
  large-v3-turbo特化版。最新の高速モデルに最適化

- **高性能文字起こし実行スクリプト_(動画ファイル自動対応版).ipynb**  
  動画ファイル自動変換対応。FFmpegで自動的に音声抽出

- **高性能・多機能_文字起こし実行スクリプト.ipynb**  
  多機能版。高度なカスタマイズが可能

- **文字起こしコード.ipynb**  
  基本機能のみのシンプル版。学習用に最適

### 📦 旧版ノートブック

`【旧版】` フォルダには過去のバージョンが保存されています。互換性が必要な場合のみご利用ください。

---

## 🚀 クイックスタート

### 必要なもの

- Googleアカウント
- Google Drive（結果保存用）
- （オプション）Gemini API Key（AI要約機能を使う場合）
  - [Google AI Studio](https://aistudio.google.com/app/apikey) で無料で取得可能

### 基本的な使い方（5分で開始）

#### ステップ1: ノートブックを開く

1. このリポジトリの `notebooks` フォルダから、使いたいノートブックを選択
2. 「Open in Colab」バッジをクリック、またはファイルをGoogle Colabにアップロード

#### ステップ2: GPU設定を確認

Google Colabで以下の手順を実行：

1. メニューから `ランタイム` → `ランタイムのタイプを変更`
2. `ハードウェアアクセラレータ` を **T4 GPU** に設定
3. `保存` をクリック

> **重要**: GPUを設定しないと処理が非常に遅くなります

#### ステップ3: セル1を実行（環境構築）

```python
# セル1を実行して必要なライブラリをインストール
# ✓ GPU確認
# ✓ faster-whisper (v1.0.3)
# ✓ yt-dlp (YouTube動画ダウンローダー)
# ✓ ffmpeg (音声・動画処理)
# ✓ google-generativeai (Gemini API)
```

実行時間: 約1〜2分

#### ステップ4: セル2を実行（Google Drive接続）

```python
# Google Driveにマウント
from google.colab import drive
drive.mount('/content/drive')
```

許可を求められたら、アカウントを選択して承認します。

#### ステップ5: メイン処理セルを実行

各ノートブックのメイン処理セル（`@title` マーク付き）で以下を設定：

**YouTube動画の場合**:
```python
video_url = "https://www.youtube.com/watch?v=XXXXX"  # YouTube URL
output_transcript_dir = "/content/drive/MyDrive/Whisper_Transcripts/output"
enable_playlist = True  # プレイリストの場合
```

**ローカルファイルの場合**:
```python
drive_audio_input_dir = "/content/drive/MyDrive/Whisper_Transcripts/input"
drive_transcript_output_dir = "/content/drive/MyDrive/Whisper_Transcripts/output"
```

セルを実行すると、自動で文字起こしが開始されます！

---

## 🔧 技術仕様

### 使用技術

| 技術 | バージョン | 説明 |
|------|------------|------|
| **OpenAI Whisper** | - | 音声認識モデル（faster-whisper実装） |
| **faster-whisper** | 1.0.3 | CTranslate2による高速化実装 |
| **yt-dlp** | 最新 | YouTube動画ダウンローダー |
| **FFmpeg** | - | 音声・動画処理ツール |
| **Google Gemini API** | - | AI要約・分析エンジン |
| **CUDA** | - | GPU加速（Google Colab T4） |

### アーキテクチャ

```
YouTube URL / ローカルファイル
        ↓
    yt-dlp / FFmpeg (音声抽出)
        ↓
    WAV形式に変換 (16kHz, モノラル)
        ↓
    faster-whisper (文字起こし)
        ↓
    タイムスタンプ付きテキスト出力
        ↓
   (オプション) Gemini API (要約・分析)
        ↓
    最終結果をGoogle Driveに保存
```

---

## 🎯 モデル選択ガイド

### Whisperモデル比較表

| モデル | 精度 | 速度 | メモリ | 処理時間<sup>*</sup> | 推奨用途 |
|--------|------|------|--------|---------------------|----------|
| **Zoont/faster-whisper-large-v3-turbo-int8-ct2** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | 0.5x | **最も推奨** - バランス最良 |
| **deepdml/faster-whisper-large-v3-turbo-ct2** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | 0.7x | 高精度・高速 |
| **RoachLin/kotoba-whisper-v2.2-faster** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐ | 1.0x | **日本語特化v2.2** - 日本語最高精度 |
| **kotoba-tech/kotoba-whisper-v2.0-faster** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐ | 1.0x | **日本語特化v2.0** - 日本語高精度 |
| **large-v3** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐ | 1.5x | 最高精度（やや遅い） |
| **large-v2** | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐ | 1.5x | 高精度（安定版） |
| **distil-large-v3** | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | 0.6x | 軽量・高速バランス型 |
| **medium** | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | 0.3x | 軽量タスク |
| **small** | ⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | 0.2x | 高速処理優先 |
| **base** | ⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | 0.1x | 最軽量 |
| **tiny** | ⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | 0.05x | テスト用 |

<sup>*</sup> 処理時間は音声1時間あたりの目安。0.5x = 30分で処理完了

### 計算タイプ（Compute Type）

| タイプ | 説明 | 推奨モデル | メモリ使用量 |
|--------|------|------------|--------------|
| **int8_float16** | 量子化最適化（推奨） | int8モデル全般 | 低 |
| **float16** | 標準精度 | float16モデル全般 | 中 |
| **int8** | 省メモリ | メモリ制約がある場合 | 最低 |
| **float32** | 最高精度 | 精度最優先の場合 | 高 |

### Geminiモデル選択ガイド

以下は利用可能なGeminiモデルの一覧です：

| モデル | 速度 | コンテキスト長 | コスト | 推奨用途 |
|--------|------|----------------|--------|----------|
| **gemini-flash-latest** | ⭐⭐⭐⭐⭐ | 標準 | 低 | **最推奨** - 常に最新のFlashモデル |
| **gemini-3.1-pro-preview** | ⭐⭐⭐ | 超大規模 | 高 | 最高品質の分析・要約 |
| **gemini-3.1-flash-lite-preview** | ⭐⭐⭐⭐⭐ | 標準 | 最低 | 高速・低コスト |
| **gemini-3-flash-preview** | ⭐⭐⭐⭐ | 標準 | 低 | バランス型 |
| **gemini-2.5-pro** | ⭐⭐⭐ | 大規模 | 高 | 高度な分析タスク |
| **gemini-2.5-flash** | ⭐⭐⭐⭐⭐ | 標準 | 低 | 高速処理 |
| **gemini-2.5-flash-lite** | ⭐⭐⭐⭐⭐ | 標準 | 最低 | 超高速・超低コスト |
| **gemini-2.0-flash** | ⭐⭐⭐⭐ | 標準 | 低 | 安定版・高速 |
| **gemini-2.0-flash-lite** | ⭐⭐⭐⭐⭐ | 標準 | 最低 | 安定版・超高速 |

> **推奨**: 一般的な用途には `gemini-flash-latest` を推奨します。常に最新のFlashモデルが自動で使用されます。

---

## 📖 詳細設定ガイド

### VAD（音声区間検出）設定

VADは無音区間を自動検出・除去して、文字起こし精度を向上させます。

```python
use_vad_filter = True  # VADを有効化
vad_min_silence_duration_ms = 200  # 200ms以上の無音を区切りとして認識
```

**推奨値**:
- 会議・講演: 200-500ms
- 音楽・歌詞: 100-200ms
- ポッドキャスト: 300-500ms
- 雑音が多い環境: 100-200ms

**効果**:
- ✅ 無音部分の誤認識を防止
- ✅ 文の区切りが自然になる
- ✅ 処理速度が向上

### 言語設定

```python
enable_language_specification = True
language_code = "ja"  # 日本語
```

**主要言語コード**:
- `ja`: 日本語
- `en`: 英語
- `zh`: 中国語（簡体字）
- `ko`: 韓国語
- `es`: スペイン語
- `fr`: フランス語
- `de`: ドイツ語
- `it`: イタリア語
- `pt`: ポルトガル語
- `ru`: ロシア語

[全言語コード一覧（100以上）](https://github.com/openai/whisper/blob/main/whisper/tokenizer.py)

**Tips**:
- 言語を指定すると、精度が向上します
- 自動検出でも高精度ですが、明確な言語がある場合は指定を推奨

### ビームサイズ調整

```python
beam_size = 5  # 推奨値
```

**設定の目安**:
- **1-3**: 高速だが精度低下（テスト・確認用）
- **5-7**: バランス（推奨）
- **8-10**: 高精度だが処理時間増（重要な文書）

**faster-whisperのデフォルト**: 5（バランスが良い）

### プレイリスト処理

YouTubeプレイリスト全体を一括処理できます：

```python
video_url = "https://www.youtube.com/playlist?list=XXXXX"
enable_playlist = True  # プレイリスト全体を処理
```

個別の動画だけを処理したい場合は `enable_playlist = False` に設定します。

**注意点**:
- プレイリストの動画数が多い場合、処理時間がかかります
- Google Colabの無料プランには時間制限があります（約12時間）

### Geminiプロンプト例

#### 要約
```python
gemini_prompt = "以下の書き起こしテキストを、主要なポイント3つに要約してください。"
```

#### 議事録作成
```python
gemini_prompt = "以下の会議書き起こしから、決定事項とアクションアイテムを抽出してください。"
```

#### Q&A抽出
```python
gemini_prompt = "以下の書き起こしから、質問と回答のペアを抽出してください。"
```

#### キーワード抽出
```python
gemini_prompt = "以下のテキストから重要なキーワードを10個抽出してください。"
```

#### 翻訳
```python
gemini_prompt = "以下の日本語テキストを英語に翻訳してください。"
```

---

## 🖥️ システム要件

### Google Colab環境（推奨）

- **GPU**: T4 GPU（無料プランで利用可能）
- **RAM**: 12GB以上推奨
- **ストレージ**: Google Drive空き容量（出力ファイル保存用）
- **ブラウザ**: Chrome, Firefox, Safari（最新版推奨）

### ローカル実行（非推奨）

ローカル環境で実行する場合:
- **GPU**: NVIDIA GPU（CUDA対応、8GB VRAM以上）
- **Python**: 3.8以上
- **CUDA**: 11.x または 12.x
- **OS**: Windows, Linux, macOS

> **注意**: ローカル実行は環境構築が複雑なため、Google Colabの使用を強く推奨します。

---

## ❓ トラブルシューティング

### よくある問題と解決方法

#### 1. GPU が認識されない

**症状**: `nvidia-smi` でエラーが表示される

**解決方法**:
1. `ランタイム` → `ランタイムのタイプを変更`
2. `ハードウェアアクセラレータ` を **T4 GPU** に変更
3. ランタイムを再起動

#### 2. メモリ不足エラー

**症状**: `CUDA out of memory` または `RuntimeError: Out of memory`

**解決方法**:
- 軽量モデルに変更: `distil-large-v3` または `medium`
- 計算タイプを変更: `int8_float16`
- ランタイムを再起動してメモリをクリア

#### 3. YouTube動画のダウンロード失敗

**症状**: `yt-dlp` エラー、`Unable to extract`

**解決方法**:
```python
# セル1で最新版に更新
!pip install -U yt-dlp
```

または、動画URLが正しいか確認してください。

#### 4. Gemini APIエラー

**症状**: `API Key invalid` または `403 Forbidden`

**解決方法**:
1. [Google AI Studio](https://aistudio.google.com/app/apikey) でAPIキーを取得
2. 正しいキーをコピー＆ペースト（スペースなし）
3. APIの利用制限・クォータを確認

#### 5. 文字起こし結果が不正確

**解決方法**:
- VADを有効化: `use_vad_filter = True`
- 言語を明示: `language_code = "ja"`
- ビームサイズを増やす: `beam_size = 7`
- より高精度なモデルを使用: `large-v3` または `kotoba-whisper-v2.0`（日本語）

#### 6. プレイリストの一部しか処理されない

**解決方法**:
```python
enable_playlist = True  # 必ず True に設定
```

また、プレイリストが公開設定になっているか確認してください。

#### 7. Google Driveのマウントに失敗

**症状**: `Mount failed`

**解決方法**:
1. ブラウザのポップアップブロックを解除
2. Googleアカウントの権限を許可
3. ランタイムを再起動してやり直し

#### 8. ファイル名が文字化けする

**症状**: 日本語ファイル名が「？？？」になる

**解決方法**:
- 最新版のノートブックを使用（UTF-8対応済み）
- ファイル名に特殊文字を使わない

---

## 📝 出力ファイル形式

### 文字起こし結果（.txt）

```
元ファイル名: example_video.mp4
処理完了日時: 2026-04-07 12:00:00 (月曜日)
使用モデル: Zoont/faster-whisper-large-v3-turbo-int8-ct2
計算タイプ: int8_float16
VADフィルター: 有効
検出言語: ja (確率: 0.99)

---

[0000.00s -> 0005.23s] こんにちは、今日はプロジェクトの進捗について説明します。
[0005.45s -> 0012.67s] 現在、フェーズ2が完了し、フェーズ3に移行する準備が整いました。
[0013.12s -> 0019.34s] 次回のミーティングは来週月曜日の午後2時です。
```

タイムスタンプ付きで、読みやすい形式で保存されます。

### Gemini処理結果（.txt）

```
■ 元ファイル名: example_video.mp4
■ 処理実行日時: 2026-04-07 12:05:00 (月曜日)
■ 使用モデル: gemini-flash-latest

---

【要約】

本動画では、プロジェクトの進捗について以下の3点が報告されています。

1. **フェーズ2の完了**: 計画通りにフェーズ2の全タスクが完了し、成果物が納品されました。
2. **フェーズ3への移行**: フェーズ3の準備が整い、来週から本格的に開始予定です。
3. **次回ミーティング**: 来週月曜日14時に進捗確認ミーティングを実施します。
```

---

## 🤝 貢献

バグ報告、機能リクエスト、プルリクエストを歓迎します！

### 貢献方法

1. このリポジトリをフォーク
2. 機能ブランチを作成 (`git checkout -b feature/amazing-feature`)
3. 変更をコミット (`git commit -m 'Add amazing feature'`)
4. ブランチにプッシュ (`git push origin feature/amazing-feature`)
5. プルリクエストを作成

### バグ報告

問題が発生した場合は、以下の情報を含めて報告してください：

- エラーメッセージの全文
- 使用したノートブック名
- 実行環境（Google Colab / ローカル）
- 再現手順

---

## 📄 ライセンス

このプロジェクトはMITライセンスの下で公開されています。  
詳細は [LICENSE](LICENSE) ファイルをご確認ください。

---

## 🙏 謝辞

- [OpenAI Whisper](https://github.com/openai/whisper) - 音声認識モデル
- [faster-whisper](https://github.com/guillaumekln/faster-whisper) - 高速化実装
- [yt-dlp](https://github.com/yt-dlp/yt-dlp) - YouTubeダウンローダー
- [Google Gemini](https://ai.google.dev/) - AI要約エンジン
- [Google Colab](https://colab.research.google.com/) - 無料GPU環境
- [CTranslate2](https://github.com/OpenNMT/CTranslate2) - 高速推論エンジン

---

## 📞 サポート

問題が発生した場合:
1. [Issues](https://github.com/Taichi2005/GoogleColab_Whisper/issues) で既存の問題を検索
2. 新しい問題を報告する際は、エラーメッセージと実行環境を記載してください
3. [USAGE.md](USAGE.md) で詳細な使い方を確認

---

## 📊 統計情報

- **対応言語**: 100以上
- **対応フォーマット**: 13種類（音声6種 + 動画7種）
- **処理速度**: リアルタイムの2-3倍（large-v3-turbo-int8使用時）
- **精度**: WER（単語誤り率）< 5%（日本語標準音声）、< 2.5%（kotoba-whisper-v2.2使用時）
- **Whisperモデル**: 11種類以上（日本語特化モデル2種含む）
- **Geminiモデル**: 9種類

---

## 🔄 更新履歴

### 主な更新内容

- ✅ faster-whisper v1.0.3対応
- ✅ Gemini API統合（9モデル対応）
- ✅ プレイリスト一括処理
- ✅ VADフィルタ最適化
- ✅ 日本語特化モデル（kotoba-whisper）追加
- ✅ エラーハンドリング改善
- ✅ 自動クリーンアップ機能

最新の変更については、[Commits](https://github.com/Taichi2005/GoogleColab_Whisper/commits) をご確認ください。

---

## ⭐ スター

このプロジェクトが役立った場合は、ぜひスターを付けてください！

[![GitHub stars](https://img.shields.io/github/stars/Taichi2005/GoogleColab_Whisper.svg?style=social&label=Star&maxAge=2592000)](https://github.com/Taichi2005/GoogleColab_Whisper/stargazers)

---

## 🎓 関連リソース

- [USAGE.md](USAGE.md) - 詳細な使い方ガイド
- [OpenAI Whisper公式ドキュメント](https://github.com/openai/whisper)
- [faster-whisperドキュメント](https://github.com/guillaumekln/faster-whisper)
- [Google Gemini APIドキュメント](https://ai.google.dev/docs)

---

**Made with ❤️ by [Taichi2005](https://github.com/Taichi2005)**
