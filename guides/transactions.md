---
id: transactions
title: Transactions
sidebar_label: Transactions
---

Transactions are helpful to run a list of inter-dependent queries that could potentially break. If one doesn't work, it shouldn't impact the others and roll back the previous queries.

Any query that goes inside a transaction block, will only be run if all of them have been successfully executed.

> Transactions are supported on MySQL, MariaDB, SQLite and Postgres (not MongoDB).

```typescript
const db = new Database(connection);

// Declare your models and link them to your database

await db.transaction(async () => {
  const user = await User.create({ name: 'Joelle' });
  await Payments.create({ userId: user.id, amount: 20 });
});
```

Just to make sure it's all clear for you: if `User.create` or `Payments.create` fails, this whole block will be dismissed and it will be as if nothing had happened.
