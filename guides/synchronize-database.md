---
id: synchronize-database
title: Synchronize database
sidebar_label: Synchronize database
---

Before creating our models, we need to both link and synchronize them to our database.

## Link models

Linking our models is very straight-forward. Use `db.link` with every model you have created:

```javascript
const db = new Database(...);

// Put or import your models before linking them
class Business extends Model { ... }

db.link([Business]);
```

In case of pivot models created with `Relationships.manyToMany`, it is good practice to put them first:

```javascript
const BusinessOwner = Relationships.manyToMany(Business, Owner);

db.link([BusinessOwner, Business, Owner]);
```

## Synchronize models

Synchronizing your models means making sure your models are available in the provided database. If they do not exist, synchronizing will create them.

```javascript
db.sync();
```

Some of these tables might have values already and so you might want to drop them:

```javascript
db.sync({ drop: true });
```

> This will drop every model previously put into `db.link(models)`.
