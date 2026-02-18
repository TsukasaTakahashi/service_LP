# 作業指示書 — HP・サービスLP管理

## 最終更新: 2026-02-11

---

## このディレクトリの目的

Arealinks Inc. の**HP（コーポレートサイト）**および**各サービスのLP（ランディングページ）**を作成・管理する作業場所。

サービスが増えるたびに、ここにLP用のディレクトリを追加していく。

---

## 1. プロジェクト概要

**会社:** Arealinks Inc.（株式会社エリアリンクス）
**HP:** https://www.arealinks.jp/（現在はGoogle Sitesで運用。今後自前化するかは未定）
**サービスブランド:** Sumai Agent（スマイエージェント）
**連絡先:** tsukasa.takahashi@arealinks.jp

---

## 2. アカウント情報

| サービス | アカウント |
|---------|-----------|
| GitHub | TsukasaTakahashi |
| Vercel | GitHubで連携 |
| お名前.com | budounomizu@gmail.com |
| YouTube | （要確認） |

---

## 3. ディレクトリ構成

```
/Users/tsukasa/Arealinks/HP/
├── HANDOVER.md          # この作業指示書
└── obikae/              # オビカエLP（完成済み）
    ├── index.html
    ├── style.css
    ├── images/
    └── 参考image/
```

新しいサービスLPを作る場合は、サービス名のディレクトリを追加する。

---

## 4. 共通ルール

### デプロイ
- **ホスティング:** Vercel（GitHubにpushで自動デプロイ）
- **ドメイン:** お名前.comで管理（DNSでVercelに向ける）
- **GitHub:** git@github.com:TsukasaTakahashi/service_LP.git

### LP作成時の共通方針
- レスポンシブ対応（PC / スマホ）
- お問い合わせはGoogle Formを使用（Workspace制限回避のため個人Gmailで作成）
- CTA（無料で試す等）はファーストビューと下部の2箇所に配置

---

## 5. 完成済みサービスLP

### オビカエ
- **LP:** https://obikae.arealinks.jp/
- **サービス本体:** https://sumai-agent-rb.arealinks.jp/business/
- **内容:** 不動産図面の帯をAIで自動差し替えするサービス
- **YouTube:** https://youtu.be/Jt7k1riVoxo
- **お問い合わせ:** https://forms.gle/3j1pUDLTDQXMdPpS7
- **ステータス:** 公開済み、課金動線は別途実装済み

---

## 6. HP（コーポレートサイト）

- **現在:** Google Sites（https://sites.google.com/view/arealinks/home）
- **今後:** 自前で作り直すかは未定。決まり次第このディレクトリで作業する。

---

## 7. 新しいLPを作るときの手順

1. このディレクトリにサービス名のフォルダを作成（例: `新サービス名/`）
2. `index.html` + `style.css` を作成
3. 画像・動画は `images/` に配置
4. GitHubにpush → Vercelで自動デプロイ
5. お名前.comでサブドメインのDNS設定
6. このファイルの「完成済みサービスLP」に情報を追記

---

## 8. 参考：Firebase カスタムドメイン設定（保留中）

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

### ステータス

- [ ] お名前.comでDNSレコード追加
- [ ] 反映待ち（最大48時間）
- [ ] Firebase側で「確認」ボタンを押して完了
