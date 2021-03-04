# アプリ名
Music House
# 概要
●ユーザー登録機能
●曲の投稿が可能！
●投稿にいいねができる！
●支援したい人を自分のペースで支援できる！

# 制作背景
新しい曲を聴きたい時に、古い曲やすでに知っている曲が多く、出来立てほやほやのような曲を知りたい！！
そんなサイトがないので、今回このアプリを作成しようと考えました。
# 実装予定の内容
・ユーザー管理機能
・曲投稿機能(youtubeリンクをつけれる)
・曲投稿詳細機能
・投稿編集機能
・投稿削除機能
・投稿一覧表示機能
・サブスクリプション機能
・いいね機能
・ランキング機能
・メッセージ機能
・コメント投稿機能
・SNS認証機能
・フォロー＆フォロワー
・曲検索機能

# DB設計

## usersテーブル
| Column             | Type   | Options                   |
| ------------------ | ------ | ------------------------- |
| nickname           | string | null: false               |
| email              | string | null: false, unique: true |
| encrypted_password | string | null: false               |
| birthday           | date   |                           |
| profile            | text   |                           |
| image              |        |                           |

### Association
- has_many :tweets
- has_many :comments
- has_many :messages
- has_many :relationships
- has_many :room_users
- has_many :relationships,foreign_key: "user_id",dependent::destroy
- has_many :followings,through::relationships,source::follow
- has_many :passive_relationships, class_name: "Relationship", foreign_key: "follow_id", dependent::destroy
- has_many :followers, through::passive_relationships, source::user
- has_one_attached :image
<!-- - has_many :sns_authentication -->

## tweetsテーブル
| Column             | Type    | Options                   |
| ------------------ | ------- | ------------------------- |
| youtube            | string  |                           |
| main_text          | text    | null: false               |
| user_id            |reference| foreign_key: true         |

### Association
- has_many :comments
- belongs_to :user

## commentsテーブル
| Column             | Type    | Options                   |
| ------------------ | ------- | ------------------------- |
| text               | string  |                           |
| user_id            |reference| foreign_key: true         |
| tweet_id           |reference| foreign_key: true         |

### Association
- belongs_to :user
- belongs_to :tweets

## relationshipsテーブル
| Column             | Type    | Options                       |
| ------------------ | ------- | ----------------------------- |
| user_id            |reference| foreign_key: true             |
| follow_id          |reference| foreign_key: {to_table::users}|

### Association
- belongs_to :user
- belongs_to :follow, class_name: "User"

## chat_roomsテーブル

### Association
- has_many :room_users
- has_many :users, through::room_users
- has_many :messages

## room_usersテーブル
| Column             | Type    | Options                       |
| ------------------ | ------- | ----------------------------- |
| user_id            |reference| foreign_key: true             |
| chat_rooms_id      |reference| foreign_key: true             |

### Association
- belongs_to :user
- belongs_to :chat_room

## messagesテーブル
| Column             | Type    | Options                       |
| ------------------ | ------- | ----------------------------- |
| user               |reference| foreign_key: true             |
| chats              |reference| foreign_key: true             |

### Association
- belongs_to :user
- belongs_to :chat_room


<!-- ## sns_authenticationテーブル
| Column             | Type    | Options                       |
| ------------------ | ------- | ----------------------------- |
| user               |reference| foreign_key: true             |
| chats              |reference| foreign_key: true             | -->