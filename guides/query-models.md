---
id: query-models
title: Query models
sidebar_label: Query models
---

Once we have all the models created, we can easily query them.

## Using query builder

Query builder allows you to generate queries using only simple, abstract methods.

> More information about available methods can be found in [Model methods](api/model-methods.md).

Here is a list of examples:

```typescript
await Flight.create([
  {
    departure: 'Paris',
    destination: 'Tokyo',
  },
  {
    departure: 'London',
    destination: 'San Francisco',
  },
]);

await Flight.select('destination').all();
// [ { destination: "Tokyo" }, { destination: "San Francisco" } ]

await Flight.where('destination', 'Tokyo').delete();

await Flight.all();
// [
//  {
//    id: 2,
//    departure: "London",
//    destination: "San Francisco",
//    flightDuration: 2.5,
//    created_at: 2020-05-17T13:16:32.333Z,
//    updated_at: 2020-05-17T13:16:32.333Z
//   }
// ]

await Flight.select('destination').find('2');
// [ { destination: "San Francisco" } ]

await Flight.count();
// 1

await Flight.select('id', 'destination').orderBy('id').get();
// [ { id: "2", destination: "San Francisco" } ]
```

## Using model records

You can also create, update and delete model records by using the model record itself.

> More information about using model records can be found in [Model records](api/model-records.md).

```typescript
const flight = new Flight();
flight.departure = 'Dublin';
flight.destination = 'Paris';
flight.flightDuration = 3;
await flight.save();

flight.departure = 'Tokyo';
await flight.update();

await flight.delete();
```
