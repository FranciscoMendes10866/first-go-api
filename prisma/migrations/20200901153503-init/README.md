# Migration `20200901153503-init`

This migration has been generated at 9/1/2020, 3:35:03 PM.
You can check out the [state of the schema](./schema.prisma) after the migration.

## Database Steps

```sql
PRAGMA foreign_keys=OFF;

CREATE TABLE "quaint"."User" (
"age" INTEGER   ,"createdAt" DATE NOT NULL DEFAULT CURRENT_TIMESTAMP ,"email" TEXT NOT NULL  ,"id" TEXT NOT NULL  ,"name" TEXT   ,
    PRIMARY KEY ("id"))

CREATE TABLE "quaint"."Post" (
"authorID" TEXT NOT NULL  ,"content" TEXT   ,"createdAt" DATE NOT NULL DEFAULT CURRENT_TIMESTAMP ,"id" TEXT NOT NULL  ,"published" BOOLEAN NOT NULL  ,"title" TEXT NOT NULL  ,"updatedAt" DATE NOT NULL  ,
    PRIMARY KEY ("id"),FOREIGN KEY ("authorID") REFERENCES "User"("id") ON DELETE CASCADE ON UPDATE CASCADE)

CREATE UNIQUE INDEX "quaint"."User.email" ON "User"("email")

PRAGMA "quaint".foreign_key_check;

PRAGMA foreign_keys=ON;
```

## Changes

```diff
diff --git schema.prisma schema.prisma
migration ..20200901153503-init
--- datamodel.dml
+++ datamodel.dml
@@ -1,0 +1,30 @@
+datasource db {
+    provider = "sqlite"
+    url      = "file:dev.db"
+}
+
+generator db {
+    provider = "go run github.com/prisma/prisma-client-go"
+}
+
+model User {
+    id        String   @default(cuid()) @id
+    createdAt DateTime @default(now())
+    email     String   @unique
+    name      String?
+    age       Int?
+
+    posts     Post[]
+}
+
+model Post {
+    id        String   @default(cuid()) @id
+    createdAt DateTime @default(now())
+    updatedAt DateTime @updatedAt
+    published Boolean
+    title     String
+    content   String?
+
+    author   User @relation(fields: [authorID], references: [id])
+    authorID String
+}
```


