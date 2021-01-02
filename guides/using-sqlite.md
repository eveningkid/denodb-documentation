---
id: using-sqlite
title: Using SQLite
sidebar_label: Using SQLite
---

DenoDB is using [Deno SQLite](https://deno.land/x/sqlite) for SQLite support.

The configuration is pretty straight-forward:

```typescript
import { Database, SQLite3Connector } from 'https://deno.land/x/denodb/mod.ts';

const connector = new SQLite3Connector({
  filepath: './database.sqlite',
});

const db = new Database(connector);
```
