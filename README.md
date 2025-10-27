# ☁️ Sky Logger Verifier

このリポジトリは、**Sky Logger App** によって生成されたログとハッシュ値を  
第三者が検証できるように設計された **公開検証アーカイブ** です。

---

## 📁 構成
---

## 🔍 概要

Sky Logger は「いつ・どこで・誰が撮影した映像か」を  
技術的に検証可能にするためのシステムです。

この検証アーカイブでは、撮影時に生成されたログ情報を  
Firebase Firestore と GitHub 上のハッシュアーカイブに二重保存し、  
改ざんがないことを保証します。

---

## ⚙️ ハッシュの仕組み

1. 撮影時に App が `SHA-256` ハッシュを計算します。  
2. そのハッシュを GitHub (`hashes/`) に自動アップロードします。  
3. 同時に Firestore にも同じハッシュを記録します。  
4. どちらか一方が改ざんされても、照合時に不一致として検出されます。

これにより「後から書き換えられていない」ことを第三者が確認できます。

---

## 🔗 関連ページ

| ページ名 | 目的 |
|-----------|------|
| [verify.html](https://kub1221.github.io/skylogger-verifier/verify.html) | QRコードから開く、撮影ログの内容確認ページ |
| [firestore_viewer.html](https://kub1221.github.io/skylogger-verifier/firestore_viewer.html) | FirestoreとGitHubのログ整合性を自動検証 |
| [hashes/](https://github.com/kub1221/skylogger-verifier/tree/main/hashes) | 公開ハッシュ原本フォルダ（各ログ1ファイル） |

---

## 🧾 検証手順（第三者向け）

1️⃣ Firestore または QRコードから取得した JSON データを入手  
2️⃣ JSON の中身を UTF-8（空白なし）でエンコード  
3️⃣ `SHA-256` を計算  
4️⃣ 同じ `videoId.txt` ファイル内のハッシュと比較  
5️⃣ 一致すれば「改ざんなし」と判定  

---

## 🧠 信頼モデル

- **Firestore**：撮影時に端末が直接送信した一次記録  
- **GitHub**：公開コミットログとSHA署名により時刻証跡を保証  

この2層が一致している限り、  
記録は「撮影当時の真実を保持している」と判断できます。

---

© 2025 Sky Logger Project – Maintained by kub1221  
📍 https://kub1221.github.io/skylogger-verifier/
