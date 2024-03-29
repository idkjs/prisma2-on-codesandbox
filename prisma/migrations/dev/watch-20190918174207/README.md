# Migration `watch-20190918174207`

This migration has been generated at 9/18/2019, 5:42:08 PM.
You can check out the [state of the datamodel](./datamodel.prisma) after the migration.

## Database Steps

```sql
CREATE TABLE "lift"."User" (
  "email" TEXT NOT NULL DEFAULT ''  ,
  "id" TEXT NOT NULL   ,
  "name" TEXT    ,
  PRIMARY KEY ("id")
);

CREATE TABLE "lift"."Post" (
  "author" TEXT    REFERENCES "User"(id) ON DELETE SET NULL,
  "content" TEXT    ,
  "createdAt" DATE NOT NULL DEFAULT '1970-01-01 00:00:00'  ,
  "id" TEXT NOT NULL   ,
  "published" BOOLEAN NOT NULL DEFAULT false  ,
  "title" TEXT NOT NULL DEFAULT ''  ,
  "updatedAt" DATE NOT NULL DEFAULT '1970-01-01 00:00:00'  ,
  PRIMARY KEY ("id")
);

CREATE UNIQUE INDEX "lift"."User.email" ON "User"("email")
```

## Changes

```diff
diff --git datamodel.mdl datamodel.mdl
migration ..watch-20190918174207
--- datamodel.dml
+++ datamodel.dml
@@ -1,0 +1,30 @@
+datasource db {
+  provider = "sqlite"
+  url      = "file:dev.db"
+  default  = true
+}
+
+generator photon {
+  provider = "photonjs"
+}
+
+generator nexus_prisma {
+  provider = "nexus-prisma"
+}
+
+model User {
+  id    String  @default(cuid()) @id
+  email String  @unique
+  name  String?
+  posts Post[]
+}
+
+model Post {
+  id        String   @default(cuid()) @id
+  createdAt DateTime @default(now())
+  updatedAt DateTime @updatedAt
+  published Boolean
+  title     String
+  content   String?
+  author    User?
+}
```

## Photon Usage

You can use a specific Photon built for this migration (watch-20190918174207)
in your `before` or `after` migration script like this:

```ts
import Photon from '@generated/photon/watch-20190918174207'

const photon = new Photon()

async function main() {
  const result = await photon.users()
  console.dir(result, { depth: null })
}

main()

```
