# schema.prisma

公式ドキュメント

<https://www.prisma.io/docs/getting-started/setup-prisma/start-from-scratch/relational-databases-typescript-postgresql>

## 作成

```cmd
npx prisma init
```

## datasource

<https://www.prisma.io/docs/orm/reference/prisma-schema-reference>

## テーブル例

<https://www.prisma.io/docs/orm/reference/prisma-client-reference>

```
model User {
  id           Int              @id @default(autoincrement())
  name         String?
  email        String           @unique
  profileViews Int              @default(0)
  role         Role             @default(USER)
  coinflips    Boolean[]
  posts        Post[]
  city         String
  country      String
  profile      ExtendedProfile?
  pets         Json
}

model ExtendedProfile {
  id     Int     @id @default(autoincrement())
  userId Int?    @unique
  bio    String?
  User   User?   @relation(fields: [userId], references: [id])
}

model Post {
  id        Int     @id @default(autoincrement())
  title     String
  published Boolean @default(true)
  author    User    @relation(fields: [authorId], references: [id])
  authorId  Int
  comments  Json
  views     Int     @default(0)
  likes     Int     @default(0)
}

enum Role {
  USER
  ADMIN
}
```