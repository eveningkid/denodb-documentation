---
id: many-to-many
title: Many-to-many
sidebar_label: Many-to-many
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

We want each business to belong to zero or many owners and each owner to own zero or many businesses (`n..n`).

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

On each model, we will now add a method to simply query a model's relationship value, e.g. in this case a business' owners or an owner's businesses:

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

  // Fetch owners binded to this business
  static owners() {
    return this.hasMany(Owner);
  }
}
```

## Modeling our n..n relationship using a pivot model

You might have heard that in order to modelize `n..n` relationships, we need to create a pivot model which allows each entity to be connected to each other.

We can easily create this pivot model using `Relationships.manyToMany`:

```typescript
import { Relationships } from 'https://deno.land/x/denodb/mod.ts';

// Generate a pivot model for Business and Owner
const BusinessOwner = Relationships.manyToMany(Business, Owner);
```

This will generate a pivot model with three fields:

- `id` to identify a connection between a business and an owner
- `ownerId` to identify which owner is connected to which business
- `businessId` to identify which business is connected to which owner

This is all we need to do to modelize our relationship.

> The field naming convention is the model name, lowercased, followed by `Id`. Another model `Flight` would therefore be represented by a `flightId` field.

## Synchronize models

This is a little different from the usual relationships. We indeed need to also link our newly created pivot model.

It should be put first in the list, before the actual models it's connecting:

```typescript
db.link([BusinessOwner, Business, Owner]);
```

## Create models' values

After linking and syncing our models with the database, we can now create some values:

```typescript
await Owner.create([
  {
    id: '1',
    name: 'John',
  },
  {
    id: '2',
    name: 'Sarah',
  },
]);

await Business.create([
  {
    id: '1',
    name: 'Parisian Café',
  },
  {
    id: '2',
    name: 'Something About Us',
  },
]);

// Bind each business to an owner, both ways
// Both business and owner instances need to be created at this point
await BusinessOwner.create([
  { businessId: '1', ownerId: '1' },
  { businessId: '1', ownerId: '2' },
  { businessId: '2', ownerId: '1' },
]);
```

## Query models

To query our models, we can now use the methods we created in the first place:

```typescript
await Owner.where('id', '1').businesses();
// [
//   { id: "1", businessId: 1, ownerId: 1, name: "Parisian Café" },
//   { id: "2", businessId: 2, ownerId: 1, name: "Something About Us" }
// ]

await Owner.where('id', '2').businesses();
// [ { id: "1", businessId: 1, ownerId: 2, name: "Parisian Café" } ]

await Business.where('id', '2').owners();
// [ { id: "1", businessId: 2, ownerId: 1, name: "John" } ]
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
  };

  static owners() {
    return this.hasMany(Owner);
  }
}

const BusinessOwner = Relationships.manyToMany(Business, Owner);

db.link([BusinessOwner, Business, Owner]);

await db.sync({ drop: true });

await Owner.create([
  {
    id: '1',
    name: 'John',
  },
  {
    id: '2',
    name: 'Sarah',
  },
]);

await Business.create([
  {
    id: '1',
    name: 'Parisian Café',
  },
  {
    id: '2',
    name: 'Something About Us',
  },
]);

await BusinessOwner.create([
  { businessId: '1', ownerId: '1' },
  { businessId: '1', ownerId: '2' },
  { businessId: '2', ownerId: '1' },
]);

await Owner.where('id', '1').businesses();
await Owner.where('id', '2').businesses();
await Business.where('id', '2').owners();

await db.close();
```
