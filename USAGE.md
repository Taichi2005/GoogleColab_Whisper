# 📖 詳細な使い方ガイド - GoogleColab_Whisper

このガイドでは、GoogleColab_Whisperの各ノートブックの詳しい使い方を説明します。

---

## 📋 目次

1. [初めての方向け：クイックスタート](#1-初めての方向けクイックスタート)
2. [完成版ノートブックの詳細](#2-完成版ノートブックの詳細)
3. [モデルとパフォーマンスの選び方](#3-モデルとパフォーマンスの選び方)
4. [VAD（音声区間検出）の設定](#4-vad音声区間検出の設定)
5. [言語設定とビームサイズ](#5-言語設定とビームサイズ)
6. [Gemini AI統合の使い方](#6-gemini-ai統合の使い方)
7. [プレイリスト処理](#7-プレイリスト処理)
8. [ローカルファイルの一括処理](#8-ローカルファイルの一括処理)
9. [高度な設定とカスタマイズ](#9-高度な設定とカスタマイズ)
10. [トラブルシューティング](#10-トラブルシューティング)
11. [ベストプラクティス](#11-ベストプラクティス)

---

## 1. 初めての方向け：クイックスタート

### 🚀 5分で始める文字起こし

#### ステップ1: Google Colabの準備

1. **Googleアカウントでログイン**
2. **[Google Colab](https://colab.research.google.com/)** にアクセス
3. このリポジトリの `notebooks` フォルダから使いたいノートブックを開く

#### ステップ2: GPU設定（重要！）

> **必須**: GPUを有効にしないと処理が非常に遅くなります

1. Colabのメニューから `ランタイム` をクリック
2. `ランタイムのタイプを変更` を選択
3. **ハードウェアアクセラレータ** を `T4 GPU` に設定
4. `保存` をクリック

**確認方法**:
```python
# セル1で以下のコマンドが成功すればOK
!nvidia-smi
```

成功すると、GPUの情報（NVIDIA T4など）が表示されます。

#### ステップ3: 環境構築（セル1を実行）

最初のセルを実行すると、必要なライブラリが自動でインストールされます：

```python
# インストールされるもの：
# - faster-whisper v1.0.3（高速化されたWhisper実装）
# - yt-dlp（YouTube動画ダウンローダー）
# - ffmpeg（音声・動画処理ツール）
# - google-generativeai（Gemini API）
```

**実行時間**: 約1〜2分

**進捗の確認**:
- `▼ GPUの確認` でGPU情報が表示される
- `✅ 環境構築が完了しました` と表示されればOK

#### ステップ4: Google Driveに接続（セル2を実行）

```python
from google.colab import drive
drive.mount('/content/drive')
```

1. セルを実行すると、認証画面が表示されます
2. Googleアカウントを選択
3. 「Google Drive File Streamに接続」を許可

**確認方法**:
```python
!ls /content/drive/MyDrive
```
Google Driveのファイル一覧が表示されればOK。

#### ステップ5: メイン処理セルの設定と実行

**YouTube動画の場合**:

```python
#@title 🚀 URLから高精度文字起こし実行

# 1. 動画URLを入力
video_url = "https://www.youtube.com/watch?v=XXXXX"

# 2. 出力先フォルダを指定
output_transcript_dir = "/content/drive/MyDrive/Whisper_Transcripts/output"

# 3. モデルを選択（推奨: Zoont/faster-whisper-large-v3-turbo-int8-ct2）
model_name = "Zoont/faster-whisper-large-v3-turbo-int8-ct2"
compute_type = "int8_float16"

# 4. セルを実行！
```

**処理の流れ**:
1. モデルのロード（初回のみ時間がかかります）
2. 動画のダウンロード
3. 音声の抽出
4. 文字起こし実行
5. 結果をGoogle Driveに保存

---

## 2. 完成版ノートブックの詳細

### 2.1 動画URLから高精度文字起こし＆Gemini処理（プレイリスト対応版）

**ファイル名**: `【完成版】動画URLから高精度文字起こし＆Gemini処理実行（プレイリスト対応版）.ipynb`

**特徴**:
- ✅ YouTube動画/プレイリストの一括処理
- ✅ Gemini APIによる自動要約
- ✅ VADフィルタ搭載
- ✅ 自動クリーンアップ機能

**使い方**:

#### 基本設定

```python
# 1. 動画のURLと出力先
video_url = "https://www.youtube.com/watch?v=XXXXX"
output_transcript_dir = "/content/drive/MyDrive/Whisper_Transcripts/output_transcripts"

# 2. プレイリスト処理の有効化
enable_playlist = True  # プレイリスト全体を処理する場合
```

#### モデル設定

```python
# Whisperモデル（推奨設定）
model_name = "Zoont/faster-whisper-large-v3-turbo-int8-ct2"
compute_type = "int8_float16"

# VAD設定
use_vad_filter = True
vad_min_silence_duration_ms = 200  # 200ms以上の無音を区切りに
```

#### Gemini設定（オプション）

```python
# Gemini処理を有効化
enable_gemini_processing = True
gemini_api_key = "YOUR_API_KEY_HERE"  # Google AI Studioで取得
gemini_model = "gemini-flash-latest"  # 推奨モデル
output_gemini_dir = "/content/drive/MyDrive/Whisper_Transcripts/gemini_outputs"

# プロンプト例
gemini_prompt = "以下の動画書き起こしテキストを、重要なポイントと動画の構成を含めて要約してください。"
```

#### 実行

セルを実行すると以下の処理が自動で行われます：

1. **動画情報の取得**
   - URLからメタデータを抽出
   - プレイリストの場合は全動画をリストアップ

2. **各動画の処理**
   - 音声のダウンロード（yt-dlp）
   - WAV形式に変換
   - 文字起こし実行
   - タイムスタンプ付きテキストを生成

3. **Gemini処理**（有効な場合）
   - 文字起こし結果をGemini APIに送信
   - 要約・分析結果を取得
   - 別ファイルとして保存

4. **クリーンアップ**
   - 一時音声ファイルを自動削除

**出力例**:

```
/content/drive/MyDrive/Whisper_Transcripts/
├── output_transcripts/
│   ├── VIDEO_ID_タイトル.txt
│   └── VIDEO_ID2_タイトル2.txt
└── gemini_outputs/
    ├── VIDEO_ID_タイトル_gemini_output.txt
    └── VIDEO_ID2_タイトル2_gemini_output.txt
```

---

### 2.2 高性能文字起こし実行スクリプト（モデル・量子化選択版）

**ファイル名**: `【完成版】高性能文字起こし実行スクリプト_(モデル・量子化_選択肢追加版).ipynb`

**特徴**:
- ✅ Google Drive内のファイル一括処理
- ✅ 動画・音声の自動判別
- ✅ 進捗バー表示
- ✅ 日本語ファイル名対応

**使い方**:

#### フォルダ構成の準備

Google Drive内に以下のフォルダを作成：

```
/content/drive/MyDrive/Whisper_Transcripts/
├── input_audio/          # 処理したいファイルをここに配置
├── output_transcripts/   # 文字起こし結果の保存先
└── gemini_outputs/       # Gemini処理結果の保存先
```

#### ファイルの配置

以下の形式のファイルを `input_audio/` に配置：

**音声ファイル**: 
- MP3, WAV, M4A, FLAC, OGG, OPUS

**動画ファイル**: 
- MP4, MOV, AVI, WMV, MKV, FLV, WEBM

#### 設定と実行

```python
#@title 🚀 高性能文字起こし実行

# 1. フォルダパス設定
drive_audio_input_dir = "/content/drive/MyDrive/Whisper_Transcripts/input_audio"
drive_transcript_output_dir = "/content/drive/MyDrive/Whisper_Transcripts/output_transcripts"

# 2. モデル設定
model_name = "Zoont/faster-whisper-large-v3-turbo-int8-ct2"
compute_type = "int8_float16"

# 3. VAD設定
use_vad_filter = True
vad_min_silence_duration_ms = 200

# 4. 言語設定（日本語の場合）
enable_language_specification = True
language_code = "ja"

# 5. ビームサイズ
beam_size = 5  # 推奨値

# セルを実行！
```

**処理の流れ**:

1. **ファイル検索**
   - `input_audio/` 内の全メディアファイルを検出
   - 対応フォーマットのみをリストアップ

2. **各ファイルの処理**（進捗バー表示）
   - 動画の場合: FFmpegで音声抽出
   - 文字起こし実行
   - メタデータ付きテキストファイルを生成

3. **Gemini処理**（有効な場合）
   - 各ファイルの文字起こし結果を処理

4. **結果の保存**
   - Google Driveに自動保存
   - 処理完了ログを表示

**進捗表示例**:

```
2026-04-07 12:00:00 --- 3. 処理対象ファイルの検索 ---
✅ 15 件のメディアファイルを検出しました。

2026-04-07 12:00:05 --- 4. 文字起こし処理開始 ---

全体進捗: 20%|████████          | 3/15 [02:30<10:00, 50.0s/it]

■ 処理開始: lecture_01.mp4
  - 動画ファイルを検出。音声の抽出を開始...
  - 音声の抽出が完了
  - 文字起こしを実行中... (言語: ja, beam_size: 5, VAD: 有効)
  - 文字起こし結果を保存しました
  - ✅ Gemini処理完了
■ 処理完了 (125.34秒)
```

---

### 2.3 安定版：動画URLから高精度文字起こし

**ファイル名**: `【安定版】動画URLから高精度文字起こし実行スクリプト.ipynb`

**特徴**:
- ✅ シンプルな設定
- ✅ 安定性重視
- ✅ 初心者向け

**使い方**:

最小限の設定で実行できます：

```python
# 1. URLを入力
video_url = "https://www.youtube.com/watch?v=XXXXX"

# 2. 出力先を指定
output_dir = "/content/drive/MyDrive/Whisper_Transcripts/output"

# 3. 実行！（他の設定はデフォルト値を使用）
```

**推奨対象**:
- 初めて文字起こしを試す方
- 複雑な設定が不要な方
- 単一動画の処理のみ必要な方

---

## 3. モデルとパフォーマンスの選び方

### 3.1 Whisperモデル詳細比較

#### 🏆 推奨モデル（用途別）

| 用途 | 推奨モデル | 理由 |
|------|------------|------|
| **一般的な用途** | `Zoont/faster-whisper-large-v3-turbo-int8-ct2` | 速度・精度・メモリのバランスが最良 |
| **日本語専用** | `kotoba-tech/kotoba-whisper-v2.0-faster` または `RoachLin/kotoba-whisper-v2.2-faster` | 日本語に特化した最高精度 |
| **最高速度** | `distil-large-v3` | 高速かつ高精度 |
| **最高精度** | `large-v3` | 精度最優先（やや遅い） |
| **軽量・テスト用** | `medium` または `small` | 高速処理、テスト確認用 |

#### モデル詳細スペック

##### 1. Zoont/faster-whisper-large-v3-turbo-int8-ct2

```python
model_name = "Zoont/faster-whisper-large-v3-turbo-int8-ct2"
compute_type = "int8_float16"
```

**特徴**:
- ⭐ **最も推奨**するモデル
- int8量子化により高速化
- メモリ使用量が少ない
- 精度はほぼlarge-v3と同等

**性能**:
- 処理速度: 約0.5x（1時間の音声を30分で処理）
- 精度: WER < 5%（日本語標準音声）
- VRAM使用量: 約4GB

**推奨用途**: ほぼ全ての用途に対応

---

##### 2. deepdml/faster-whisper-large-v3-turbo-ct2

```python
model_name = "deepdml/faster-whisper-large-v3-turbo-ct2"
compute_type = "float16"
```

**特徴**:
- float16精度
- Zoontより若干遅いが高精度
- 安定性が高い

**性能**:
- 処理速度: 約0.7x
- 精度: WER < 4%
- VRAM使用量: 約6GB

**推奨用途**: 精度を重視する場合

---

##### 3. kotoba-tech/kotoba-whisper-v2.0-faster

```python
model_name = "kotoba-tech/kotoba-whisper-v2.0-faster"
compute_type = "float16"
```

**特徴**:
- 🇯🇵 **日本語特化モデル**
- 日本語の固有表現に強い
- 方言や訛りにも対応

**性能**:
- 処理速度: 約1.0x
- 精度: 日本語 WER < 3%（日本語データセットでトレーニング）
- VRAM使用量: 約6GB

**推奨用途**: 日本語の会議、講演、インタビューなど

---

##### 4. RoachLin/kotoba-whisper-v2.2-faster

```python
model_name = "RoachLin/kotoba-whisper-v2.2-faster"
compute_type = "float16"
```

**特徴**:
- 🇯🇵 **日本語特化モデル（v2.2改良版）**
- kotoba v2.0の改良版
- より高精度な日本語認識

**性能**:
- 処理速度: 約1.0x
- 精度: 日本語 WER < 2.5%
- VRAM使用量: 約6GB

**推奨用途**: 最高精度の日本語文字起こしが必要な場合

---

##### 5. large-v3

```python
model_name = "large-v3"
compute_type = "float16"
```

**特徴**:
- OpenAI公式の最新モデル
- 多言語対応
- 最高クラスの精度

**性能**:
- 処理速度: 約1.5x（1時間の音声を90分で処理）
- 精度: WER < 4%
- VRAM使用量: 約10GB

**推奨用途**: 精度最優先、多言語混在音声

---

##### 6. distil-large-v3

```python
model_name = "distil-large-v3"
compute_type = "float16"
```

**特徴**:
- large-v3を蒸留（軽量化）
- 高速で高精度のバランス型

**性能**:
- 処理速度: 約0.6x
- 精度: WER < 6%
- VRAM使用量: 約5GB

**推奨用途**: 高速処理と精度のバランス重視

---

##### 7. medium, small, base, tiny

```python
model_name = "medium"  # または small, base, tiny
compute_type = "float16"
```

**特徴**:
- 軽量モデル
- 高速処理
- 精度はやや低下

**性能比較**:

| モデル | 処理速度 | 精度 | VRAM | 用途 |
|--------|----------|------|------|------|
| medium | 0.3x | WER ~10% | 3GB | 軽量タスク |
| small | 0.2x | WER ~15% | 2GB | 高速処理 |
| base | 0.1x | WER ~20% | 1GB | テスト用 |
| tiny | 0.05x | WER ~30% | 1GB | 動作確認 |

---

### 3.2 計算タイプ（Compute Type）の選び方

#### int8_float16（推奨）

```python
compute_type = "int8_float16"
```

**特徴**:
- int8量子化モデル専用
- 最も効率的
- メモリ使用量が少ない

**対応モデル**:
- `Zoont/faster-whisper-large-v3-turbo-int8-ct2`

**推奨理由**: 速度・精度・メモリのバランスが最良

---

#### float16（標準）

```python
compute_type = "float16"
```

**特徴**:
- 標準的な精度
- ほぼ全てのモデルで使用可能
- バランスが良い

**対応モデル**:
- 全てのfloat16モデル

**推奨理由**: 汎用性が高い

---

#### int8（省メモリ）

```python
compute_type = "int8"
```

**特徴**:
- メモリ使用量が最も少ない
- やや精度低下
- VRAM制約がある場合に有効

**推奨理由**: メモリ不足エラーが出る場合

---

#### float32（最高精度）

```python
compute_type = "float32"
```

**特徴**:
- 最高精度
- 処理が遅い
- メモリ使用量が多い

**推奨理由**: 研究用、最高精度が必要な場合のみ

---

## 4. VAD（音声区間検出）の設定

### 4.1 VADとは？

VAD (Voice Activity Detection) は、音声の中から「話している部分」と「無音部分」を自動で判別する機能です。

**効果**:
- ✅ 無音部分の誤認識を防止
- ✅ 文の区切りが自然になる
- ✅ 処理速度が向上（無音をスキップ）
- ✅ 不要なノイズを除去

### 4.2 基本設定

```python
use_vad_filter = True  # VADを有効化（推奨）
vad_min_silence_duration_ms = 200  # 無音の最小継続時間（ミリ秒）
```

### 4.3 推奨値（用途別）

#### 会議・講演

```python
use_vad_filter = True
vad_min_silence_duration_ms = 300  # 300ms = 0.3秒
```

**理由**: 
- 話者の間が比較的長い
- 文の区切りが明確

---

#### ポッドキャスト・インタビュー

```python
use_vad_filter = True
vad_min_silence_duration_ms = 400  # 400ms = 0.4秒
```

**理由**:
- 話者交代時の間が長い
- バックグラウンド音楽がある場合がある

---

#### 音楽・歌詞

```python
use_vad_filter = True
vad_min_silence_duration_ms = 100  # 100ms = 0.1秒
```

**理由**:
- 歌詞の間が短い
- ブレスや短い休符を認識

---

#### 雑音が多い環境

```python
use_vad_filter = True
vad_min_silence_duration_ms = 150  # 150ms = 0.15秒
```

**理由**:
- 背景雑音を無音と誤認識させない
- 短い無音でも区切りとして認識

---

#### 早口・テンポが速い

```python
use_vad_filter = True
vad_min_silence_duration_ms = 100  # 100ms = 0.1秒
```

**理由**:
- 話速が速いと無音が短い
- 細かい区切りを検出

---

### 4.4 VADを無効にする場合

```python
use_vad_filter = False
```

**無効にする理由**:
- 音楽がメイン（歌詞の間が非常に短い）
- 環境音や効果音を含めて全て文字起こししたい
- VADが誤動作する場合

---

## 5. 言語設定とビームサイズ

### 5.1 言語設定

#### 言語を指定する場合（推奨）

```python
enable_language_specification = True
language_code = "ja"  # 日本語
```

**メリット**:
- 精度が向上
- 処理が高速化
- 言語の誤検出を防止

**主要言語コード**:

| 言語 | コード | 言語 | コード |
|------|--------|------|--------|
| 日本語 | `ja` | 英語 | `en` |
| 中国語（簡体） | `zh` | 韓国語 | `ko` |
| スペイン語 | `es` | フランス語 | `fr` |
| ドイツ語 | `de` | イタリア語 | `it` |
| ポルトガル語 | `pt` | ロシア語 | `ru` |
| アラビア語 | `ar` | ヒンディー語 | `hi` |
| タイ語 | `th` | ベトナム語 | `vi` |
| インドネシア語 | `id` | トルコ語 | `tr` |

[全言語コード一覧（100以上）](https://github.com/openai/whisper/blob/main/whisper/tokenizer.py)

---

#### 言語を自動検出する場合

```python
enable_language_specification = False
```

**メリット**:
- 多言語混在の音声に対応
- 言語が不明な場合に便利

**デメリット**:
- やや精度が落ちる可能性
- 処理がやや遅くなる

**自動検出の結果確認**:

実行後のログに以下のように表示されます：

```
検出言語: ja (確率: 0.99)
```

確率が0.9以上であれば、高精度で検出されています。

---

### 5.2 ビームサイズ

#### ビームサイズとは？

ビームサーチのビーム幅を指定するパラメータ。値が大きいほど精度が向上しますが、処理時間も増加します。

#### 推奨設定

```python
beam_size = 5  # faster-whisperのデフォルト（推奨）
```

#### 設定値の目安

| ビームサイズ | 精度 | 速度 | 推奨用途 |
|--------------|------|------|----------|
| 1 | 低 | 最速 | テスト・動作確認 |
| 3 | 中 | 速い | 速度優先 |
| **5** | **高** | **標準** | **推奨** |
| 7 | 高 | やや遅い | 精度重視 |
| 10 | 最高 | 遅い | 最高精度が必要な場合 |

#### 例：最高精度設定

```python
beam_size = 10
model_name = "large-v3"
compute_type = "float16"
use_vad_filter = True
vad_min_silence_duration_ms = 200
enable_language_specification = True
language_code = "ja"
```

**用途**: 論文、重要な会議、法的文書など

---

#### 例：高速処理設定

```python
beam_size = 3
model_name = "distil-large-v3"
compute_type = "float16"
use_vad_filter = True
vad_min_silence_duration_ms = 300
```

**用途**: 大量のファイル処理、下書き作成

---

## 6. Gemini AI統合の使い方

### 6.1 Gemini APIキーの取得

1. [Google AI Studio](https://aistudio.google.com/app/apikey) にアクセス
2. Googleアカウントでログイン
3. **「APIキーを作成」** をクリック
4. 生成されたキーをコピー

### 6.2 基本設定

```python
# Gemini処理を有効化
enable_gemini_processing = True

# APIキーを設定
gemini_api_key = "YOUR_API_KEY_HERE"  # ここに取得したキーを貼り付け

# モデルを選択
gemini_model = "gemini-flash-latest"  # 推奨

# 出力先フォルダ
output_gemini_dir = "/content/drive/MyDrive/Whisper_Transcripts/gemini_outputs"
```

### 6.3 Geminiモデルの選び方

#### 推奨モデル（用途別）

| 用途 | 推奨モデル | 理由 |
|------|------------|------|
| **一般的な要約** | `gemini-flash-latest` | 最新モデル、高速、低コスト |
| **高度な分析** | `gemini-3.1-pro-preview` | 最高品質、長文対応 |
| **大量処理** | `gemini-2.5-flash-lite` | 超高速、超低コスト |
| **バランス型** | `gemini-2.5-flash` | 速度と品質のバランス |

#### モデル詳細

##### gemini-flash-latest（最推奨）

```python
gemini_model = "gemini-flash-latest"
```

**特徴**:
- 常に最新のFlashモデル
- 高速処理
- コストパフォーマンス最良

**推奨用途**: ほぼ全ての用途

---

##### gemini-3.1-pro-preview

```python
gemini_model = "gemini-3.1-pro-preview"
```

**特徴**:
- 最高品質
- 超長文対応（200万トークン）
- 複雑な分析が可能

**推奨用途**: 学術論文、詳細分析

---

##### gemini-2.5-flash-lite

```python
gemini_model = "gemini-2.5-flash-lite"
```

**特徴**:
- 超高速
- 超低コスト
- 基本的な要約に最適

**推奨用途**: 大量のファイル処理

---

### 6.4 プロンプトの書き方

#### 基本構造

```python
gemini_prompt = "[指示内容]を[形式]で[詳細度]してください。"
```

#### プロンプト例集

##### 1. 要約（基本）

```python
gemini_prompt = "以下の書き起こしテキストを、重要なポイント3つに要約してください。"
```

**出力例**:
```
1. プロジェクトのフェーズ2が完了
2. 次回ミーティングは来週月曜日14時
3. 新機能のリリースは来月予定
```

---

##### 2. 要約（詳細）

```python
gemini_prompt = """
以下の動画書き起こしテキストを、以下の形式で要約してください：

1. 全体のサマリー（3-5文）
2. 主要なポイント（箇条書き、5つ）
3. 重要な数値データ
4. アクションアイテム
"""
```

---

##### 3. 議事録作成

```python
gemini_prompt = """
以下の会議の書き起こしから、議事録を作成してください。

【必須項目】
- 会議の目的
- 参加者（推測可能な場合）
- 決定事項
- 次回のアクション
- 課題・懸念事項
"""
```

---

##### 4. Q&A抽出

```python
gemini_prompt = """
以下のインタビュー書き起こしから、質問と回答のペアを抽出してください。

形式：
Q1: [質問内容]
A1: [回答内容]
Q2: ...
"""
```

---

##### 5. キーワード抽出

```python
gemini_prompt = """
以下のテキストから：
1. 重要なキーワード（10個）
2. 専門用語（5個）
3. 固有名詞（人名、組織名、製品名など）
を抽出してください。
"""
```

---

##### 6. 構造化

```python
gemini_prompt = """
以下の講演書き起こしを、以下の構造で整理してください：

# タイトル
## 導入
- ポイント1
- ポイント2

## 本論
### セクション1
- 内容

### セクション2
- 内容

## 結論
- まとめ
"""
```

---

##### 7. 翻訳

```python
gemini_prompt = "以下の日本語テキストを、自然な英語に翻訳してください。専門用語は英語のまま保持してください。"
```

---

##### 8. タイムスタンプ付き要約

```python
gemini_prompt = """
以下の書き起こしテキスト（タイムスタンプ付き）から、
主要な話題が変わる箇所を特定し、各セクションの要約を作成してください。

形式：
[00:00-05:23] トピック: XXX
要約: ...

[05:24-12:45] トピック: YYY
要約: ...
"""
```

---

### 6.5 エラー対処

#### API Key invalid

**原因**: APIキーが正しくない

**解決方法**:
1. [Google AI Studio](https://aistudio.google.com/app/apikey) で新しいキーを生成
2. コピー時にスペースが入っていないか確認
3. クォーテーションマークの中に正しく貼り付け

---

#### API クォータ超過

**原因**: 無料枠を使い切った

**解決方法**:
1. 翌日まで待つ（無料枠は1日ごとにリセット）
2. 有料プランにアップグレード
3. 別のAPIキーを使用

---

#### 応答が遅い

**原因**: モデルが重い、または入力が長すぎる

**解決方法**:
- 軽量モデルに変更: `gemini-2.5-flash-lite`
- 入力テキストを分割
- プロンプトを簡潔に

---

## 7. プレイリスト処理

### 7.1 基本設定

```python
# プレイリストのURL
video_url = "https://www.youtube.com/playlist?list=PLxxxxxxxxxxxxxxx"

# プレイリスト処理を有効化
enable_playlist = True
```

### 7.2 処理フロー

1. **プレイリスト情報の取得**
   - yt-dlpがプレイリスト内の全動画をリストアップ

2. **各動画の処理**
   - 1本ずつ順番に処理
   - 進捗状況をログに表示

3. **結果の保存**
   - 各動画の文字起こし結果を個別ファイルとして保存

### 7.3 処理例

**プレイリスト**:
```
https://www.youtube.com/playlist?list=PLxxxxxx
├── 動画1: 「Pythonの基礎」
├── 動画2: 「データ分析入門」
└── 動画3: 「機械学習の応用」
```

**実行ログ**:
```
✅ プレイリストを検出しました。3件の動画を処理します。

--- [1/3] 処理開始 ---
  - 対象: Pythonの基礎
  - URL: https://www.youtube.com/watch?v=xxxxx
    -> ✅ 文字起こし完了 (125.34秒)
    -> ✅ Gemini処理完了

--- [2/3] 処理開始 ---
  - 対象: データ分析入門
  ...
```

**出力ファイル**:
```
/content/drive/MyDrive/Whisper_Transcripts/output_transcripts/
├── xxxxx_Pythonの基礎.txt
├── yyyyy_データ分析入門.txt
└── zzzzz_機械学習の応用.txt
```

### 7.4 単一動画のみ処理する場合

プレイリスト内の特定の動画だけを処理したい場合：

```python
# 個別の動画URL
video_url = "https://www.youtube.com/watch?v=xxxxx"

# プレイリスト処理を無効化
enable_playlist = False
```

### 7.5 注意事項

#### 処理時間

プレイリストの動画数 × 1動画あたりの処理時間

**例**: 
- 10本のプレイリスト
- 各動画10分
- 処理速度0.5x（10分 → 5分で処理）
- 合計: 約50分

#### Google Colab の制限

**無料プラン**:
- 最大連続実行時間: 約12時間
- 大量のプレイリスト処理は複数回に分けて実行

**対策**:
- 処理済みの動画をスキップする機能はないため、手動で分割

---

## 8. ローカルファイルの一括処理

### 8.1 フォルダ構成

```
/content/drive/MyDrive/Whisper_Transcripts/
├── input_audio/              # ここにファイルを配置
│   ├── meeting_20260407.mp4
│   ├── lecture_01.m4a
│   ├── interview.wav
│   └── podcast_ep5.mp3
├── output_transcripts/        # 文字起こし結果
│   ├── meeting_20260407.txt
│   ├── lecture_01.txt
│   ├── interview.txt
│   └── podcast_ep5.txt
└── gemini_outputs/            # Gemini処理結果（オプション）
    ├── meeting_20260407_gemini_output.txt
    ├── lecture_01_gemini_output.txt
    ├── interview_gemini_output.txt
    └── podcast_ep5_gemini_output.txt
```

### 8.2 対応ファイル形式

**音声ファイル**:
- MP3, WAV, M4A, FLAC, OGG, OPUS

**動画ファイル**:
- MP4, MOV, AVI, WMV, MKV, FLV, WEBM

### 8.3 実行手順

#### ステップ1: ファイルをアップロード

Google Drive内の `input_audio/` フォルダに処理したいファイルを配置。

**方法1**: Web UIでアップロード
1. Google Driveを開く
2. `Whisper_Transcripts/input_audio/` に移動
3. ファイルをドラッグ＆ドロップ

**方法2**: Colabからアップロード
```python
from google.colab import files
uploaded = files.upload()
```

#### ステップ2: 設定

```python
# フォルダパス
drive_audio_input_dir = "/content/drive/MyDrive/Whisper_Transcripts/input_audio"
drive_transcript_output_dir = "/content/drive/MyDrive/Whisper_Transcripts/output_transcripts"

# モデル設定
model_name = "Zoont/faster-whisper-large-v3-turbo-int8-ct2"
compute_type = "int8_float16"

# VAD設定
use_vad_filter = True
vad_min_silence_duration_ms = 200

# 言語設定
enable_language_specification = True
language_code = "ja"
```

#### ステップ3: 実行

セルを実行すると、`input_audio/` 内の全ファイルが自動で処理されます。

### 8.4 進捗確認

```
2026-04-07 12:00:00 --- 3. 処理対象ファイルの検索 ---
✅ 4 件のメディアファイルを検出しました。

2026-04-07 12:00:05 --- 4. 文字起こし処理開始 ---

全体進捗: 25%|██████            | 1/4 [02:15<06:45, 135.0s/it]

■ 処理開始: meeting_20260407.mp4
  - 動画ファイルを検出。音声の抽出を開始...
  - 音声の抽出が完了 -> extracted_audio_meeting_20260407.wav
  - 文字起こしを実行中... (言語: ja, beam_size: 5, VAD: 有効)
  - 文字起こし結果を保存しました
  - Geminiによる処理を開始...
  - ✅ 処理結果を保存しました
■ 処理完了 (135.00秒)
```

### 8.5 エラー対処

#### ファイルが見つからない

**エラーメッセージ**:
```
⚠️ 入力フォルダに処理対象のメディアファイルが見つかりませんでした。
```

**原因**:
- フォルダパスが間違っている
- ファイルが配置されていない

**解決方法**:
```python
# フォルダ内を確認
!ls "/content/drive/MyDrive/Whisper_Transcripts/input_audio"
```

#### FFmpegエラー

**エラーメッセージ**:
```
💥 FFmpegエラー: 音声の抽出に失敗しました
```

**原因**:
- 動画ファイルが破損している
- 非対応のコーデック

**解決方法**:
- ファイルを別の形式に変換
- VLCなどで事前に確認

---

## 9. 高度な設定とカスタマイズ

### 9.1 出力ファイルのカスタマイズ

#### ファイル名のカスタマイズ

デフォルトでは以下の形式で保存されます：
```
{VIDEO_ID}_{タイトル}.txt
```

カスタマイズ例（コードを直接編集）:
```python
# タイムスタンプ付き
from datetime import datetime
timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
output_filename = f"{timestamp}_{safe_title}.txt"

# 日付のみ
date = datetime.now().strftime("%Y%m%d")
output_filename = f"{date}_{safe_title}.txt"
```

#### 出力内容のカスタマイズ

デフォルトのヘッダー:
```
元ファイル名: example.mp4
処理完了日時: 2026-04-07 12:00:00 (月曜日)
使用モデル: Zoont/faster-whisper-large-v3-turbo-int8-ct2
計算タイプ: int8_float16
VADフィルター: 有効
検出言語: ja (確率: 0.99)

---

[タイムスタンプ付き文字起こし]
```

カスタマイズ例:
```python
# タイムスタンプなしのテキストのみ
for segment in segments:
    f.write(f"{segment.text.strip()}\n")

# 時間範囲を秒数のみで表示
for segment in segments:
    f.write(f"[{segment.start:.0f}-{segment.end:.0f}] {segment.text.strip()}\n")

# 話者情報を追加（要：別途話者分離処理）
f.write(f"[話者A] {segment.text.strip()}\n")
```

### 9.2 バッチ処理の最適化

#### 複数のモデルで同じファイルを処理

```python
models = [
    ("Zoont/faster-whisper-large-v3-turbo-int8-ct2", "int8_float16"),
    ("kotoba-tech/kotoba-whisper-v2.0-faster", "float16"),
    ("large-v3", "float16")
]

for model_name, compute_type in models:
    print(f"モデル: {model_name}")
    model = WhisperModel(model_name, device="cuda", compute_type=compute_type)
    # 処理実行
    segments, info = model.transcribe(audio_file)
    # 結果を保存（モデル名を含める）
    output_path = f"{base_name}_{model_name.replace('/', '_')}.txt"
```

#### 並列処理（注意）

Google Colabでは1つのGPUしか使えないため、並列処理は効果がありません。  
複数ファイルの処理は逐次処理が最適です。

### 9.3 エクスポート形式の追加

#### SRT字幕形式

```python
def export_srt(segments, output_path):
    with open(output_path, 'w', encoding='utf-8') as f:
        for i, segment in enumerate(segments, 1):
            start = format_timestamp_srt(segment.start)
            end = format_timestamp_srt(segment.end)
            f.write(f"{i}\n")
            f.write(f"{start} --> {end}\n")
            f.write(f"{segment.text.strip()}\n\n")

def format_timestamp_srt(seconds):
    hours = int(seconds // 3600)
    minutes = int((seconds % 3600) // 60)
    secs = int(seconds % 60)
    millis = int((seconds % 1) * 1000)
    return f"{hours:02d}:{minutes:02d}:{secs:02d},{millis:03d}"
```

使用例:
```python
segments, info = model.transcribe(audio_file)
export_srt(segments, "output.srt")
```

#### VTT字幕形式

```python
def export_vtt(segments, output_path):
    with open(output_path, 'w', encoding='utf-8') as f:
        f.write("WEBVTT\n\n")
        for segment in segments:
            start = format_timestamp_vtt(segment.start)
            end = format_timestamp_vtt(segment.end)
            f.write(f"{start} --> {end}\n")
            f.write(f"{segment.text.strip()}\n\n")

def format_timestamp_vtt(seconds):
    hours = int(seconds // 3600)
    minutes = int((seconds % 3600) // 60)
    secs = seconds % 60
    return f"{hours:02d}:{minutes:02d}:{secs:06.3f}"
```

#### JSON形式

```python
import json

def export_json(segments, info, output_path):
    data = {
        "language": info.language,
        "language_probability": info.language_probability,
        "duration": info.duration,
        "segments": [
            {
                "id": i,
                "start": segment.start,
                "end": segment.end,
                "text": segment.text.strip()
            }
            for i, segment in enumerate(segments)
        ]
    }
    with open(output_path, 'w', encoding='utf-8') as f:
        json.dump(data, f, ensure_ascii=False, indent=2)
```

---

## 10. トラブルシューティング

### 10.1 GPU関連

#### GPU が認識されない

**確認コマンド**:
```python
!nvidia-smi
```

**エラー**: `NVIDIA-SMI has failed`

**解決方法**:
1. ランタイム → ランタイムのタイプを変更
2. ハードウェアアクセラレータ → T4 GPU
3. 保存
4. ランタイムを再起動

---

#### CUDA out of memory

**エラーメッセージ**:
```
RuntimeError: CUDA out of memory
```

**解決方法**:

**方法1**: 軽量モデルに変更
```python
model_name = "distil-large-v3"  # または medium, small
```

**方法2**: 計算タイプを変更
```python
compute_type = "int8_float16"  # または int8
```

**方法3**: メモリをクリア
```python
import torch
torch.cuda.empty_cache()
```

**方法4**: ランタイムを再起動
- ランタイム → ランタイムを再起動

---

### 10.2 YouTube関連

#### Unable to extract

**エラーメッセージ**:
```
ERROR: Unable to extract video data
```

**原因**:
- 動画が削除されている
- 非公開/限定公開
- yt-dlpのバージョンが古い

**解決方法**:
```python
# yt-dlpを最新版に更新
!pip install -U yt-dlp

# セル1から再実行
```

---

#### This video is unavailable

**原因**:
- 地域制限
- 年齢制限
- 著作権制限

**解決方法**:
- VPNを使用（非推奨）
- 別の動画を試す

---

### 10.3 文字起こし精度

#### 日本語の認識が悪い

**解決方法**:

**方法1**: 日本語特化モデルを使用
```python
model_name = "kotoba-tech/kotoba-whisper-v2.0-faster"
# または
model_name = "RoachLin/kotoba-whisper-v2.2-faster"
```

**方法2**: 言語を明示
```python
enable_language_specification = True
language_code = "ja"
```

**方法3**: ビームサイズを増やす
```python
beam_size = 7  # または 10
```

**方法4**: VADを調整
```python
use_vad_filter = True
vad_min_silence_duration_ms = 150  # 値を小さく
```

---

#### 固有名詞が正しく認識されない

**原因**:
- モデルが固有名詞を学習していない

**対処方法**:
- 文字起こし後に手動で修正
- Geminiに「固有名詞を正しく修正してください」と指示

```python
gemini_prompt = """
以下の書き起こしテキストの誤字・固有名詞を修正してください。
特に人名、会社名、製品名を正しく表記してください。
"""
```

---

#### 雑音が多い音声

**解決方法**:

**方法1**: VADを有効化
```python
use_vad_filter = True
vad_min_silence_duration_ms = 100  # 短めに設定
```

**方法2**: 事前に音声を編集
- Audacityなどでノイズ除去
- ボリュームを正規化

---

### 10.4 Gemini関連

#### API key not valid

**解決方法**:
1. [Google AI Studio](https://aistudio.google.com/app/apikey) で新しいキーを生成
2. スペースや改行が入っていないか確認
3. クォーテーションマークの中に正しく貼り付け

---

#### Resource exhausted

**エラーメッセージ**:
```
429 Resource exhausted
```

**原因**:
- API クォータ超過（無料枠を使い切った）

**解決方法**:
- 翌日まで待つ（無料枠は1日ごとにリセット）
- 有料プランにアップグレード
- リクエスト数を減らす

---

#### 応答が途中で切れる

**原因**:
- 入力テキストが長すぎる
- モデルの出力上限に達した

**解決方法**:
- 入力テキストを分割
- より大きなコンテキスト長のモデルを使用: `gemini-3.1-pro-preview`

---

### 10.5 ファイル関連

#### Google Drive mount failed

**解決方法**:
1. ブラウザのポップアップブロックを解除
2. シークレットモードではない通常モードで実行
3. ランタイムを再起動

---

#### Permission denied

**原因**:
- フォルダの権限がない
- パスが間違っている

**解決方法**:
```python
# フォルダを作成
import os
os.makedirs("/content/drive/MyDrive/Whisper_Transcripts/output", exist_ok=True)

# 権限を確認
!ls -la "/content/drive/MyDrive/Whisper_Transcripts"
```

---

#### 日本語ファイル名が文字化け

**解決方法**:
- 最新版のノートブックを使用（UTF-8対応済み）
- ファイル名を英数字のみに変更

---

## 11. ベストプラクティス

### 11.1 推奨ワークフロー

#### 初回実行

1. **テスト実行**
   - 短い動画（1-2分）で動作確認
   - モデル: `small` または `medium`
   - 設定が正しいか確認

2. **設定の最適化**
   - VAD設定を調整
   - モデルを選択
   - 出力を確認

3. **本番実行**
   - 推奨モデルで実行
   - 大量のファイルを処理

---

#### 大量ファイル処理

1. **バッチに分割**
   - 10-20ファイルずつ処理
   - Google Colabの時間制限を考慮

2. **進捗管理**
   - 処理済みファイルを別フォルダに移動
   - ログを保存

3. **エラーハンドリング**
   - エラーが出ても次のファイルに進む設定

---

### 11.2 パフォーマンス最適化

#### モデル選択

**一般的な用途**:
```python
model_name = "Zoont/faster-whisper-large-v3-turbo-int8-ct2"
compute_type = "int8_float16"
```

**日本語専用**:
```python
model_name = "RoachLin/kotoba-whisper-v2.2-faster"
compute_type = "float16"
```

**大量処理**:
```python
model_name = "distil-large-v3"
compute_type = "float16"
beam_size = 3
```

---

#### バッチ処理の工夫

```python
# ファイルをサイズでソート（小さいファイルから処理）
import os
files = sorted(files, key=lambda x: os.path.getsize(x))

# または長さでソート（短い動画から処理）
# 進捗が早く確認できる
```

---

### 11.3 品質管理

#### 文字起こし後の確認

1. **サンプル確認**
   - 冒頭、中盤、最後の3箇所をチェック
   - 固有名詞の正確性を確認

2. **統計情報**
   - 文字数、単語数をカウント
   - 極端に短い/長い場合はエラーの可能性

3. **Gemini要約の確認**
   - 元の内容と齟齬がないか確認

---

#### バックアップ

```python
# 重要なファイルは複数バックアップ
import shutil

# ローカルにコピー
shutil.copy(drive_output_path, "/content/backup/")

# 別のDriveフォルダにもコピー
shutil.copy(drive_output_path, "/content/drive/MyDrive/Backup/")
```

---

### 11.4 コスト最適化

#### Google Colab無料プランの効率的な使い方

1. **GPU使用時間の最小化**
   - モデルロード後、まとめて処理
   - 休憩時はランタイムを切断

2. **ディスク容量の管理**
   - 処理後は一時ファイルを削除
   - 定期的にクリーンアップ

3. **セッションの有効活用**
   - 12時間以内に収まるよう計画

---

#### Gemini APIの効率的な使い方

1. **モデル選択**
   - 一般的な要約: `gemini-flash-latest`
   - 大量処理: `gemini-2.5-flash-lite`

2. **プロンプトの最適化**
   - 簡潔で明確な指示
   - 不要な長文を避ける

3. **バッチ処理**
   - 複数の短いテキストをまとめて処理

---

### 11.5 セキュリティとプライバシー

#### APIキーの管理

```python
# ❌ 悪い例: ノートブックにハードコーディング
gemini_api_key = "AIzaSyXXXXXXXXXXXXXXX"

# ✅ 良い例: 環境変数やColabのSecretsを使用
from google.colab import userdata
gemini_api_key = userdata.get('GEMINI_API_KEY')
```

#### 機密情報

- 機密性の高い音声（会議、個人情報など）の取り扱いに注意
- Gemini APIに送信する前に、機密情報が含まれていないか確認
- 必要に応じて、Gemini処理をスキップ

---

## 📞 さらなるサポート

このガイドで解決しない問題がある場合:

1. **GitHubのIssues**: [Issues](https://github.com/Taichi2005/GoogleColab_Whisper/issues)
2. **README**: [README.md](README.md)
3. **公式ドキュメント**:
   - [faster-whisper](https://github.com/guillaumekln/faster-whisper)
   - [OpenAI Whisper](https://github.com/openai/whisper)
   - [Google Gemini](https://ai.google.dev/docs)

---

**Happy Transcribing! 🎤✨**
