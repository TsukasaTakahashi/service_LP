# 引き継ぎドキュメント

## 最終更新: 2026-01-19

---

## 🚀 次のタスク（作業開始して、と言われたらこれ）

**課金動線の設計・実装**

詳細は「5. 次にやること：課金動線の設計・実装」を参照。

やること：
1. 画面遷移図の作成
2. DB設計（usersテーブル）
3. 機能要件の詳細化
4. 外注向け設計ドキュメント作成

---

## 1. プロジェクト概要

**会社:** Arealinks Inc.（株式会社エリアリンクス）
**HP:** https://www.arealinks.jp/（Google Sitesで運用）
**サービスブランド:** Sumai Agent（スマイエージェント）

---

## 2. 完了したタスク

### オビカエLP
- **URL:** https://obikae.arealinks.jp/
- **GitHub:** git@github.com:TsukasaTakahashi/service_LP.git
- **ホスティング:** Vercel（GitHubにpushすると自動デプロイ）
- **ドメイン:** お名前.comで管理（DNSでVercelに向けている）
- **お問い合わせ:** Google Form https://forms.gle/3j1pUDLTDQXMdPpS7
  - 個人Gmailアカウントで作成（Workspace制限回避のため）
  - 項目: お名前（必須）、会社名、メールアドレス（必須）、電話番号、お問い合わせ内容（必須）

### YouTube動画
- **URL:** https://youtu.be/Jt7k1riVoxo
- **タイトル:** 【不動産仲介向け】図面の帯をAIで自動差し替え | オビカエ

### HP（Google Sites）更新
- **URL:** https://sites.google.com/view/arealinks/home
- SERVICEセクションにオビカエを追加（YouTube埋め込み + 説明文 + LPへのリンク）

### オビカエサービス本体
- **URL:** https://api-front-image-changer-837157351927.us-west1.run.app/bq/index
- 不動産図面の帯をAIで自動差し替えするサービス
- LPの「無料で試す」ボタンからリンク

---

## 3. ファイル構成

```
/Users/tsukasa/Arealinks/HP/
├── HANDOVER.md          # この引き継ぎドキュメント
└── obikae/
    ├── index.html       # LP本体
    ├── style.css        # スタイル
    ├── images/
    │   ├── demo.mp4           # デモ動画
    │   ├── step1-upload.png   # 使い方画像1
    │   ├── step2-detect.png   # 使い方画像2
    │   └── step3-download.png # 使い方画像3
    └── 参考image/       # 元の参考ファイル
```

---

## 4. アカウント情報

| サービス | アカウント |
|---------|-----------|
| GitHub | TsukasaTakahashi |
| Vercel | GitHubで連携 |
| お名前.com | budounomizu@gmail.com |
| YouTube | （要確認） |

---

## 5. 次にやること：課金動線の設計・実装

### 要件

```
・月額サブスク
・メールアドレスのみで登録（ハードル低く）
・登録から30日間無料トライアル
・30日後 → アンケート画面 + 本登録画面に遷移
・目的: 潜在ユーザーへのアプローチ、会話のきっかけ作り
```

### 動線イメージ

```
LP「無料で試す」
    ↓
メールアドレス登録フォーム
    ↓
30日間無料で利用可能
（画面に「あとXX日無料で使えます」を表示）
    ↓
30日後、サービスアクセス時に
    ↓
┌─────────────────────────┐
│ アンケート画面          │
│ ・使ってみてどうでした？ │
│ ・今後使いたい？        │
│ ・ご要望は？            │
└─────────────────────────┘
    ↓
本登録 or お問い合わせ誘導
```

### 技術要件

```
【データベース】
users テーブル
- email: メールアドレス
- registered_at: 登録日時

【ロジック】
残り日数 = 30 - (今日の日付 - registered_at)

if 残り日数 > 0:
    「あと○日無料で使えます」を表示 → サービス画面へ
else:
    「無料期間が終了しました」→ アンケート/本登録画面へ
```

### 開発体制

- CTOポジション: tsukasa（プロジェクトオーナー）
- 開発: 外注（gitで連携可能）
- 次のステップ: 設計ドキュメントを作成 → 外注に共有

---

## 6. 作成すべきドキュメント（次セッション）

1. **画面遷移図** - 登録〜トライアル〜アンケート〜本登録の流れ
2. **DB設計** - usersテーブルの詳細
3. **機能要件** - 各画面でやることの詳細
4. **API設計**（必要に応じて）

---

## 7. 決済について（将来）

- 本登録時の決済手段として **Stripe** を推奨
- 理由: サブスク管理が楽、トライアル期間設定可能、API充実
- ただし、今のフェーズでは課金より「リード獲得」「会話のきっかけ」が優先

---

## 8. 連絡先

- **メール:** tsukasa.takahashi@arealinks.jp

---

## 9. Firebase カスタムドメイン設定（保留中）

認証メールを `noreply@arealinks.jp` から送信するための設定。

### お名前.comで追加するDNSレコード（4つ）

**① TXT（SPF）**
```
ホスト名: （空欄）
TYPE: TXT
VALUE: v=spf1 include:_spf.firebasemail.com ~all
```

**② TXT（Firebase確認）**
```
ホスト名: （空欄）
TYPE: TXT
VALUE: firebase=struccle-media
```

**③ CNAME（DKIM1）**
```
ホスト名: firebase1._domainkey
TYPE: CNAME
VALUE: mail-arealinks-jp.dkim1._domainkey.firebasemail.com
```

**④ CNAME（DKIM2）**
```
ホスト名: firebase2._domainkey
TYPE: CNAME
VALUE: mail-arealinks-jp.dkim2._domainkey.firebasemail.com
```

### 設定後の確認方法

```bash
# TXTレコード確認
dig TXT arealinks.jp +short

# CNAMEレコード確認
dig CNAME firebase1._domainkey.arealinks.jp +short
dig CNAME firebase2._domainkey.arealinks.jp +short
```

### ステータス

- [ ] お名前.comでDNSレコード追加
- [ ] 反映待ち（最大48時間）
- [ ] Firebase側で「確認」ボタンを押して完了

---

## 10. 今回のセッション（2026-01-19）での変更

- お問い合わせをmailto:からGoogle Formに変更
- Google FormはWorkspace制限を避けるため個人Gmailで作成
- サービス本体のURLを新エンドポイントに更新

---

## 11. 次のセッションで最初に伝えること

```
引き継ぎドキュメントは /Users/tsukasa/Arealinks/HP/HANDOVER.md にあります。
```
