---
id: model-records
title: Model records
sidebar_label: Model records
---

Model records are handy if you don't want to use `Model.create`, `Model.delete` or `Model.update` with `where(...)` clauses every time.

However, it is important to note that in order to use model records, you will need to edit your models a little bit by adding every field to your model:

```typescript
class Flight extends Model {
  static fields = {
    _id: { primaryKey: true },
    departure: DataTypes.STRING,
    destination: DataTypes.STRING,
    flightDuration: DataTypes.FLOAT,
  };

  _id!: string;
  departure!: string;
  destination!: string;
  flightDuration!: number;

  // `createdAt` and `updatedAt` should not be added
  // as they will automatically be created by the system
  // if `timestamps = true`
}
```

> The API should not change anytime soon. However, the way fields are added could switch to a different approach as it now comes with "field duplicates".

### Create a new record

You can create a single record without using `Model.create()`, but by instantiating a record and setting its fields one by one. The record will only be created into the database when `save()` is called.

Any missing field will be matched to its default if provided through the model `static defaults` property.

```typescript
const flight = new Flight();
flight.departure = 'London';
flight.destination = 'Roma';
flight.flightDuration = 3;
await flight.save();
```

### Update a record

If you already have a record instance, you can update any of its fields and then call `update()`.

```typescript
// flight is an instance of Flight which extends Model
flight.flightDuration = 2;
await flight.update();
```

### Delete a record

If you already have a record instance, you can delete it from the database using `delete()`.

```typescript
// flight is an instance of Flight which extends Model
await flight.delete();
```
