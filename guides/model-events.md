---
id: model-events
title: Model events
sidebar_label: Model events
---

When creating, updating and deleting records, it can be handy to create side-effects e.g. every time a user is deleted, log this somewhere.

## Happening VS Past events

Briefly, you will find in this section two types of events:

- the ones happening **when a record is being** created/updated/deleted
- the ones happening **after a record** was created/updated/deleted

Therefore, the `creating` event means we are looking at a record which is being created, while a `created` event means it's been created already.

## Events

There are three types of events at the moment: creation, update and deletion.

> In the following section, we assume we have created a `Flight` model. [Read more about creating models](guides/create-models.md).

### Create

```javascript
Flight.on('creating', () => {
  console.log('Creating a flight record');
});

Flight.on('created', (model: Flight) => {
  console.log('Created:', model.id);
});
```

### Update

```javascript
Flight.on('updating', () => {
  console.log('Updating a flight record');
});

Flight.on('updated', (model: Flight) => {
  console.log('Updated:', model.id);
});
```

### Delete

```javascript
Flight.on('deleting', () => {
  console.log('Deleting a flight record');
});

Flight.on('deleted', (model: Flight) => {
  console.log('Deleted:', model.id);
});
```

## Using `addEventListener` instead of `on`

You have seen examples using `Model.on(eventType, callback)` so far yet a more common alias is available using `Model.addEventListener(eventType, callback)` which works exactly the same:

```javascript
Flight.addEventListener('creating', () => {
  console.log('Creating a flight record');
});
```

## Stop listening to an event

To stop listening to an event, use `Model.removeEventListener`:

```javascript
const handleUpdate = (model: Flight) => { ... };

Flight.on('updating', handleUpdate);
Flight.removeEventListener('updating', handleUpdate);
```

## Chaining events

Event listeners can be chained easily:

```javascript
Flight.on('updating', () => {
  console.log('Updating a flight record');
}).on('updated', (model: Flight) => {
  console.log('Updated:', model.id);
});
```
