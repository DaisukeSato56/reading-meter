# README

This README would normally document whatever steps are necessary to get the
application up and running.

Things you may want to cover:

* Ruby version

* System dependencies

* Configuration

* Database creation

* Database initialization

* How to run the test suite

* Services (job queues, cache servers, search engines, etc.)

* Deployment instructions

* ...


# DB設計

## users table
|Column|Type|Options|
|------|----|-------|
|name|string|null: false, unique: true|
|email|string|null: false, unique: true|

### Association
- has_many :active_relationships, class_name: "Relation", foreign_key: "follower_id", dependent: :destroy
- has_many :passive_relationships, class_name: "Relation", foreign_key: "followed_id", dependent: :destroy
- has_many :following, through: :active_relationships, source: :followed
- has_many :followers, through: :passive_relationships, source: :follower
- has_many :books, through: :situations
- has_many :situations
- has_many :tweets
- has_many :tweets, through: :likes
- has_many :likes

## books table
|Column|Type|Options|
|------|----|-------|
|name|string|null: false, unique: true|
|page|integer||
|author|string||
|amazon_url|string||
|genre|string||

### Association
- has_many :users, through: :situations
- has_many :situations
- has_many :tweets

## tweet table
|Column|Type|Options|
|------|----|-------|
|text|string|null: false|
|user_id|integer|null: false, foreign_key: true|
|book_id|integer|null: false, foreign_key: true|

### Association
- belongs_to :user
- belongs_to :book
- has_many :users, through: :likes
- has_many :likes


## relations table
|Column|Type|Options|
|------|----|-------|
|follower_id|integer|null: false, foreign_key: true, index: true|
|followed_id|integer|null: false, foreign_key: true, index: true|

### Association
- belongs_to :follower, class_name: "User"
- belongs_to :followed, class_name: "User"


## status table
|Column|Type|Options|
|------|----|-------|
|name|string|null: false, unique: true|

### Association
- has_many :situations

## situation table
|Column|Type|Options|
|------|----|-------|
|status_id|integer|null: false, foreign_key|
|user_id|integer|null: false, foreign_key: true|
|book_id|integer|null: false, foreign_key: true|

### Association
- belongs_to :user
- belongs_to :book
- belongs_to :status

## like table
|Column|Type|Options|
|------|----|-------|
|user_id|integer|null: false, foreign_key: true|
|tweet_id|integer|null: false, foreign_key: true|

### Association
- belongs_to :user
- belongs_to :tweet
