# 🎤 GoogleColab_Whisper

Google Colab上でOpenAIのWhisperを使用した高精度な音声文字起こしを実行するためのノートブックコレクション

## 📝 概要

このリポジトリには、動画・音声ファイルから高精度な文字起こしを行うための、複数の機能を持つJupyter Notebookが含まれています。Google Colabの無料GPUを活用し、YouTubeの動画URLやローカルファイルから簡単に文字起こしができます。

## 🚀 主な機能

- 🎬 **YouTube動画対応**: URLを入力するだけで動画から文字起こし
- 📂 **ローカルファイル一括処理**: Google Drive内の複数ファイルを自動処理
- 🤖 **Gemini AI統合**: 文字起こし結果をGemini APIで要約・分析（オプション）
- 🎯 **複数モデル対応**: Whisperの各種モデルから選択可能
  - **最適化モデル**: `Zoont/faster-whisper-large-v3-turbo-int8-ct2` (推奨・最速)
  - **高速モデル**: `deepdml/faster-whisper-large-v3-turbo-ct2`
  - **標準モデル**: `large-v3`, `large-v2`, `distil-large-v3`, `medium`, `small`, `base`, `tiny`
- ⚡ **量子化オプション**: メモリ使用量を抑えた高速処理
  - `int8_float16` (推奨・高速)
  - `float16` (標準)
  - `int8` (省メモリ)
  - `float32` (高精度)
- 🎥 **動画ファイル自動対応**: mp4, mov, avi, mkv, webmなどから自動で音声抽出
- 🎵 **音声ファイル直接対応**: wav, mp3などの音声ファイルもそのまま処理
- 🌐 **多言語対応**: 日本語をはじめ、多言語の文字起こしに対応
- 🎛️ **VAD（音声区間検出）**: 無音区間を自動除去して精度向上
- 🔧 **詳細パラメータ調整**: beam_size、VAD設定など細かく調整可能

## 📚 ノートブック一覧

### ✨ 推奨ノートブック（最新・完成版）

#### 1. [`【完成版】動画URLから高精度文字起こし実行スクリプト.ipynb`](notebooks/【完成版】動画URLから高精度文字起こし実行スクリプト.ipynb) ⭐
**YouTube動画URL専用の完全自動化ノートブック**

**主な特徴:**
- 🎬 **YouTube動画URL入力だけで完結**: URLを入力するだけで、ダウンロードから文字起こしまで全自動
- 🚀 **yt-dlp統合**: 最新のyt-dlpで安定した動画ダウンロード
- 🤖 **Gemini AI統合**: 文字起こし結果をGemini 2.5で要約・分析（オプション）
  - 対応モデル: `gemini-2.5-pro`, `gemini-2.5-flash`, `gemini-2.5-flash-lite`
  - カスタムプロンプトで柔軟な処理が可能
- 🎯 **VAD（音声区間検出）機能**: 無音区間を自動除去し、精度向上
  - 無音閾値をミリ秒単位で調整可能（100ms～2000ms）
- 🧹 **自動クリーンアップ**: 処理完了後に一時ファイルを自動削除（オプション）
- ⚙️ **詳細設定可能**: 
  - beam_size調整（1～10）で精度と速度のバランスを最適化
  - 言語コード指定で特定言語に最適化
  - 計算タイプ（int8_float16/float16/int8/float32）選択
- 💡 **初心者に最適**: 3つのセルを順に実行するだけの簡単操作

**推奨用途:** YouTube動画の文字起こしをしたい方、URLだけで手軽に処理したい方、Gemini AIで要約も同時に行いたい方

**デフォルト設定:**
- モデル: `deepdml/faster-whisper-large-v3-turbo-ct2`
- 計算タイプ: `float32`
- VAD: 有効（200ms閾値）
- beam_size: 7

---

#### 2. [`【完成版】高性能文字起こし実行スクリプト_(モデル・量子化_選択肢追加版).ipynb`](notebooks/【完成版】高性能文字起こし実行スクリプト_(モデル・量子化_選択肢追加版).ipynb) ⭐
**Google Driveのローカルファイル専用の高性能ノートブック**

**主な特徴:**
- 📁 **Google Drive内のファイル一括処理**: 指定フォルダ内の全ての動画・音声ファイルを自動処理
- 🎬 **動画ファイル自動対応**: mp4, mov, avi, wmv, mkv, flv, webmなど動画から自動で音声抽出
  - FFmpegで16kHz/モノラルに最適化して音声抽出
  - 動画ファイル検出時は自動的に音声抽出モードに切り替え
- 🎵 **音声ファイル直接対応**: wav, mp3などの音声ファイルもそのまま処理
- 🤖 **Gemini AI統合**: 文字起こし結果をGemini 2.5で要約・分析（オプション）
  - 対応モデル: `gemini-2.5-pro`, `gemini-2.5-flash`, `gemini-2.5-flash-lite`
  - カスタムプロンプトで会議録、講義、対話の要約に最適化
- 🔧 **9種類のモデルと4種類の量子化タイプから選択可能**:
  - モデル: Zoont/int8版、deepdml版、large-v3、large-v2、distil-large-v3、medium、small、base、tiny
  - 計算タイプ: int8_float16、float16、int8、float32
- ⚡ **バッチ処理最適化**: 複数ファイルを効率的に連続処理
- 📊 **詳細なログ出力**: 処理時間、使用モデル、計算タイプを記録
- 🎯 **VAD機能**: 無音区間を自動除去（200ms閾値、調整可能）
- 💪 **上級者向け**: beam_size（1～10）、VAD設定など細かいパラメータ調整が可能

**推奨用途:** 既にダウンロード済みの動画・音声ファイルを大量に処理したい方、最大限のカスタマイズをしたい方、動画ファイルから直接文字起こししたい方

**デフォルト設定:**
- モデル: `Zoont/faster-whisper-large-v3-turbo-int8-ct2`（推奨）
- 計算タイプ: `int8_float16`（最速）
- VAD: 有効（200ms閾値）
- beam_size: 5（バランス重視）

---

### 📦 旧版ノートブック（参考用）

以下のノートブックは旧版または機能が限定されたバージョンです。基本的には上記の推奨版（完成版）をご利用ください。

- [`【旧版】動画URLから高精度文字起こし実行スクリプト.ipynb`](notebooks/【旧版】動画URLから高精度文字起こし実行スクリプト.ipynb): 完成版のベースとなった旧バージョン（Gemini機能なし）
- [`【旧版】高性能文字起こし実行スクリプト_(モデル・量子化_選択肢追加版).ipynb`](notebooks/【旧版】高性能文字起こし実行スクリプト_(モデル・量子化_選択肢追加版).ipynb): 完成版のベースとなった旧バージョン（Gemini機能なし）
- [`動画URLから高精度文字起こし実行スクリプト.ipynb`](notebooks/動画URLから高精度文字起こし実行スクリプト.ipynb): 初期バージョン
- [`文字起こしコード.ipynb`](notebooks/文字起こしコード.ipynb): 基本的な文字起こしコード
- [`高性能・多機能_文字起こし実行スクリプト.ipynb`](notebooks/高性能・多機能_文字起こし実行スクリプト.ipynb): 多機能版（旧版）
- [`高性能文字起こし実行スクリプト_(large_v3_turbo対応版).ipynb`](notebooks/高性能文字起こし実行スクリプト_(large_v3_turbo対応版).ipynb): large-v3-turbo特化版（旧版）
- [`高性能文字起こし実行スクリプト_(動画ファイル自動対応版).ipynb`](notebooks/高性能文字起こし実行スクリプト_(動画ファイル自動対応版).ipynb): 動画ファイル対応版（旧版）

## 🎯 使い方

### クイックスタート

#### 🎬 YouTube動画から文字起こしする場合
**→ [`【完成版】動画URLから高精度文字起こし実行スクリプト.ipynb`](notebooks/【完成版】動画URLから高精度文字起こし実行スクリプト.ipynb)を使用**

1. **ランタイムの設定**
   - Google Colabでノートブックを開く
   - `ランタイム` → `ランタイムのタイプを変更` → `T4 GPU`を選択

2. **セルを上から順に実行**
   - **セル1: 環境構築** - yt-dlp、faster-whisper、ffmpeg、Gemini APIをインストール
   - **セル2: Google Driveへの接続（オプション）** - 結果をDriveに保存したい場合のみ実行
   - **セル3: URLから高精度文字起こし＆Gemini処理実行** - 設定と実行

3. **パラメータ設定例**
```python
# 1. 動画のURLと出力先の設定
video_url = "https://youtu.be/xxxxx"
output_transcript_dir = "/content/drive/MyDrive/Colab/Whisper_Transcripts/output_transcripts"

# 2. モデルとパフォーマンス設定
model_name = "deepdml/faster-whisper-large-v3-turbo-ct2"  # 高速モデル
compute_type = "float32"  # 計算タイプ

# 3. VAD (音声区間検出) 設定
use_vad_filter = True  # VADを有効化
vad_min_silence_duration_ms = 200  # 無音閾値（ミリ秒）

# 4. 言語設定（オプション）
enable_language_specification = False  # 自動検出
language_code = "ja"  # 日本語を指定する場合

# 5. 高度な設定
beam_size = 7  # 精度と速度のバランス（1～10）
cleanup_audio_file = True  # 処理後に音声ファイルを削除

# 6. Geminiによる処理の設定（オプション）
enable_gemini_processing = False  # Gemini機能を使う場合はTrue
gemini_api_key = ""  # GeminiのAPIキー
gemini_model = "gemini-2.5-flash"  # 使用するGeminiモデル
output_gemini_dir = "/content/drive/MyDrive/Colab/Whisper_Transcripts/gemini_outputs"
gemini_prompt = "以下の動画書き起こしテキストを、重要なポイントと動画の構成を含めて要約して最大コンテクストで出力してください。"
```

4. **実行完了後**
   - 文字起こし結果が`.txt`ファイルとして保存されます
   - Gemini処理を有効にした場合、要約結果も別ファイルとして保存されます

---

#### 📁 Google Drive内のファイルから文字起こしする場合
**→ [`【完成版】高性能文字起こし実行スクリプト_(モデル・量子化_選択肢追加版).ipynb`](notebooks/【完成版】高性能文字起こし実行スクリプト_(モデル・量子化_選択肢追加版).ipynb)を使用**

1. **ランタイムの設定**
   - Google Colabでノートブックを開く
   - `ランタイム` → `ランタイムのタイプを変更` → `T4 GPU`を選択

2. **セルを上から順に実行**
   - **セル1: 環境構築** - faster-whisper、CUDA、Gemini APIをインストール
   - **セル2: Google Driveへの接続（必須）** - Google Driveをマウント
   - **セル3: 高性能文字起こし＆Gemini処理実行** - 設定と実行

3. **パラメータ設定例**
```python
# 1. Google Driveのパス設定
drive_audio_input_dir = "/content/drive/MyDrive/Colab/Whisper_Transcripts/input_audio"
drive_transcript_output_dir = "/content/drive/MyDrive/Colab/Whisper_Transcripts/output_transcripts"

# 2. モデルとパフォーマンス設定
model_name = "Zoont/faster-whisper-large-v3-turbo-int8-ct2"  # 推奨モデル（最速）
compute_type = "int8_float16"  # int8モデルに最適

# 3. VAD (音声区間検出) 設定
use_vad_filter = True  # VADを有効化
vad_min_silence_duration_ms = 200  # 無音閾値（ミリ秒）

# 4. 言語設定（オプション）
enable_language_specification = False  # 自動検出
language_code = "ja"  # 日本語を指定する場合

# 5. 高度な設定
beam_size = 5  # バランスの取れた推奨値（1～10）

# 6. Geminiによる処理の設定（オプション）
enable_gemini_processing = False  # Gemini機能を使う場合はTrue
gemini_api_key = ""  # GeminiのAPIキー
gemini_model = "gemini-2.5-flash"  # 使用するGeminiモデル
drive_gemini_output_dir = "/content/drive/MyDrive/Colab/Whisper_Transcripts/gemini_outputs"
gemini_prompt = "以下の会議や講義、対話の書き起こしテキストを、重要なポイントや構成をまとめて要約して最大コンテクストで出力してください。"
```

4. **ファイルの準備**
   - `input_audio`フォルダに動画・音声ファイルを配置
   - 対応形式: **動画** (mp4, mov, avi, wmv, mkv, flv, webm) / **音声** (wav, mp3など)
   - 動画ファイルの場合、FFmpegが自動的に音声を抽出します

5. **実行完了後**
   - `output_transcripts`フォルダに文字起こし結果が保存されます
   - Gemini処理を有効にした場合、`gemini_outputs`フォルダに要約結果が保存されます
   - 複数ファイルがある場合、すべて自動的に処理されます

---

### 📤 出力形式

文字起こし結果は以下の形式で出力されます：

**文字起こしファイル (.txt)**
- ファイル名: `transcript_[元ファイル名]_[モデル名]_[タイムスタンプ].txt`
- 内容: プレーンテキスト形式の文字起こし結果
- メタ情報: 使用モデル、計算タイプ、処理時間などが含まれます

**Gemini処理結果 (.txt)** ※Gemini機能を有効にした場合
- ファイル名: `gemini_[元ファイル名]_[タイムスタンプ].txt`
- 内容: Geminiによる要約・分析結果
- カスタムプロンプトに応じた柔軟な出力

### 🎨 モデルとパフォーマンスの選び方

#### モデル選択ガイド
| モデル | 精度 | 速度 | メモリ | 推奨用途 |
|--------|------|------|--------|----------|
| `Zoont/faster-whisper-large-v3-turbo-int8-ct2` | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | **最推奨・総合バランス最高** |
| `deepdml/faster-whisper-large-v3-turbo-ct2` | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | 高精度・高速 |
| `large-v3` | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ | 最高精度 |
| `large-v2` | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ | 高精度 |
| `distil-large-v3` | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | 精度と速度のバランス |
| `medium` | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | 標準 |
| `small` | ⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | 高速・軽量 |
| `base` | ⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | 最速・最軽量 |
| `tiny` | ⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | 超高速・超軽量 |

#### 計算タイプ選択ガイド
| 計算タイプ | 速度 | メモリ | 精度 | 推奨組み合わせ |
|-----------|------|--------|------|----------------|
| `int8_float16` | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | **Zoont/int8モデル + int8_float16（推奨）** |
| `float16` | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | deepdmlモデル + float16 |
| `int8` | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | 省メモリ重視 |
| `float32` | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | 高精度重視 |

#### 推奨設定パターン

**パターン1: 総合推奨（速度・精度・メモリの最適バランス）**
```python
model_name = "Zoont/faster-whisper-large-v3-turbo-int8-ct2"
compute_type = "int8_float16"
beam_size = 5
```

**パターン2: 高精度重視**
```python
model_name = "deepdml/faster-whisper-large-v3-turbo-ct2"
compute_type = "float16"
beam_size = 7～10
```

**パターン3: 高速処理重視**
```python
model_name = "Zoont/faster-whisper-large-v3-turbo-int8-ct2"
compute_type = "int8_float16"
beam_size = 3～5
```

### 🤖 Gemini AI統合の活用方法

完成版の両ノートブックには、Gemini 2.5 AIによる自動要約・分析機能が統合されています。

#### 対応Geminiモデル
- `gemini-2.5-pro`: 最高品質の要約・分析（処理時間長め）
- `gemini-2.5-flash`: 高速で高品質（推奨）
- `gemini-2.5-flash-lite`: 超高速・軽量版

#### カスタムプロンプト例

**会議録の要約**
```python
gemini_prompt = "以下の会議の書き起こしから、決定事項、アクションアイテム、議論のポイントを箇条書きで抽出してください。"
```

**講義・プレゼンの要約**
```python
gemini_prompt = "以下の講義内容を、章立てして要約してください。各章で重要なキーワードも抽出してください。"
```

**動画コンテンツの構成分析**
```python
gemini_prompt = "以下の動画の書き起こしから、動画の構成（イントロ、本編、まとめ）を分析し、各セクションの要点をまとめてください。"
```

**多言語対応の要約**
```python
gemini_prompt = "以下の英語の書き起こしを日本語で要約し、重要なポイントを5つに絞って箇条書きで出力してください。"
```

#### Gemini機能の有効化手順
1. [Google AI Studio](https://makersuite.google.com/app/apikey)でAPIキーを取得
2. ノートブックの設定で`enable_gemini_processing = True`に変更
3. `gemini_api_key`にAPIキーを入力
4. `gemini_model`と`gemini_prompt`を用途に合わせて設定
5. 実行すると、文字起こし結果とGemini処理結果の両方が保存されます

## 🔧 必要環境

- **Google Colab**（無料版で動作）
- **GPU**: T4推奨（無料枠で利用可能）
- **インターネット接続**: モデルのダウンロードとYouTube動画取得に必要
- **Google Drive**: ローカルファイル版を使用する場合（推奨）
- **Gemini APIキー**: Gemini機能を使用する場合（オプション）

## 📦 使用ライブラリ

- [OpenAI Whisper](https://github.com/openai/whisper): 音声認識モデル
- [faster-whisper](https://github.com/guillaumekln/faster-whisper): CTranslate2ベースの高速化版Whisper実装
- [yt-dlp](https://github.com/yt-dlp/yt-dlp): YouTube動画ダウンロード（YouTube版ノートブック）
- [FFmpeg](https://ffmpeg.org/): 動画から音声抽出、音声ファイル前処理
- [Google Generative AI](https://ai.google.dev/): Gemini API統合（オプション）
- [tqdm](https://github.com/tqdm/tqdm): プログレスバー表示

## 💡 よくある質問（FAQ）

### Q1: どのノートブックを使えばいいですか？
**A:** 用途によって選んでください：
- **YouTube動画から文字起こし** → [`【完成版】動画URLから高精度文字起こし実行スクリプト.ipynb`](notebooks/【完成版】動画URLから高精度文字起こし実行スクリプト.ipynb)
- **Google Driveのファイルから文字起こし** → [`【完成版】高性能文字起こし実行スクリプト_(モデル・量子化_選択肢追加版).ipynb`](notebooks/【完成版】高性能文字起こし実行スクリプト_(モデル・量子化_選択肢追加版).ipynb)

### Q2: 動画ファイルから直接文字起こしできますか？
**A:** はい、可能です。[`【完成版】高性能文字起こし実行スクリプト_(モデル・量子化_選択肢追加版).ipynb`](notebooks/【完成版】高性能文字起こし実行スクリプト_(モデル・量子化_選択肢追加版).ipynb)は、mp4, mov, avi, wmv, mkv, flv, webm形式の動画から自動的に音声を抽出します。

### Q3: 無料版のGoogle Colabで使えますか？
**A:** はい、完全に無料版で動作します。T4 GPUを使用することで、高速に文字起こしができます。

### Q4: 処理時間はどのくらいかかりますか？
**A:** 
- **推奨設定**（Zoont/int8モデル + int8_float16）: 10分の動画 → 約2～3分
- **高精度設定**（large-v3 + float16）: 10分の動画 → 約5～7分
- 実際の処理時間は、動画の長さ、音質、GPU使用状況によって変動します。

### Q5: Gemini機能は必須ですか？
**A:** いいえ、オプションです。`enable_gemini_processing = False`（デフォルト）で、従来通りの文字起こしのみ実行されます。要約や分析が必要な場合のみ有効にしてください。

### Q6: エラーが出た場合は？
**A:** 
1. ランタイムタイプがGPU（T4）になっているか確認
2. Google Driveが正しくマウントされているか確認（ローカルファイル版）
3. ファイルパスが正しいか確認
4. 使用制限に達していないか確認（無料版の場合）

### Q7: 複数の動画を一度に処理できますか？
**A:** 
- **YouTube版**: 現在は1本ずつ処理（将来的にプレイリスト対応予定）
- **ローカルファイル版**: input_audioフォルダ内の全ファイルを自動的に一括処理

### Q8: 対応言語は？
**A:** Whisperがサポートする全ての言語（99言語以上）に対応しています。`language_code`で言語を指定することも、自動検出させることもできます。

## ⚠️ 注意事項

- Google Colabの無料版には**使用制限**があります（連続使用時間、1日の使用量など）
- 長時間の動画処理には時間がかかる場合があります
- YouTubeの**利用規約**を遵守してください
- **著作権**に配慮した利用をお願いします
- 動画のダウンロードは私的利用の範囲内で行ってください
- Gemini APIには**使用制限とコスト**があります（無料枠あり）

## 🚀 今後の予定

- [ ] プレイリスト一括処理機能の追加（YouTube版）
- [ ] 字幕ファイル（SRT/VTT）出力対応
- [ ] タイムスタンプ付き文字起こし
- [ ] 話者分離機能（diarization）
- [ ] より多くのGeminiモデル対応
- [ ] バッチ処理の進捗表示改善

## 🤝 コントリビューション

バグ報告や機能追加の提案は、[Issues](https://github.com/Taichi2005/GoogleColab_Whisper/issues)からお願いします。

## 📄 ライセンス

このプロジェクトは[MITライセンス](LICENSE)の下で公開されています。

## 📚 参考資料

- [OpenAI Whisper 公式リポジトリ](https://github.com/openai/whisper)
- [faster-whisper 公式ドキュメント](https://github.com/guillaumekln/faster-whisper)
- [Google Gemini API ドキュメント](https://ai.google.dev/)
- [使い方の詳細ガイド](USAGE.md)

## 👤 作成者

**Taichi2005**

## 🌟 サポート

このプロジェクトが役に立ったら、ぜひ**Star⭐**をお願いします！

---

**最終更新**: 2025年1月
