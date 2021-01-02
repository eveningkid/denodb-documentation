---
id: one-to-many
title: One-to-many
sidebar_label: One-to-many
---

In this guide, we will consider the following two entities:

```
owner:
  - id
  - name

business:
  - id
  - name
```

We want each business to belong to one owner and each owner to own zero or many businesses (`0..n`).

## Create models

Let's first create our models:

```javascript
import { Model } from 'https://deno.land/x/denodb/mod.ts';

class Owner extends Model {
  static table = 'owners';

  static fields = {
    id: {
      type: DataTypes.STRING,
      primaryKey: true,
    },
    name: DataTypes.STRING,
  };
}

class Business extends Model {
  static table = 'businesses';

  static fields = {
    id: {
      type: DataTypes.STRING,
      primaryKey: true,
    },
    name: DataTypes.STRING,
  };
}
```

## Add querying methods to models

On each model, we will now add a method to simply query a model's relationship value, e.g. in this case a business' owner or an owner's businesses:

```typescript
class Owner extends Model {
  // ...

  // Fetch businesses binded to this owner
  static businesses() {
    return this.hasMany(Business);
  }
}

class Business extends Model {
  // ...

  // Fetch an owner binded to this business
  static owner() {
    return this.hasOne(Owner);
  }
}
```

- `this.hasMany(Business)` will look for `Business` instances where their `Business.ownerId` matches the `Owner.id` (its primary key).
- `this.hasOne(Owner)` will look for an `Owner` instance where its ID matches the `Business.ownerId` field.

## Add a foreign key

In order for this relationship to work, we need to add an `ownerId` field to `Business`.

You can easily add this foreign key to `Business` using `Relationships.belongsTo`:

```typescript
import { Relationships } from 'https://deno.land/x/denodb/mod.ts';

class Business extends Model {
  // ...

  static fields = {
    // ...
    ownerId: Relationships.belongsTo(Owner),
  };

  // ...
}
```

`ownerId` will be set as a foreign key on `Owner.id`.

## Create models' values

After linking and syncing our models with the database, we can now create some values:

```typescript
await Owner.create({
  id: '1',
  name: 'John',
});

await Business.create({
  id: '1',
  name: 'Parisian Café',

  // Bind the business to an owner
  ownerId: '1',
});

await Business.create({
  id: '2',
  name: 'Something About Us',

  // Same here
  ownerId: '1',
});
```

## Query models

To query our models, we can now use the methods we created in the first place:

```typescript
await Owner.where('id', '1').businesses();
// [
//   { id: "1", name: "Parisian Café", ownerId: 1 },
//   { id: "2", name: "Something About Us", ownerId: 1 }
// ]

await Business.where('id', '1').owner();
// { id: "1", name: "John" }

await Business.where('id', '2').owner();
// { id: "1", name: "John" }
```

## Example

```javascript
const db = new Database(...);

class Owner extends Model {
  static table = 'owners';

  static fields = {
    id: {
      type: DataTypes.STRING,
      primaryKey: true,
    },
    name: DataTypes.STRING,
  };

  static businesses() {
    return this.hasMany(Business);
  }
}

class Business extends Model {
  static table = 'businesses';

  static fields = {
    id: {
      type: DataTypes.STRING,
      primaryKey: true,
    },
    name: DataTypes.STRING,
    ownerId: Relationships.belongsTo(Owner),
  };

  static owner() {
    return this.hasOne(Owner);
  }
}

db.link([Owner, Business]);

await db.sync({ drop: true });

await Owner.create({
  id: '1',
  name: 'John',
});

await Business.create({
  id: '1',
  name: 'Parisian Café',
  ownerId: '1',
});

await Business.create({
  id: '2',
  name: 'Something About Us',
  ownerId: '1',
});

await Owner.where('id', '1').businesses();
await Business.where('id', '1').owner();
await Business.where('id', '2').owner();

await db.close();
```
