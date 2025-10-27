# meets TEC

音声入力により画面の色と幾何学模様が変わるiPhoneアプリです。マイクから入力された音階を判断し、オクターブが違っても同じ音階なら同じ色を表示します。音階の変化に応じて、画面上の幾何学模様がダイナミックに変化します。

## 機能

- **リアルタイム音階検出**: マイクから入力された音声をリアルタイムで分析し、音階を判定します
- **12音階カラーマッピング**: 各音階（C, C#, D, D#, E, F, F#, G, G#, A, A#, B）に固有の色を割り当て
- **3つのカラーテーマ**: 
  - **虹色**: 12色の鮮やかな虹色グラデーション
  - **情熱的**: 赤から黄色への暖色系グラデーション
  - **静か**: 青色系の落ち着いたグラデーション
- **幾何学模様の可視化**: 音階に応じて変化する動的な幾何学パターン
  - 無音時: 直線的なパターン
  - 音声入力時: 周波数と音階に応じてギザギザに変化
  - 複数のレイヤーによる波形パターン
- **スムーズな色遷移**: 音階が変わる際に、滑らかなアニメーションで色が変化します
- **オクターブ非依存**: どのオクターブでも同じ音階なら同じ色を表示します
- **テーマ切り替え**: 画面右上のパレットアイコンから簡単にテーマを切り替え可能

## カラーテーマ

### 虹色テーマ

| 音階 | 色 (RGB) |
|------|----------|
| C (ド) | 赤 (255, 0, 0) |
| C# (ド#) | オレンジ (255, 128, 0) |
| D (レ) | 黄色 (255, 255, 0) |
| D# (レ#) | 黄緑 (128, 255, 0) |
| E (ミ) | 緑 (0, 255, 0) |
| F (ファ) | 青緑 (0, 255, 128) |
| F# (ファ#) | シアン (0, 255, 255) |
| G (ソ) | 水色 (0, 128, 255) |
| G# (ソ#) | 青 (0, 0, 255) |
| A (ラ) | 紫 (128, 0, 255) |
| A# (ラ#) | マゼンタ (255, 0, 255) |
| B (シ) | ピンク (255, 0, 128) |

### 情熱的テーマ

赤から黄色への暖色系グラデーション（12段階）

### 静かテーマ

濃い青から明るい青への寒色系グラデーション（12段階）

## 技術仕様

- **開発言語**: Swift 5.0
- **フレームワーク**: SwiftUI
- **最小対応OS**: iOS 16.0以上
- **音声処理**: AVFoundation, Accelerate (FFT)
- **ピッチ検出**: FFT (Fast Fourier Transform) を使用した周波数解析

## プロジェクト構成

```
meetstec/
├── MusicColorApp.xcodeproj/
│   └── project.pbxproj
├── MusicColorApp/
│   ├── MusicColorAppApp.swift      # アプリのエントリーポイント
│   ├── ContentView.swift            # メインUI（テーマセレクター含む）
│   ├── AudioManager.swift           # 音声入力管理
│   ├── PitchDetector.swift          # ピッチ検出ロジック
│   ├── ColorMapper.swift            # 音階→色マッピング（3テーマ対応）
│   ├── GeometricPatternView.swift   # 幾何学模様の可視化
│   ├── Info.plist                   # アプリ設定
│   └── Assets.xcassets/             # アセット
└── README.md
```

## セットアップ方法

### 必要な環境

- macOS 13.0以上
- Xcode 15.0以上
- iPhone実機（マイク機能を使用するため、シミュレーターでは完全には動作しません）

### ビルド手順

1. リポジトリをクローン:
```bash
git clone https://github.com/tsubasa-shinohara/meetstec.git
cd meetstec
```

2. Xcodeでプロジェクトを開く:
```bash
open MusicColorApp.xcodeproj
```

3. Xcodeで以下の設定を行う:
   - プロジェクトナビゲーターで `MusicColorApp` を選択
   - `Signing & Capabilities` タブを開く
   - `Team` を自分のApple Developer アカウントに設定
   - 必要に応じて `Bundle Identifier` を変更

4. iPhone実機を接続し、ターゲットデバイスとして選択

5. `Product` > `Run` (または ⌘R) でビルド＆実行

## 使い方

1. アプリを起動すると、マイクへのアクセス許可を求められます。「許可」を選択してください
2. 画面右上のパレットアイコンをタップして、お好みのカラーテーマ（虹色/情熱的/静か）を選択
3. 画面中央の「Start」ボタンをタップして音声入力を開始
4. 楽器や声で音を出すと、画面の色と幾何学模様が変化します
5. 画面には現在検出されている音階と周波数が表示されます
6. 無音時は直線的なパターンが表示され、音を出すとギザギザに変化します
7. 「Stop」ボタンをタップして音声入力を停止

## 技術的な詳細

### ピッチ検出アルゴリズム

1. **音声入力**: AVAudioEngineを使用してマイクから音声データを取得
2. **FFT処理**: Accelerateフレームワークを使用して高速フーリエ変換を実行
3. **周波数解析**: FFT結果から最も強い周波数成分を抽出
4. **音階変換**: 検出された周波数を音階に変換（A4 = 440Hzを基準）
5. **信頼度フィルタリング**: 信頼度が30%以上の結果のみを採用

### カラーアニメーション

- SwiftUIの `.animation()` モディファイアを使用
- 0.3秒のイージングアニメーションで滑らかに色が遷移
- 音階が変わるたびに新しい色へアニメーション
- 3つのカラーテーマ（虹色、情熱的、静か）をリアルタイムで切り替え可能

### 幾何学模様の可視化

- **GeometricPatternView**: 周波数に応じて変化する波形パターン
  - 無音時: 振幅が小さく、ほぼ直線
  - 音声入力時: 周波数と振幅に応じてギザギザに変化
  - サイン波を使用した滑らかな曲線
- **WavePatternView**: 複数レイヤーの波形エフェクト
  - 5層の半透明な波形を重ね合わせ
  - 周波数に応じて波長が変化
  - 連続的なアニメーションで動的な視覚効果

## トラブルシューティング

### マイクが動作しない
- 設定アプリ > プライバシーとセキュリティ > マイク で、アプリの権限を確認
- アプリを再起動してみてください

### 音階が正しく検出されない
- 静かな環境で試してください
- マイクに近づいて、はっきりとした音を出してください
- 楽器の場合は、単音（和音ではなく）を演奏してください

### ビルドエラーが発生する
- Xcodeのバージョンが15.0以上であることを確認
- `Product` > `Clean Build Folder` を実行後、再ビルド
- Signing & Capabilitiesの設定を確認

## GitHubへのプッシュ方法

このコードをGitHubリポジトリにプッシュする方法を2つ提供します。

### 方法1: ZIPファイルを使用（シンプル）

1. 空のリポジトリをクローン:
```bash
git clone https://github.com/tsubasa-shinohara/meetstec.git
cd meetstec
```

2. ZIPファイル（`meetstec.zip`）を解凍し、中身をすべてこのフォルダにコピー

3. ブランチを作成してコミット:
```bash
git checkout -b devin/1761557967-music-color-app
git add -A
git commit -m "Add meets TEC app with color themes and geometric patterns"
```

4. ベースブランチ（main）を作成:
```bash
git checkout --orphan main
git commit --allow-empty -m "Initial empty commit"
git push -u origin main
```

5. フィーチャーブランチをプッシュ:
```bash
git checkout devin/1761557967-music-color-app
git push -u origin devin/1761557967-music-color-app
```

6. GitHubでPRを作成:
   - Head branch: `devin/1761557967-music-color-app`
   - Base branch: `main`

### 方法2: Git Bundleを使用（履歴を保持）

1. Bundleファイル（`meets-tec.bundle`）からクローン:
```bash
git clone meets-tec.bundle -b devin/1761557967-music-color-app meetstec
cd meetstec
```

2. リモートリポジトリを追加:
```bash
git remote add origin https://github.com/tsubasa-shinohara/meetstec.git
```

3. ベースブランチ（main）を作成:
```bash
first_commit=$(git rev-list --max-parents=0 HEAD)
git branch -f main "$first_commit"
```

4. ブランチをプッシュ:
```bash
git push -u origin main
git push -u origin devin/1761557967-music-color-app
```

5. GitHubでPRを作成:
   - Head branch: `devin/1761557967-music-color-app`
   - Base branch: `main`

## ライセンス

このプロジェクトはMITライセンスの下で公開されています。

## 作成者

Created by Devin for tsubasa-shinohara
