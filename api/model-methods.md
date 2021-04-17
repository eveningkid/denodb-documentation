---
id: model-methods
title: Model methods
sidebar_label: Model methods
---

The following section only shows some examples for each method. The methods signatures can easily be found through your editor intellisense.

## all

Fetch all the model records. It works just as `get` but is more readable when you intend to fetch _all_ records for a model.

```typescript
await Flight.all();
await Flight.select('departure').all();
```

## avg

Compute the average value of a field's values from all the selected records.

```typescript
await Flight.avg('flightDuration');
await Flight.where('destination', 'San Francisco').avg('flightDuration');
```

## count

Count the number of records of a model or filtered by a field name.

```typescript
await Flight.count();
await Flight.where('destination', 'Dublin').count();
```

## create

Create one or multiple records in the current model.

```typescript
await Flight.create({ departure: "Paris", destination: "Tokyo" });
await Flight.create([{ ... }, { ... }]);
```

## delete

Delete selected records.

```typescript
await Flight.where('destination', 'Paris').delete();
```

## deleteById

Delete a record by a primary key value.

```typescript
await Flight.deleteById('1');
```

## field

Return the table name followed by a field name. Passing a second parameter works as the `AS` SQL keyword.

```typescript
await Flight.select(Flight.field('departure', 'flight_departure')).all();
```

## find

Find one or multiple records based on the model primary key. The value type **must** match the primary key type.

```typescript
await Flight.find('64');
// Find a flight where the `id` = '64' because the primary key is `id`
```

## first

Return the first record that matches the current query. Sugar version of `take(1)`.

```typescript
await Flight.where('id', '>', '1').first();
```

## get

Run the current query.

```typescript
await Flight.select('departure').get();
```

## groupBy

Group rows by a given field.

```typescript
await Flight.select('departure').groupBy('departure').all();
```

## join

Join a table to the current query. You might need to use `field` in case field names are overlapping (which might happen with `id`s).

```typescript
await Flight.where(Flight.field('departure'), 'Paris')
  .join(Airport, Airport.field('id'), Flight.field('airportId'))
  .get();
```

### leftJoin

Join a table with left statement to the current query. You might need to use `field` in case field names are overlapping (which might happen with `id`s).

```typescript
await Flight.where(Flight.field('departure'), 'Paris')
	.leftJoin(Airport, Airport.field('id'), Flight.field('airportId'))
	.get();
```

### leftOuterJoin

Join a table with left outer statement to the current query. You might need to use `field` in case field names are overlapping (which might happen with `id`s).

```typescript
await Flight.where(Flight.field('departure'), 'Paris')
	.leftOuterJoin(Airport, Airport.field('id'), Flight.field('airportId'))
	.get();
```

## limit

Limit the number of results returned from the query.

```typescript
await Flight.limit(10).get();
```

## max

Find the maximum value of a field from all the selected records.

```typescript
await Flight.max('flightDuration');
```

## min

Find the minimum value of a field from all the selected records.

```typescript
await Flight.min('flightDuration');
```

## offset

Skip `n` values in the results.

```typescript
await Flight.offset(10).get();
await Flight.offset(10).limit(2).get();
```

## orderBy

Order query results based on a field name and an optional direction. It will order in ascending order by default.

```typescript
await Flight.orderBy('departure').all();
await Flight.orderBy('departure', 'desc').all();
await Flight.orderBy('departure', 'asc').all();
await Flight.orderBy({ departure: 'asc', destination: 'desc' }).all();
```

## select

Indicate which fields should be returned/selected from the query.

```typescript
await Flight.select('id').all();
await Flight.select('id', 'destination').all();
```

## skip

Sugar-version of [`offset`](#offset), skip `n` values in the results.

```typescript
await Flight.skip(10).get();
await Flight.skip(10).limit(2).get();
```

## sum

Compute the sum of a field's values from all the selected records.

```typescript
await Flight.sum('flightDuration');
```

## take

Sugar-version of [`limit`](#limit), limit the number of results returned from the query.

```typescript
await Flight.take(10).get();
```

## update

Update one or multiple records. Also update `updated_at` if `timestamps` is `true`.

```typescript
await Flight.where('departure', 'Dublin').update('departure', 'Tokyo');
await Flight.where('departure', 'Dublin').update({ destination: 'Tokyo' });
await Flight.where('id', '64').update({ destination: 'Tokyo' });
```

## where

Add a `WHERE` clause to your query.

```typescript
await Flight.where('id', '1').get();
await Flight.where('id', '>', '1').get();
await Flight.where({ id: '1', departure: 'Paris' }).get();
```
