# Audio Waveform Visualizer

音声ファイル（mp3 / wav / m4a / ogg）をアップロードすると、波形に合わせて動くビジュアライザーをプレビュー・動画（WebM）として書き出せる、完全クライアントサイドの Web アプリです。

👉 デモ: GitHub Pages で公開後、`https://<ユーザー名>.github.io/<リポジトリ名>/` でアクセスできます。

## 機能

- 音声ファイルのドラッグ＆ドロップ読み込み
- 4種類のビジュアライザースタイル
  - なめらか波形ライン / ミラー波形 / スペクトラムバー / サークル
- カラー・背景色・感度・解像度（720p / 1080p / 正方形）のカスタマイズ
- リアルタイムプレビュー再生
- **動画書き出し**（Canvas + 音声を MediaRecorder で録画 → MP4 (H.264 + AAC) ダウンロード。非対応ブラウザでは WebM にフォールバック）

## 仕組み

サーバーは一切不要。すべてブラウザ内で完結します。

1. Web Audio API の `decodeAudioData` で音声をデコード
2. `AnalyserNode` で波形（時間領域）・スペクトラム（周波数領域）を毎フレーム取得
3. Canvas 2D にアニメーション描画
4. `canvas.captureStream()` + `MediaStreamAudioDestinationNode` を `MediaRecorder` で録画

## GitHub Pages での公開手順

1. このリポジトリを GitHub に push
2. リポジトリの **Settings → Pages** を開く
3. Source: **Deploy from a branch** / Branch: `main` / フォルダ: `/ (root)` を選択して Save
4. 数分後に `https://<ユーザー名>.github.io/<リポジトリ名>/` で公開

> **注意**: プライベートリポジトリで GitHub Pages を使うには、有料プラン（GitHub Pro / Team / Enterprise）が必要です。無料プランの場合はリポジトリをパブリックにする必要があります。
> なお、Pages で公開したページ自体は誰でもアクセス可能になります（Enterprise の Access control を使う場合を除く）。

## 制限事項

- 動画書き出しはリアルタイム録画のため、曲の長さと同じ時間がかかります
- 書き出し中はタブを前面に表示したままにしてください（バックグラウンドでは描画が止まります）
- 出力は MP4 形式（H.264 + AAC）。ブラウザが MP4 録画に非対応の場合は WebM になります
