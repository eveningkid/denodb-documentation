---
id: connect-database
title: Connect to a database
sidebar_label: Connect to a database
---

Before going any further, you will need to connect to your database.

```javascript
import { Database, YourDatabaseConnector } from 'https://deno.land/x/denodb/mod.ts';

// SQLite3Connector, MySQLConnector, PostgresConnector...
const connector = new YourDatabaseConnector({ ... });
const db = new Database(connector);

// If you need debug logs, set `debug` to `true`
const db = new Database(
  connector,
  debug: true,
);
```

Depending on the dialect, you might need different options:

- [Using MariaDB](guides/using-mariadb.md)
- [Using MongoDB](guides/using-mongodb.md)
- [Using MySQL](guides/using-mysql.md)
- [Using PostegreSQL](guides/using-postgresql.md)
- [Using SQLite](guides/using-sqlite.md)
