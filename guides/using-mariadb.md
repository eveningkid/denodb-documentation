---
id: using-mariadb
title: Using MariaDB
sidebar_label: Using MariaDB
---

DenoDB is using [deno_mysql](https://deno.land/x/mysql) for MariaDB support. This means the connector we'll use is the same as with MySQL.

```typescript
import { Database, MySQLConnector } from 'https://deno.land/x/denodb/mod.ts';

const connector = new MySQLConnector({
  database: 'my-database',
  host: 'url-to-db.com',
  username: 'username',
  password: 'password',
  port: 3306, // optional
});

const db = new Database(connector);
```
