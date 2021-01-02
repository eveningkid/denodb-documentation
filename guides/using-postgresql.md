---
id: using-postgresql
title: Using PostgreSQL
sidebar_label: Using PostgreSQL
---

DenoDB is using [deno-postgres](https://deno.land/x/postgres) for PostgreSQL support.

The configuration is pretty straight-forward:

```typescript
import { Database, PostgresConnector } from 'https://deno.land/x/denodb/mod.ts';

const connector = new PostgresConnector({
  database: 'my-database',
  host: 'url-to-db.com',
  username: 'username',
  password: 'password',
  port: 5432, // optional
});

const db = new Database(connector);
```
