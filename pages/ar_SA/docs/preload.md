---
title: Preloading (Eager Loading)
layout: page
---

## Preload

GORM allows eager loading relations in other SQL with `Preload`, for example:

```go
db. Preload("Orders", func(db *gorm.DB) *gorm.DB {
  return db. Order("orders.amount DESC")
}). Find(&users)
// SELECT * FROM users;
// SELECT * FROM orders WHERE user_id IN (1,2,3,4) order by orders.amount DESC;
```

## Joins Preloading

`Preload` loads the association data in a separate query, `Join Preload` will loads association data using inner join, for example:

```go
db.Joins("Company").Joins("Manager").Joins("Account").First(&user, 1)
db.Joins("Company").Joins("Manager").Joins("Account").First(&user, "users.name = ?", "jinzhu")
db.Joins("Company").Joins("Manager").Joins("Account").Find(&users, "users.id IN ?", []int{1,2,3,4,5})
```

**NOTE** `Join Preload` works with one-to-one relation, e.g: `has one`, `belongs to`

## Preload All

`clause.Associations` can works with `Preload` similar like `Select` when creating/updating, you can use it to `Preload` all associations, for example:

```go
type User struct {
  gorm. Model
  Name       string
  CompanyID  uint
  Company    Company
  Role       Role
}

db. Preload(clause. Associations). Find(&users)
```

## Preload with conditions

GORM allows Preload associations with conditions, it works similar to [Inline Conditions](query.html#inline_conditions)

```go
Find(&users)

// Customize Preload conditions for `Orders`
// And GORM won't preload unmatched order's OrderItems then
db. Preload("Orders", "state = ?", "paid").
```

## Custom Preloading SQL

You are able to custom preloading SQL by passing in `func(db *gorm.DB) *gorm.DB`, for example:

```go
db. OrderItems. Product"). Preload("CreditCard"). Find(&users)

// Customize Preload conditions for `Orders`
// And GORM won't preload unmatched order's OrderItems then
db.
```

## Nested Preloading

GORM supports nested preloading, for example:

```go
db. OrderItems. Product"). Preload("CreditCard"). Find(&users)

// Customize Preload conditions for `Orders`
// And GORM won't preload unmatched order's OrderItems then
db. Preload("Orders", "state = ?", "paid"). OrderItems").
```
