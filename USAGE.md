# 📖 詳細な使い方ガイド

このガイドでは、GoogleColab_Whisperの各ノートブックの詳しい使い方を説明します。

## 目次
1. [初めての方向け：クイックスタート](#初めての方向けクイックスタート)
2. [完成版ノートブックの詳細](#完成版ノートブックの詳細)
3. [モデルとパフォーマンスの選び方](#モデルとパフォーマンスの選び方)
4. [Gemini AI統合の使い方](#gemini-ai統合の使い方)
5. [高度な設定とカスタマイズ](#高度な設定とカスタマイズ)
6. [トラブルシューティング](#トラブルシューティング)

---

## 初めての方向け：クイックスタート

### 🚀 5分で始める文字起こし

#### ステップ1: Google Colabの準備

1. **Googleアカウントでログイン**
2. **[Google Colab](https://colab.research.google.com/)にアクセス**
3. 以下のいずれかの方法でノートブックを開く：
   - GitHubから直接開く：`ファイル` → `ノートブックを開く` → `GitHub` タブ → `Taichi2005/GoogleColab_Whisper`を検索
   - ダウンロードしてアップロード：リポジトリから`.ipynb`ファイルをダウンロード → `ファイル` → `ノートブックをアップロード`

#### ステップ2: GPUの設定（重要！）

```
ランタイム → ランタイムのタイプを変更 → ハードウェアアクセラレータ → T4 GPU
```

💡 **ポイント**: GPUを有効にしないと、処理が非常に遅くなります。必ずT4 GPUを選択してください。

#### ステップ3: セルの実行

1. **セル1を実行**（環境構築）
   - `Shift + Enter`でセルを実行
   - ライブラリのインストールに約2～3分かかります
   - `✅ インストールが完了しました`と表示されるまで待機

2. **セル2を実行**（Google Drive接続）※オプション
   - Google Driveに結果を保存したい場合のみ実行
   - 認証画面が表示されたら、指示に従ってアクセスを許可

3. **セル3を実行**（文字起こし実行）
   - パラメータを設定（URLやファイルパスなど）
   - セルを実行すると、自動的に処理が開始されます

#### ステップ4: 結果の確認

- **実行完了**: `✅ 全ての処理が完了しました`と表示されます
- **結果ファイル**: 
  - Google Drive版：指定したフォルダに保存
  - ローカル版：左側のファイルパネルから確認・ダウンロード

---

## 完成版ノートブックの詳細

### 🎬 【完成版】動画URLから高精度文字起こし＆Gemini処理実行（プレイリスト対応版）.ipynb

**対象ユーザー**: 初心者～中級者、YouTubeプレイリスト・動画を文字起こししたい方

**このノートブックでできること**:
- ✅ YouTubeプレイリストURLから全動画を一括文字起こし
- ✅ YouTube単体動画URLにも対応
- ✅ yt-dlpによる安定した動画ダウンロード
- ✅ Gemini AIによる自動要約・分析（オプション）
- ✅ VADによる無音区間除去で精度向上
- ✅ 処理後の自動クリーンアップ
- ✅ Google Drive推奨（結果の永続保存、デフォルト設定で使用）

---

#### 📋 セル構成

**セル1: 環境構築**
```python
# GPUの確認
!nvidia-smi

# 必要なライブラリをインストール
# - yt-dlp: YouTube動画/プレイリストダウンロード
# - faster-whisper: 高速文字起こし
# - google-generativeai: Gemini API
# - ffmpeg: 音声処理
```

**実行時間**: 約2～3分

---

**セル2: Google Driveへの接続（推奨）**
```python
from google.colab import drive
drive.mount('/content/drive')
```

**注意**: 
- デフォルト設定ではGoogle Drive内のパスを使用します
- Google Driveに結果を保存したい場合は、このセルを実行してください
- ローカルパス（`/content/`）に変更すれば、Google Drive接続は不要です
- 認証が必要です（初回のみ）

**Google Driveフォルダの準備**:
以下のフォルダ構造を事前に作成してください
```
My Drive/
└── Whisper_Transcripts/
    ├── output_transcripts/   ← 文字起こし結果が保存される
    └── gemini_outputs/       ← Gemini処理結果が保存される（オプション）
```

---

**セル3: プレイリスト/単体動画URLから高精度文字起こし＆Gemini処理実行**

#### 基本設定

```python
#@title 🚀 プレイリスト/動画URLから高精度文字起こし＆Gemini処理実行

# 1. 動画/プレイリストのURLと出力先の設定（Google Drive必須）
video_url = "https://www.youtube.com/playlist?list=xxxxx"  # プレイリストURL
# または単体動画URL: video_url = "https://youtu.be/xxxxx"
output_transcript_dir = "/content/drive/My Drive/Whisper_Transcripts/output_transcripts"  #@param {type:"string"}

# 2. モデルとパフォーマンス設定
model_name = "Zoont/faster-whisper-large-v3-turbo-int8-ct2"  # デフォルト（推奨）
compute_type = "int8_float16"  # デフォルト（推奨）

# 3. VAD (音声区間検出) 設定
use_vad_filter = True  # 無音除去を有効化
vad_min_silence_duration_ms = 200  # 無音閾値（ミリ秒）

# 4. 言語設定（オプション）
enable_language_specification = False  # 自動検出
language_code = "ja"  # 日本語指定する場合

# 5. 高度な設定
beam_size = 5  # バランス重視（デフォルト）
cleanup_audio_file = True  # 処理後に音声ファイル削除

# 6. Geminiによる処理の設定（オプション）
enable_gemini_processing = False  # Gemini機能を使う場合True
gemini_api_key = ""  # GeminiのAPIキー
gemini_model = "gemini-2.5-flash"  # Geminiモデル選択
output_gemini_dir = "/content/drive/My Drive/Whisper_Transcripts/gemini_outputs"  #@param {type:"string"}
gemini_prompt = "以下の動画書き起こしテキストを、重要なポイントと動画の構成を含めて要約して最大コンテクストで出力してください。"
```

---

#### 使用例

**例1: プレイリスト全動画の一括処理**
```python
video_url = "https://www.youtube.com/playlist?list=PLxxxxxxxxxxxxxxx"
output_transcript_dir = "/content/drive/My Drive/Whisper_Transcripts/output_transcripts"

# デフォルト設定で実行
# model_name = "Zoont/faster-whisper-large-v3-turbo-int8-ct2"
# compute_type = "int8_float16"
# beam_size = 5
```

**例2: 単体動画URL（基本設定）**
```python
video_url = "https://www.youtube.com/watch?v=dQw4w9WgXcQ"
output_transcript_dir = "/content/drive/My Drive/Whisper_Transcripts/output_transcripts"
```

**例3: プレイリスト＋高精度設定**
```python
video_url = "https://www.youtube.com/playlist?list=PLxxxxxxxxxxxxxxx"
model_name = "Zoont/faster-whisper-large-v3-turbo-int8-ct2"  # 推奨モデル
compute_type = "int8_float16"  # 推奨計算タイプ
beam_size = 10  # 精度最優先
use_vad_filter = True
vad_min_silence_duration_ms = 150  # より細かい無音検出
```

**例4: プレイリスト＋Gemini要約付き**
```python
video_url = "https://www.youtube.com/playlist?list=PLxxxxxxxxxxxxxxx"
enable_gemini_processing = True
gemini_api_key = "YOUR_API_KEY_HERE"  # Google AI Studioで取得
gemini_model = "gemini-2.5-flash"  # 高速・高品質
gemini_prompt = "以下の動画内容を3つのポイントに要約してください。"
```

---

#### 処理フロー

1. **URL検証** → プレイリスト or 単体動画の判定
2. **出力ディレクトリ作成** → 保存先フォルダの準備
3. **Whisperモデルロード** → 約30秒～1分
4. **プレイリストの場合**: 全動画URLリストを取得
5. **各動画ごとに以下を実行**:
   - **yt-dlpで動画ダウンロード** → 動画サイズによる
   - **音声抽出** → FFmpegで音声のみ抽出
   - **文字起こし実行** → Whisperで文字起こし（最も時間がかかる）
   - **テキストファイル保存** → Google Driveに.txtで保存
   - **Gemini処理**（オプション）→ 要約・分析実行
   - **クリーンアップ** → 一時ファイル削除
6. **全動画処理完了** → 完了メッセージ表示

---

### 📁 【完成版】高性能文字起こし実行スクリプト_(モデル・量子化_選択肢追加版).ipynb

**対象ユーザー**: 中級者～上級者、大量のファイルを処理したい方

**このノートブックでできること**:
- ✅ Google Drive内の複数ファイルを一括処理
- ✅ 動画ファイル（mp4, mov, avi, wmv, mkv, flv, webm）から自動音声抽出
- ✅ 音声ファイル（wav, mp3など）を直接処理
- ✅ 9種類のモデルと4種類の量子化タイプから選択
- ✅ Gemini AIによる自動要約・分析（オプション）
- ✅ バッチ処理最適化

---

#### 📋 セル構成

**セル1: 環境構築**
```python
# GPUの確認
!nvidia-smi

# 必要なライブラリをインストール
# - faster-whisper: 高速文字起こし
# - nvidia-cublas-cu12, nvidia-cudnn-cu12: GPU最適化
# - google-generativeai: Gemini API
# FFmpegはColabにプリインストール済み
```

**実行時間**: 約2～3分

---

**セル2: Google Driveへの接続（必須）**
```python
from google.colab import drive
drive.mount('/content/drive')
```

**注意**: 
- このノートブックではGoogle Drive接続は**必須**です
- ファイルはDrive内に配置する必要があります

**Google Driveフォルダの準備**:
以下のフォルダ構造を事前に作成してください
```
My Drive/
└── Whisper_Transcripts/
    ├── input_audio/          ← ここに動画・音声ファイルを配置
    ├── output_transcripts/   ← 文字起こし結果が保存される
    └── gemini_outputs/       ← Gemini処理結果が保存される（オプション）
```

---

**セル3: 高性能文字起こし＆Gemini処理 実行セル**

#### 基本設定

```python
#@title 🚀 高性能文字起こし＆Gemini処理 実行セル

# 1. Google Driveのパス設定（必須）
drive_audio_input_dir = "/content/drive/My Drive/Whisper_Transcripts/input_audio"  #@param {type:"string"}
drive_transcript_output_dir = "/content/drive/My Drive/Whisper_Transcripts/output_transcripts"  #@param {type:"string"}

# 2. モデルとパフォーマンス設定
model_name = "Zoont/faster-whisper-large-v3-turbo-int8-ct2"  # 推奨（デフォルト）
compute_type = "int8_float16"  # 推奨（デフォルト）

# 3. VAD (音声区間検出) 設定
use_vad_filter = True  # 無音除去を有効化
vad_min_silence_duration_ms = 200  # 無音閾値（ミリ秒）

# 4. 言語設定（オプション）
enable_language_specification = False  # 自動検出
language_code = "ja"  # 日本語指定する場合

# 5. 高度な設定
beam_size = 5  # バランス重視（デフォルト）

# 6. Geminiによる処理の設定（オプション）
enable_gemini_processing = False  # Gemini機能を使う場合True
gemini_api_key = ""  # GeminiのAPIキー
gemini_model = "gemini-2.5-flash"  # Geminiモデル選択
drive_gemini_output_dir = "/content/drive/My Drive/Whisper_Transcripts/gemini_outputs"  #@param {type:"string"}
gemini_prompt = "以下の会議や講義、対話の書き起こしテキストを、重要なポイントや構成をまとめて要約して最大コンテクストで出力してください。"
```

---

#### ファイル配置方法

**対応ファイル形式**
- **動画**: mp4, mov, avi, wmv, mkv, flv, webm
- **音声**: wav, mp3, m4a, aac, flac, ogg など

**ファイル配置例**
```
input_audio/
├── meeting_2024-01-15.mp4
├── lecture_part1.mov
├── interview.mp3
└── presentation.wav
```

---

#### 使用例

**例1: 基本的な使い方（推奨設定）**
```python
# フォルダパス設定（Google Drive必須）
drive_audio_input_dir = "/content/drive/My Drive/Whisper_Transcripts/input_audio"
drive_transcript_output_dir = "/content/drive/My Drive/Whisper_Transcripts/output_transcripts"

# デフォルト設定で実行（最速・高精度バランス）
# model_name = "Zoont/faster-whisper-large-v3-turbo-int8-ct2"
# compute_type = "int8_float16"
# beam_size = 5
```

**例2: 最速処理設定**
```python
model_name = "Zoont/faster-whisper-large-v3-turbo-int8-ct2"
compute_type = "int8_float16"
beam_size = 3  # 最小ビームサイズ
use_vad_filter = False  # VAD無効で高速化
```

**例3: 動画ファイル大量処理＋Gemini要約**
```python
# input_audioフォルダに複数の動画ファイルを配置
model_name = "Zoont/faster-whisper-large-v3-turbo-int8-ct2"
compute_type = "int8_float16"
beam_size = 5

# Gemini要約を有効化
enable_gemini_processing = True
gemini_api_key = "YOUR_API_KEY_HERE"
gemini_model = "gemini-2.5-flash"
gemini_prompt = "以下の会議録から、決定事項とアクションアイテムを箇条書きで抽出してください。"
```

---

#### 処理フロー（各ファイルごと）

1. **ファイル検出** → input_audioフォルダ内のファイルをスキャン
2. **ファイルタイプ判定** → 動画 or 音声
3. **動画の場合**: FFmpegで音声抽出（16kHz/モノラル）
4. **音声の場合**: そのままコピー
5. **文字起こし実行** → Whisperで処理
6. **テキストファイル保存** → output_transcriptsに保存
7. **Gemini処理**（オプション）→ gemini_outputsに保存
8. **次のファイルへ** → 全ファイル完了まで繰り返し
9. **クリーンアップ** → 一時ファイル削除

---

## モデルとパフォーマンスの選び方

### 📊 モデル選択ガイド

#### 推奨モデル（用途別）

| 用途 | モデル | 計算タイプ | beam_size | 理由 |
|------|--------|-----------|-----------|------|
| **総合推奨** | `Zoont/faster-whisper-large-v3-turbo-int8-ct2` | `int8_float16` | 5 | 速度・精度・メモリの最適バランス |
| **高速処理** | `Zoont/faster-whisper-large-v3-turbo-int8-ct2` | `int8_float16` | 3 | 量子化による高速化 |
| **長時間動画** | `Zoont/faster-whisper-large-v3-turbo-int8-ct2` | `int8_float16` | 5 | メモリ効率が良い |

---

### ⚙️ 計算タイプの選び方

| 計算タイプ | 速度 | メモリ | 精度 | 推奨モデル | 説明 |
|-----------|------|--------|------|-----------|------|
| `int8_float16` | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | Zoont/int8モデル | **最速・最省メモリ**。精度もほぼ劣化なし |
| `float16` | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | deepdmlモデル、標準モデル | 標準的なバランス |
| `int8` | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | 全モデル | 省メモリ重視、精度やや低下 |
| `float32` | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | 標準モデル | 最高精度、処理遅い |

---

### 🎯 beam_sizeの選び方

`beam_size`は精度と速度のトレードオフを調整するパラメータです。

| beam_size | 速度 | 精度 | 推奨用途 |
|-----------|------|------|----------|
| 1～3 | ⭐⭐⭐⭐⭐ | ⭐⭐ | 超高速処理が必要な場合 |
| 5 | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | **推奨バランス値（デフォルト）** |
| 7 | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | 精度重視 |
| 10 | ⭐⭐ | ⭐⭐⭐⭐⭐ | 最高精度、処理時間長い |

💡 **推奨設定**: 通常は`5`、精度重視なら`7～10`、速度重視なら`3`

---

### 📈 処理時間の目安

**推奨設定**（Zoont/int8モデル + int8_float16 + beam_size=5）
- 10分の動画: 約2～3分
- 30分の動画: 約6～9分
- 1時間の動画: 約12～18分

※ GPU使用状況、音声の複雑さによって変動します

---

## Gemini AI統合の使い方

### 🤖 Gemini APIの準備

1. **APIキーの取得**
   - [Google AI Studio](https://makersuite.google.com/app/apikey)にアクセス
   - `Create API Key`をクリック
   - APIキーをコピー

2. **ノートブックでの設定**
```python
enable_gemini_processing = True
gemini_api_key = "YOUR_API_KEY_HERE"  # ここにAPIキーを貼り付け
gemini_model = "gemini-2.5-flash"  # モデル選択
gemini_prompt = "カスタムプロンプト"  # 処理内容を指定
```

---

### 📝 Geminiモデルの選び方

| モデル | 速度 | 品質 | コスト | 推奨用途 |
|--------|------|------|--------|----------|
| `gemini-2.5-pro` | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | 高 | 最高品質の要約・分析が必要な場合 |
| `gemini-2.5-flash` | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | 中 | **推奨バランス型** |
| `gemini-2.5-flash-lite` | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | 低 | 超高速・軽量版 |

---

### 💡 カスタムプロンプトの例

#### 会議録の要約
```python
gemini_prompt = """
以下の会議の書き起こしから、以下の項目を抽出してください：
1. 決定事項（箇条書き）
2. アクションアイテムと担当者
3. 議論のポイント
4. 次回までの宿題
"""
```

#### 講義・プレゼンの要約
```python
gemini_prompt = """
以下の講義内容を以下の形式で要約してください：
1. 全体のテーマ
2. 章立て（各章のタイトルと要点）
3. 重要なキーワード（10個）
4. 学習ポイント（5つ）
"""
```

#### YouTube動画の構成分析
```python
gemini_prompt = """
以下の動画の書き起こしを分析し、以下を出力してください：
1. 動画の構成（イントロ、本編、まとめ）
2. 各セクションの時間配分の推定
3. 主要なメッセージ（3つ）
4. 視聴者へのアクションコール
"""
```

#### インタビューの要約
```python
gemini_prompt = """
以下のインタビューから、以下を抽出してください：
1. インタビュイーの主張（3つ）
2. 重要な引用（5つ）
3. エピソードの要約
4. 全体のトーン（ポジティブ/ネガティブ/中立）
"""
```

#### 多言語対応
```python
gemini_prompt = """
以下の英語の書き起こしを日本語で要約してください：
1. 主要な論点を5つに絞って箇条書き
2. 各論点の詳細説明（100文字程度）
3. 結論
"""
```

---

### 🎨 Gemini出力のカスタマイズ

**構造化された出力**
```python
gemini_prompt = """
以下の内容をMarkdown形式で出力してください：

# タイトル
## 概要
- ポイント1
- ポイント2

## 詳細
### セクション1
内容

### セクション2
内容

## まとめ
"""
```

**表形式の出力**
```python
gemini_prompt = """
以下の内容から、表形式で情報を整理してください：

| トピック | 詳細 | 重要度 |
|---------|------|--------|
| ...     | ...  | ...    |
"""
```

---

## 高度な設定とカスタマイズ

### 🎛️ VAD（音声区間検出）の調整

**VAD設定のパラメータ**
```python
use_vad_filter = True  # VADを有効化
vad_min_silence_duration_ms = 200  # 無音閾値（ミリ秒）
```

**推奨設定**:
- **通常**: `200ms` - バランスが良い（デフォルト）
- **雑音が多い環境**: `300～500ms` - ノイズを無視
- **短い発話が多い**: `100～150ms` - 細かく検出
- **講義・プレゼン**: `200～300ms` - 適度な区切り
- **対話・会議**: `150～200ms` - 発話の切れ目を検出

**VADのメリット**:
- ✅ 無音区間を除去して処理時間短縮
- ✅ 文字起こしの精度向上
- ✅ 出力テキストの読みやすさ向上

---

### 🌐 言語指定のカスタマイズ

**自動検出（推奨）**
```python
enable_language_specification = False
```

**特定言語指定**
```python
enable_language_specification = True
language_code = "ja"  # 日本語
# language_code = "en"  # 英語
# language_code = "zh"  # 中国語
# language_code = "ko"  # 韓国語
# language_code = "es"  # スペイン語
# language_code = "fr"  # フランス語
```

💡 **ポイント**: 
- 言語が明確な場合は指定すると精度向上
- 多言語が混在する場合は自動検出推奨
- [対応言語一覧](https://github.com/openai/whisper/blob/main/whisper/tokenizer.py)

---

### 🗂️ ファイル名と出力形式

**出力ファイル名の構造**
```
transcript_[元ファイル名]_[モデル名]_[タイムスタンプ].txt
```

**例**:
```
transcript_meeting_2024-01-15_Zoont-int8_20250128_103045.txt
gemini_meeting_2024-01-15_20250128_103045.txt
```

**ファイル内容の構造**
```
=== 文字起こし結果 ===
モデル: Zoont/faster-whisper-large-v3-turbo-int8-ct2
計算タイプ: int8_float16
処理時間: 123.45秒
元ファイル: meeting_2024-01-15.mp4

---

[文字起こしテキスト本文]
```

---

## トラブルシューティング

### ❌ よくあるエラーと解決方法

#### 1. GPU関連のエラー

**エラーメッセージ**:
```
RuntimeError: CUDA out of memory
```

**解決方法**:
- ✅ ランタイムを再起動: `ランタイム` → `ランタイムを再起動`
- ✅ より軽量なモデルを使用: `small`や`base`モデル
- ✅ 計算タイプを変更: `int8`や`int8_float16`
- ✅ beam_sizeを減らす: `3`に設定

---

#### 2. yt-dlpダウンロードエラー（YouTube版）

**エラーメッセージ**:
```
ERROR: unable to download video data
```

**解決方法**:
- ✅ URLが正しいか確認
- ✅ 動画が削除されていないか確認
- ✅ 地域制限がかかっていないか確認
- ✅ yt-dlpを最新版に更新（セル1を再実行）

---

#### 3. Google Drive接続エラー

**エラーメッセージ**:
```
FileNotFoundError: [Errno 2] No such file or directory
```

**解決方法**:
- ✅ Google Driveが正しくマウントされているか確認
- ✅ フォルダパスが正しいか確認（スペルミス、全角/半角）
- ✅ フォルダが存在するか確認（事前に作成）
- ✅ セル2（Drive接続）を実行したか確認

---

#### 4. Gemini APIエラー

**エラーメッセージ**:
```
google.api_core.exceptions.PermissionDenied: 403
```

**解決方法**:
- ✅ APIキーが正しいか確認
- ✅ APIキーの権限が有効か確認
- ✅ 使用制限に達していないか確認（無料枠）
- ✅ `enable_gemini_processing = True`になっているか確認

---

#### 5. 処理が途中で止まる

**症状**: 
- プログレスバーが動かない
- 何も出力されない

**解決方法**:
- ✅ GPU使用率を確認: `!nvidia-smi`を実行
- ✅ Colab無料版の使用制限に達している可能性
- ✅ ランタイムを再起動して再実行
- ✅ 長時間動画の場合は分割処理を検討

---

#### 6. 文字起こし精度が低い

**症状**:
- 誤字・脱字が多い
- 言語が混在している
- 意味不明な出力

**解決方法**:
- ✅ より高精度なモデルを使用: `large-v3`
- ✅ beam_sizeを増やす: `7～10`
- ✅ 言語を明示的に指定: `enable_language_specification = True`
- ✅ VADを有効化: `use_vad_filter = True`
- ✅ 音声品質を確認（ノイズ、音量）

---

### 💡 パフォーマンス最適化のヒント

#### メモリ不足を避ける
```python
# 推奨設定
model_name = "Zoont/faster-whisper-large-v3-turbo-int8-ct2"
compute_type = "int8_float16"
beam_size = 5
```

#### 処理速度を最大化
```python
model_name = "Zoont/faster-whisper-large-v3-turbo-int8-ct2"
compute_type = "int8_float16"
beam_size = 3
use_vad_filter = False  # VAD無効で高速化
```

---

### 🔍 デバッグ方法

**1. GPU状態の確認**
```python
!nvidia-smi
```

**2. ファイル存在確認**
```python
import os
print(os.path.exists("/content/drive/MyDrive/..."))
print(os.listdir("/content/drive/MyDrive/..."))
```

**3. モデルロードテスト**
```python
from faster_whisper import WhisperModel
model = WhisperModel("base", device="cuda", compute_type="float16")
print("✅ モデルロード成功")
```

---

## 📚 追加リソース

- [OpenAI Whisper 公式ドキュメント](https://github.com/openai/whisper)
- [faster-whisper GitHub](https://github.com/guillaumekln/faster-whisper)
- [Google Gemini API ドキュメント](https://ai.google.dev/)
- [yt-dlp GitHub](https://github.com/yt-dlp/yt-dlp)

---

## 🎓 学習ステップ

### 初心者向けの学習パス

**ステップ1**: YouTube版で基本を理解
- デフォルト設定で1本の動画を文字起こし
- 結果を確認

**ステップ2**: パラメータをカスタマイズ
- beam_sizeを変えて精度と速度の違いを体験
- VAD設定を調整

**ステップ3**: ローカルファイル版で複数処理
- Google Driveに複数ファイルを配置
- バッチ処理を体験

**ステップ4**: Gemini統合を試す
- APIキーを取得
- カスタムプロンプトで要約

**ステップ5**: 高度な設定を探求
- 異なるモデルを比較
- 用途に応じた最適設定を見つける

---

**最終更新**: 2025年10月（プレイリスト対応完成版リリース）
