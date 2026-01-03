# QB-JobApplication - QBCore ジョブ応募システム

Minecraftのstaff application forumのような、FiveM QBCore用のジョブ応募システムです。

## 📋 機能

### プレイヤー機能
- `/jobap` - ジョブに応募
- `/myjobaps` - 自分の申請一覧を確認
- 複数のジョブに同時応募可能（デフォルト3つまで）
- 申請のステータス確認（審査中、面接予定、承認、却下）
- 審査中の申請を取り消し可能
- 返信やコメントを確認

### 管理者機能
- `/jobapmanage` - 申請を管理
- 申請の承認/却下/面接設定
- 申請者へ返信メッセージ送信
- 申請にコメント追加（内部メモ）
- リアルタイム通知（新規申請時）
- ステータスフィルター機能

## 📦 インストール

### 1. リソースのダウンロード
```bash
# サーバーのresourcesフォルダに配置
[qb]/qb-jobapplication/
```

### 2. データベース設定
`job_applications.sql` を実行してテーブルを作成：
```sql
-- HeidiSQL、phpMyAdmin、またはMySQLクライアントで実行
SOURCE job_applications.sql;
```

### 3. 依存関係の確認
必要なリソース：
- **qb-core** - QBCore Framework
- **oxmysql** - MySQL Database Wrapper

### 4. server.cfg に追加
```cfg
ensure qb-core
ensure oxmysql
ensure qb-jobapplication
```

### 5. 設定のカスタマイズ
`config/config.lua` で以下をカスタマイズ可能：
- 応募可能なジョブの設定
- 各ジョブの管理者最低階級
- 申請文の最大文字数
- 再申請までのクールダウン時間
- 同時応募可能な最大数

## ⚙️ 設定例

```lua
Config.Jobs = {
    ['police'] = {
        label = '警察署',
        minGrade = 2, -- この階級以上が申請を確認できる
        requirementText = '要件: 18歳以上、無犯罪歴'
    },
    ['ambulance'] = {
        label = '救急医療',
        minGrade = 2,
        requirementText = '要件: 医療知識、18歳以上'
    }
}

Config.MaxApplicationLength = 1000 -- 申請文の最大文字数
Config.ApplicationCooldown = 24 -- 再申請までの待機時間（時間）
Config.MaxActiveApplications = 3 -- 同時に応募できる最大数
```

## 🎮 使い方

### プレイヤー向け

1. **ジョブに応募する**
   ```
   /jobap
   ```
   - ジョブを選択
   - 志望動機や自己PRを記入
   - 申請を送信

2. **自分の申請を確認**
   ```
   /myjobaps
   ```
   - 過去の申請一覧を確認
   - ステータスを確認
   - 審査中の申請は取り消し可能

### 管理者向け

1. **申請を管理する**
   ```
   /jobapmanage
   ```
   - 新しい申請を確認
   - ステータスでフィルター
   - 申請の詳細を確認

2. **申請を処理する**
   - **承認** - 申請を承認し、返信メッセージを送信
   - **面接** - 面接予定としてマーク
   - **却下** - 申請を却下し、理由を通知
   - **コメント** - 内部メモとしてコメント追加

## 📊 データベース構造

### job_applications テーブル
| フィールド | 型 | 説明 |
|-----------|-----|------|
| id | INT | 申請ID（自動採番） |
| citizenid | VARCHAR(50) | 市民ID |
| name | VARCHAR(100) | プレイヤー名 |
| job | VARCHAR(50) | 応募先ジョブ |
| message | TEXT | 申請内容 |
| status | ENUM | ステータス（pending/approved/rejected/interview） |
| reviewed_by | VARCHAR(100) | 審査者名 |
| reviewed_at | TIMESTAMP | 審査日時 |
| response | TEXT | 返信メッセージ |
| created_at | TIMESTAMP | 作成日時 |

### job_application_comments テーブル
| フィールド | 型 | 説明 |
|-----------|-----|------|
| id | INT | コメントID（自動採番） |
| application_id | INT | 申請ID（外部キー） |
| commenter_name | VARCHAR(100) | コメント者名 |
| comment | TEXT | コメント内容 |
| created_at | TIMESTAMP | 作成日時 |

## 🎨 UI機能

- **モダンなデザイン** - グラデーション背景と見やすいレイアウト
- **レスポンシブ** - 様々な画面サイズに対応
- **リアルタイム更新** - 申請リストの自動更新機能
- **ステータスバッジ** - 色分けされたステータス表示
- **コメントシステム** - 管理者が内部メモを残せる
- **文字数カウント** - リアルタイムで入力文字数を表示

## 🔧 トラブルシューティング

### データベース接続エラー
```lua
-- server.cfg でoxmysqlが正しく設定されているか確認
set mysql_connection_string "mysql://user:password@localhost/database"
```

### 権限エラー
```lua
-- config.lua でminGradeを確認
-- プレイヤーのジョブグレードが十分か確認
```

### UIが表示されない
```lua
-- F8コンソールでエラーを確認
-- NUIのキャッシュをクリア（Ctrl+F5）
```

## 📝 今後の追加予定機能

- [ ] Discord Webhook通知
- [ ] 申請テンプレート機能
- [ ] 面接日時の予約機能
- [ ] 申請の自動アーカイブ
- [ ] 統計ダッシュボード
- [ ] メール通知システム

## 🤝 サポート

問題が発生した場合：
1. F8コンソールでエラーを確認
2. サーバーコンソールでエラーを確認
3. データベースのテーブルが正しく作成されているか確認
4. 依存関係（qb-core、oxmysql）が正しくインストールされているか確認

## 📄 ライセンス

このリソースはオープンソースです。自由に使用・改変してください。

## 🙏 クレジット

- QBCore Framework
- FiveM Community

---

**バージョン**: 1.0.0  
**作成日**: 2025年  
**対応**: QBCore Framework
