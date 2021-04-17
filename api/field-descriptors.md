---
id: field-descriptors
title: Field descriptors
sidebar_label: Field descriptors
---

A field can simply be defined as such: `field: DataTypes.TYPE`, but in some cases you might need a primary key or a given length for this field.

```javascript
static fields = {
  name: {
    type: DataTypes.STRING,
    length: 22,
    allowNull: true,
  },
  // or
  otherName: {
    ...DataTypes.string(22),
    allowNull: true,
  },
  // or if `allowNull = false`
  realName: DataTypes.string(22),
};
```

## Shortcuts

There are a few _shortcuts_ to define field types.

### String

Set a maximum number of characters for this field.

```javascript
static fields = {
  name: DataTypes.string(25),
};
```

### Decimal

Set a precision and an optional scale values for a decimal field.

```javascript
static fields = {
  // Precision, then scale (which is optional)
  price: DataTypes.decimal(2, 1),
};
```

### Enum

Provide the list of possible values.

```javascript
static fields = {
  status: DataTypes.enum(['active', 'canceled']),
};
```

### Integer

Set a maximum number of digits for this field.

```javascript
static fields = {
	quantity: DataTypes.integer(2)
}
```

## Descriptors

Here is a list of all the field descriptors available:

| Descriptor               | Object attribute | Example                          |
| ------------------------ | ---------------- | -------------------------------- |
| As (rename fields)       | `as`             | `as: "field_name_in_database"`   |
| Type                     | `type`           | `type: DataTypes.STRING`         |
| Primary key              | `primaryKey`     | `primaryKey: true`               |
| Unique                   | `unique`         | `unique: true`                   |
| Auto increment           | `autoIncrement`  | `autoIncrement: true`            |
| Length                   | `length`         | `length: 64`                     |
| Allow null/is nullable   | `allowNull`      | `allowNull: false`               |
| Precision (for decimals) | `precision`      | `precision: 3`                   |
| Scale (for decimals)     | `scale`          | `scale: 2`                       |
| Values (for enums)       | `values`         | `values: ["active", "canceled"]` |
