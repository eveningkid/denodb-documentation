---
id: one-to-one
title: One-to-one
sidebar_label: One-to-one
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

We want each business to belong to one owner and therefore each owner to be attached to one owner.

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

On each model, we will now add a method to simply query a model's relationship value, e.g. in this case a business' owner or an owner's business. Let's learn about `hasOne`:

```typescript
class Owner extends Model {
  // ...

  // Fetch a business binded to this owner
  static business() {
    return this.hasOne(Business);
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

`this.hasOne(Owner)` will look for an `Owner` instance where its ID matches the `Business.ownerId` field.

## Generate relationship fields on models

In order for `hasOne` to work, we need to add both `ownerId` and `businessId` fields on respectively `Business` and `Owner` models.

A simple way of doing this, following the correct naming convention is through using the `Relationships.oneToOne` helper:

```typescript
import { Relationships } from 'https://deno.land/x/denodb/mod.ts';

// After both models declarations

Relationships.oneToOne(Business, Owner);

// Before database linking
```

This will automatically:

- Add a `businessId` field to `Owner`
- Add an `ownerId` field to `Business`

Both fields will be set as foreign keys.

> Under-the-hood, `Relationships.belongsTo` is used on both fields.

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

// Bind the owner to a business
await Owner.where('id', '1').update({ businessId: '1' });
```

This is two-fold, as the foreign key constraint couldn't be validated if we were to set a `businessId` for a business that does not exist yet.

## Query models

To query our models, we can now use the methods we created in the first place:

```typescript
await Business.where('id', '1').owner();
// { id: "1", name: "John", businessId: 1 }

await Owner.where('id', '1').business();
// { id: "1", name: "Parisian Café", ownerId: 1 }
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

  static business() {
    return this.hasOne(Business);
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
  };

  static owner() {
    return this.hasOne(Owner);
  }
}

Relationships.oneToOne(Business, Owner);

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

await Owner.where('id', '1').update({ businessId: '1' });

await Business.where('id', '1').owner();
await Owner.where('id', '1').business();

await db.close();
```
