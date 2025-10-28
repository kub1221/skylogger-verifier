# ☁️ Sky Logger 検証ツール（Verification System）

このリポジトリは **Sky Logger App** によって生成された  
ログとハッシュ値を第三者が検証できるように設計された  
**公開検証アーカイブ** です。

---

## 📸 Sky Logger の目的

Sky Logger は  
> 「いつ・どこで・誰が撮影した映像か」  
> 「その映像がデジタル的にも物理的にも改ざんされていないか」  

を検証可能にするシステムです。  

撮影時に生成されたログ情報を  
- 🔹 **Firebase Firestore（原本）**  
- 🔹 **GitHub（公開アーカイブ）**  

の二重に保存し、改ざんがないことを第三者が独立して確認できます。

---

## 🔍 物理的証明レイヤー（Physical Verification Layer）

Sky Logger の最大の特徴は、  
**アプリが生成したQRコードを撮影用カメラが実際に撮影する** ことです。

この行為によって：

1️⃣ QRコードに含まれる UUID・位置情報・時刻・ハッシュ が「アプリ宣言」として生成される。  
2️⃣ そのQRコードが映像フレーム内に物理的に記録される。  
3️⃣ 同時に Firestore と GitHub に同じハッシュが記録される。  

結果として、  
> 🔗 「ログ（クラウド）」と「映像（物理）」が一対一で結びつき、  
> 撮影行為そのものがデジタル署名として機能します。

これにより、  
**「QRが映り込んでいる＝当時アプリが発行した真正な撮影ログ」**  
として、撮影の真実性が視覚的にも暗号的にも証明されます。

---

## ⚙️ ハッシュと二重記録の仕組み

1️⃣ 撮影時にアプリが UUID・時刻・位置情報 をまとめ、 **SHA-256 ハッシュ** を生成  
2️⃣ そのハッシュを **GitHub（hashes/）** に自動保存  
3️⃣ 同時に **Firestore** にも同じログを保存  
4️⃣ どちらかが改ざんされても、照合時に「不一致」として検出  

この二重記録により、  
**撮影時点で生成されたログが後から改変されていない** ことを保証します。

---

## 🔗 構成

| ページ / フォルダ | 役割 |
|------------------|------|
| [verify.html](verify.html) | QRコードから開く、撮影ログの内容確認ページ |
| [firestore_viewer.html](firestore_viewer.html) | FirestoreとGitHubのログ整合性を自動検証（全体・個別モード対応） |
| [hashes/（GitHub上で開く）](https://github.com/kub1221/skylogger-verifier/tree/main/hashes) | 公開ハッシュ原本フォルダ（各ログ1ファイル） |
| [README.md](README.md) | システムの仕組み・検証手順・信頼モデルの説明 |

---

## 🧾 検証手順（第三者向け）

1️⃣ **撮影映像に映る QR コード** をスマートフォンなどでスキャン  
　→ `verify.html` が開く  

2️⃣ 表示された **UUID・位置情報・撮影時刻・ハッシュ** を確認  

3️⃣ ページ下のリンク  
　「🔗 Firestore と GitHub の照合結果を見る」 をタップ  
　→ `firestore_viewer.html` が開く  

4️⃣ Firestore と GitHub 上の同一 `videoId` のハッシュが一致していることを確認  

5️⃣ 必要に応じて  
　[hashes/](https://github.com/kub1221/skylogger-verifier/tree/main/hashes) 内の  
　該当ログファイルを開き、原本ハッシュを直接参照  

6️⃣ 一致すれば「改ざんなし」と判定可能  

> ※専門家は必要に応じて、ダウンロードしたJSONをもとにローカルでSHA-256を再計算して照合可能です。

---

## 🧠 信頼モデル（3層構造）

| 層 | 役割 | 内容 |
|----|------|------|
| ① 物理層 | カメラでQRを撮影 | 「現実にその瞬間が存在した」光学的証拠 |
| ② デジタル層 | Firestore と GitHub | 「改ざんされていない」二重記録による証拠 |
| ③ 検証層 | verify.html / firestore_viewer.html | 誰でも検証できる公開透明性 |

これら3層が連携することで、  
**映像そのものが「特定の時間・場所で実際に撮影された現実の記録」である」**  
ことを技術的に保証します。

---

## 🔒 信頼の根拠（3要素）

1️⃣ **アプリの正体（App Signature）**  
　UUID・緯度経度・時刻・ハッシュ → Firestore & GitHub による暗号的証明  

2️⃣ **カメラの証跡（Optical Signature）**  
　QRコードが映り込む映像 → 物理的・光学的な現実証拠  

3️⃣ **公開検証（Public Verification）**  
　誰でも照合可能 → 改ざん不可能な第三者検証モデル  

---

📍 **公式サイト（検証入口）**  
🔗 [https://kub1221.github.io/skylogger-verifier/](https://kub1221.github.io/skylogger-verifier/)

© 2025 Sky Logger Project — Maintained by kub1221
