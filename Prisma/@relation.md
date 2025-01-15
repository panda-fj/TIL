# @relation

リレーションを定義することができる。

公式ドキュメント <https://www.prisma.io/docs/orm/prisma-schema/data-model/relations>

## 基本（1対多）

```prisma
model User {
  id    Int    @id @default(autoincrement())
  name  String
  posts Post[] // UserとPostは1対多の関係-
}

model Post {
  id       Int    @id @default(autoincrement())
  title    String
  content  String
  userId   Int
  user     User   @relation(fields: [userId], references: [id]) // @relation属性でリレーションを定義
}
```

1. `fields` モデル内のどのフィールドと関連付けるかを定義する。
   * 上の例では、`Post`内の`userId`を指定して、後の`references`で指定する別モデルのフィールド(`User`モデルの`id`)と関連付けている。

2. `references` リレーション先のモデルで参照するフィールドを指定する。
   * 上記例では、`Post.userId`が`User.id`を参照している。


## 1対1

```prisma
model Profile {
  id     Int    @id @default(autoincrement())
  bio    String
  userId Int    @unique
  user   User   @relation(fields: [userId], references: [id])
}

model User {
  id      Int      @id @default(autoincrement())
  name    String
  profile Profile? // ここが違う
}
```

userIdはユニーク制約（@unique）が必要。

## 多対多

```
model Post {
  id      Int      @id @default(autoincrement())
  title   String
  content String
  tags    Tag[]    @relation("PostTags")
}

model Tag {
  id    Int    @id @default(autoincrement())
  name  String
  posts Post[] @relation("PostTags")
}
```

多対多リレーションには中間テーブルが自動的に生成される（PostTags）。