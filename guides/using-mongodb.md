---
id: using-mongodb
title: Using MongoDB
sidebar_label: Using MongoDB
---

DenoDB is using [deno_mongo](https://deno.land/x/mongo) for MongoDB support.

```typescript
import { Database, MongoDBConnector } from 'https://deno.land/x/denodb/mod.ts';

const connector = new MongoDBConnector({
  uri: 'mongodb://127.0.0.1:27017',
  database: 'test',
});

const db = new Database(connector);
```

**If you need to connect to your instance using more options** than a bare URI, check the available options from [`deno_mongo` client](https://github.com/manyuanrong/deno_mongo/blob/master/ts/client.ts#L5). **Remember to keep the `database` field in your options as this remains required.**

## Models

Using Mongo means adapting your models to its standards, one being the use of `_id` for identifying records. The only change on your side is to add an `_id` field to your models:

```typescript
class Flight extends Model {
  static fields = {
    _id: {
      primaryKey: true,
    },
  };
}
```

> More on models in the [according section](guides/create-models.md).
