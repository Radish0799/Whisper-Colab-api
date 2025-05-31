# Whisper-Colab-api

## 概要
このリポジトリは、Google Colab 上で OpenAI Whisper モデルをデプロイし、音声の文字起こしを API 経由で行うためのサンプル実装です。  
Flask サーバーと ngrok を使って外部からアクセス可能な API を立てています。

---

## 機能
- 音声ファイル（wav 等）をアップロードして文字起こし  
- Whisper モデルの選択（tiny, base, small, medium, large）可能  
- 言語指定やプロンプト、温度パラメータの調整対応  
- 健康チェック用のヘルスエンドポイントあり

---
## 環境構築・使い方（Colab 推奨）

### 1. Colab で実行する場合
(Whisper-音声文字起こしAPI.ipynb)

- Google Colab でこのリポジトリのコードをコピー・ペーストまたはクローンしてください。  
- 「ランタイム」→「ランタイムのタイプを変更」から「ハードウェアアクセラレータ」を **GPU** に切り替えます。  
- ngrok の認証トークンをコード内にセットしてください（`ngrok.set_auth_token("あなたのトークン")` の部分）。  
- これであとはセルを順に実行するだけで、Whisper モデルのロード、Flask サーバーの起動、ngrok トンネルの確立が一括で行われます。  
- 実行後に表示される ngrok の公開 URL を使って API にアクセスできます。

```bash
# 依存ライブラリのインストール例
!pip install flask pyngrok openai-whisper requests
```

---
2. ローカルや他環境で使う場合
- Python 環境で必要なライブラリをインストール
- ngrok トークンを設定
- スクリプトを実行し ngrok トンネルと Flask サーバーを起動
---

### API 呼び出し例（Python）
(Call_api.ipynb)
```
import requests

API_URL = "https://<your-ngrok-url>/v1/audio/transcriptions"
audio_file_path = "path/to/audio.wav"

files = {"file": open(audio_file_path, "rb")}
data = {
    "model": "whisper-1",
    "language": "en",
    "prompt": "",
    "temperature": "0.0",
    "response_format": "text"
}

response = requests.post(API_URL, files=files, data=data)
print(response.text)
```
---

| エンドポイント                    | メソッド | 説明                  |
| -------------------------- | ---- | ------------------- |
| `/v1/audio/transcriptions` | POST | 音声ファイルを受け取り文字起こしを返す |
| `/health`                  | GET  | サーバーのヘルスチェック        |

---

注意点
- ngrok の無料プランはトンネル URL が毎回変わるため、URL の固定化には注意してください。
- Flask の開発サーバーは本番環境向きではありません。
- Whisper モデルのサイズによってメモリ使用量が変わります。

---
