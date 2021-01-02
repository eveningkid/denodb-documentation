---
id: create-models
title: Create models
sidebar_label: Create models
---

Models represent tables in your database. Each model should link to one of your database's tables. Every field should also match the name from your database.

> If the model you are creating does not exist yet in your database, it will automatically be created when calling `new Database(...).sync(...)`.

## Create models

Creating models is very easy. Import `Model` from the library and create a class based on what you plan to modelize. Let's create a `Business` model (see the joke here? alright):

```typescript
import { Model } from 'https://deno.land/x/denodb/mod.ts';

class Business extends Model {}
```

## Set your table name

The name we call our model might differ from the table name in our database. It is usually good practice to use your model name in its plural form, e.g. `Business` and `Flight` would respectively become `businesses` and `flights`.

To set the name for your table, we will use the static `table` property:

```typescript
class Business extends Model {
  static table = 'businesses';
}
```

## Add timestamp default fields

It is very common to have `created_at` and `updated_at` fields in our tables. To quickly add this behavior on a model, set `timestamps` to `true`:

```typescript
class Business extends Model {
  // ...

  static timestamps = true;
}
```

Whenever an instance will be updated, `updated_at` will automatically be modified.

## Add fields

In order to add fields, use the static `fields` property. All the available data types can be found in the [data types section](api/data-types.md).

```typescript
import { DataTypes } from 'https://deno.land/x/denodb/mod.ts';

class Business extends Model {
  // ...

  static fields = {
    id: DataTypes.INTEGER,
    name: DataTypes.STRING,
  };
}
```

You can add as many fields as you want.

## Define a primary key

So far, our fields are using regular types: integer and string. However, we often need to add more information for each of our fields e.g. the maximum length of a string field or in this case, set `id` to be our primary key.

To do so, we need to replace the string returned by `DataTypes.INTEGER` with an object:

```javascript
class Business extends Model {
  // ...

  static fields = {
    id: {
      type: DataTypes.INTEGER,
      primaryKey: true,
    },

    // If we need to set the length for `name`, we can also use
    // an object
    name: {
      type: DataTypes.STRING,
      length: 25,
    },

    // Or use a shortcut (check field descriptors to learn more)
    name: DataTypes.string(25),
  };
}
```

You can learn more about [field descriptors (such as `primaryKey` and `length`) in the according section](api/field-descriptors.md).

## Set defaults

If you want one of your fields to have a default value, use the static `defaults` property to do so:

```typescript
class Business extends Model {
  // ...

  static defaults = {
    name: 'Something About Us',
  };
}
```

This can be done for any of your predefined fields.
