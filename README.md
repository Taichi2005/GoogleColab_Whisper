# 🎤 GoogleColab_Whisper

Google Colab上でOpenAIのWhisperを使用した高精度な音声文字起こしを実行するためのノートブックコレクション

## 📝 概要

このリポジトリには、動画・音声ファイルから高精度な文字起こしを行うための、複数の機能を持つJupyter Notebookが含まれています。Google Colabの無料GPUを活用し、YouTubeの動画URLやローカルファイルから簡単に文字起こしができます。

## 🚀 主な機能

- 🎬 **YouTube動画対応**: URLを入力するだけで動画から文字起こし
- 📂 **プレイリスト対応**: YouTubeプレイリスト全体を一括処理（一部ノートブック）
- 🎯 **複数モデル対応**: Whisperの各種モデルから選択可能
  - **最適化モデル**: `Zoont/faster-whisper-large-v3-turbo-int8-ct2` (推奨)
  - **高速モデル**: `deepdml/faster-whisper-large-v3-turbo-ct2`
  - **標準モデル**: `large-v3`, `large-v2`, `distil-large-v3`, `medium`, `small`, `base`, `tiny`
- ⚡ **量子化オプション**: メモリ使用量を抑えた高速処理
  - `int8_float16` (推奨・高速)
  - `float16` (標準)
  - `int8` (省メモリ)
  - `float32` (高精度)
- 📁 **動画ファイル自動対応**: ローカルファイルのアップロードにも対応
- 🌐 **多言語対応**: 日本語をはじめ、多言語の文字起こしに対応

## 📚 ノートブック一覧

### ✨ 推奨ノートブック（最新版）

#### 1. [`【完成版】動画URLから高精度文字起こし実行スクリプト.ipynb`](notebooks/【完成版】動画URLから高精度文字起こし実行スクリプト.ipynb) ⭐
**YouTube動画URL専用の完全自動化ノートブック**

**主な特徴:**
- 🎬 **YouTube動画URL入力だけで完結**: URLを入力するだけで、ダウンロードから文字起こしまで全自動
- 📂 **プレイリスト一括処理対応**: YouTubeプレイリストのURL入力で、リスト内の全動画を自動処理
- 🚀 **yt-dlp統合**: 最新のyt-dlpで安定した動画ダウンロード
- 🎯 **VAD（音声区間検出）機能**: 無音区間を自動除去し、精度向上
- 🧹 **自動クリーンアップ**: 処理完了後に一時ファイルを自動削除
- ⚙️ **詳細設定可能**: beam_size、VADパラメータなど調整可能
- 💡 **初心者に最適**: 3つのセルを順に実行するだけの簡単操作

**推奨用途:** YouTube動画やプレイリストの文字起こしをしたい方、URLだけで手軽に処理したい方

---

#### 2. [`【完成版】高性能文字起こし実行セル_(モデル・量子化_選択肢追加版).ipynb`](notebooks/【完成版】高性能文字起こし実行スクリプト_(モデル・量子化_選択肢追加版).ipynb) ⭐
**Google Driveのローカルファイル専用の高性能ノートブック**

**主な特徴:**
- 📁 **Google Drive内のファイル一括処理**: 指定フォルダ内の全ての動画・音声ファイルを自動処理
- 🎬 **動画ファイル自動対応**: mp4, mov, avi, mkv, webmなど動画から自動で音声抽出
- 🎵 **音声ファイル直接対応**: wav, mp3などの音声ファイルもそのまま処理
- 🔧 **FFmpeg自動処理**: 動画ファイルから16kHz/モノラルに最適化して音声抽出
- 🎛️ **モデル・量子化の詳細設定**: 9種類のモデルと4種類の量子化タイプから選択可能
- ⚡ **バッチ処理最適化**: 複数ファイルを効率的に連続処理
- 📊 **詳細なログ出力**: 処理時間、使用モデル、計算タイプを記録
- 💪 **上級者向け**: beam_size、VAD設定など細かいパラメータ調整が可能

**推奨用途:** 既にダウンロード済みの動画・音声ファイルを大量に処理したい方、最大限のカスタマイズをしたい方

### 📦 その他のノートブック（旧版・参考用）

以下のノートブックは旧版または機能が限定されたバージョンです。基本的には上記の推奨版をご利用ください。

- [`動画URLから高精度文字起こし実行スクリプト.ipynb`](notebooks/動画URLから高精度文字起こし実行スクリプト.ipynb): 完成版の旧バージョン
- [`文字起こしコード.ipynb`](notebooks/文字起こしコード.ipynb): 基本的な文字起こしコード
- [`高性能・多機能_文字起こし実行セル.ipynb`](notebooks/高性能・多機能_文字起こし実行セル.ipynb): 多機能版（旧版）
- [`高性能文字起こし実行セル_(large_v3_turbo対応版).ipynb`](notebooks/高性能文字起こし実行セル_(large_v3_turbo対応版).ipynb): large-v3-turbo特化版（旧版）
- [`高性能文字起こし実行セル_(動画ファイル自動対応版).ipynb`](notebooks/高性能文字起こし実行セル_(動画ファイル自動対応版).ipynb): 動画ファイル対応版（旧版）

## 🎯 使い方

### クイックスタート

#### 🎬 YouTube動画から文字起こしする場合
**→ [`【完成版】動画URLから高精度文字起こし実行スクリプト.ipynb`](notebooks/【完成版】動画URLから高精度文字起こし実行スクリプト.ipynb)を使用**

```python
# 1. ランタイムの設定
# ランタイム → ランタイムのタイプを変更 → GPU (T4)を選択

# 2. セルを上から順に実行（3つのセル）
# - セル1: 環境構築（yt-dlp, faster-whisper, ffmpegをインストール）
# - セル2: Google Driveへの接続（任意）
# - セル3: URLを入力して実行

# 3. 動画URLを指定
video_url = "https://www.youtube.com/watch?v=xxxxx"
# プレイリストURLも対応:
# video_url = "https://www.youtube.com/playlist?list=xxxxx"
# enable_playlist = True にすると全動画を処理

# 4. 実行完了後、結果がテキストファイルとして保存されます
```

#### 📁 Google Drive内のファイルから文字起こしする場合
**→ [`【完成版】高性能文字起こし実行セル_(モデル・量子化_選択肢追加版).ipynb`](notebooks/【完成版】高性能文字起こし実行セル_(モデル・量子化_選択肢追加版).ipynb)を使用**

```python
# 1. ランタイムの設定
# ランタイム → ランタイムのタイプを変更 → GPU (T4)を選択

# 2. セルを上から順に実行（3つのセル）
# - セル1: 環境構築（faster-whisperとCUDAライブラリをインストール）
# - セル2: Google Driveへの接続（必須）
# - セル3: パスを設定して実行

# 3. Google Driveのフォルダパスを指定
drive_audio_input_dir = "/content/drive/MyDrive/Colab/Whisper_Transcripts/input_audio"
drive_transcript_output_dir = "/content/drive/MyDrive/Colab/Whisper_Transcripts/output_transcripts"

# 4. input_audioフォルダに動画・音声ファイルを配置
# 対応形式: mp4, mov, avi, mkv, webm, wav, mp3など

# 5. 実行すると、フォルダ内の全ファイルを自動処理
# 結果はoutput_transcriptsフォルダに保存されます
```

### 📤 出力形式

文字起こし結果は以下の形式で出力されます：
- `.txt`: プレーンテキスト形式
- ファイル名には元の動画/音声ファイル名が反映されます
- 出力ファイルには使用モデル、計算タイプ、処理時間などのメタ情報が含まれます

## 🔧 必要環境

- Google Colab（無料版で動作）
- GPU: T4推奨（無料枠で利用可能）
- インターネット接続

## 📦 使用ライブラリ

- [OpenAI Whisper](https://github.com/openai/whisper): 音声認識モデル
- [yt-dlp](https://github.com/yt-dlp/yt-dlp): YouTube動画ダウンロード
- [faster-whisper](https://github.com/guillaumekln/faster-whisper): 高速化版Whisper実装
- FFmpeg: 音声処理

## ⚠️ 注意事項

- Google Colabの無料版には使用制限があります
- 長時間の動画処理には時間がかかる場合があります
- YouTubeの利用規約を遵守してください
- 著作権に配慮した利用をお願いします

## 🤝 コントリビューション

バグ報告や機能追加の提案は、Issuesからお願いします。

## 📄 ライセンス

このプロジェクトはMITライセンスの下で公開されています。

## 👤 作成者

Taichi2005

---

**Star⭐をいただけると励みになります！**
