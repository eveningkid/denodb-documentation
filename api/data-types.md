---
id: data-types
title: Data types
sidebar_label: Data types
---

| Type        | `DataTypes.?` | Example                                                            |
| ----------- | ------------- | ------------------------------------------------------------------ |
| Big Integer | `BIG_INTEGER` | `account: DataTypes.BIG_INTEGER`                                   |
| Integer     | `INTEGER`     | `id: DataTypes.INTEGER`                                            |
| Decimal     | `DECIMAL`     | `balance: { type: DataTypes.DECIMAL, precision?: 2, scale?: 4 }`   |
| Float       | `FLOAT`       | `balance: DataTypes.FLOAT`                                         |
| Uuid        | `UUID`        | `id: DataTypes.UUID`                                               |
| Boolean     | `BOOLEAN`     | `isActive: DataTypes.BOOLEAN`                                      |
| Binary      | `BINARY`      | `isActive: DataTypes.BINARY`                                       |
| Enum        | `ENUM`        | `status: { type: DataTypes.ENUM, values: ["active", "canceled"] }` |
| String      | `STRING`      | `name: { type: DataTypes.STRING, length: 20 }`                     |
| Text        | `TEXT`        | `description: DataTypes.TEXT`                                      |
| Date        | `DATE`        | `registeredAt: DataTypes.DATE`                                     |
| Datetime    | `DATETIME`    | `registeredAt: DataTypes.DATETIME`                                 |
| Time        | `TIME`        | `registeredAt: DataTypes.TIME`                                     |
| Timestamp   | `TIMESTAMP`   | `registeredAt: DataTypes.TIMESTAMP`                                |
| JSON        | `JSON`        | `credentials: DataTypes.JSON`                                      |
| JSONB       | `JSONB`       | `credentials: DataTypes.JSONB`                                     |
