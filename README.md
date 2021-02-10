# Simpler - a simpler database story for Go

A small wrapper around [ozzo-dbx](https://github.com/go-ozzo/ozzo-dbx)

Inspired by [HugSQL](https://www.hugsql.org/)

* Embrace SQL
* Load queries from files
* Be happy

## Usage

Sample `users.sql` file in `data/sql` directory of our project.

```sql
-- name: select-user
SELECT * FROM users

-- name: delete-user
DELETE FROM users WHERE id = {:id}
```


```go
package main

import "coderats.dev/simpler"

func main() {
    registry := NewRegistry()

    // Load all *.sql files from a directory
    err := registry.LoadDirectory("data/sql")
    if err != nil {
        panic(err)
    }

    // q is just a Query from dbx package
    // same result as using dbx.NewQuery call
    q := registry.Query("users/select-user")

    var users []User
    err := q.All(&users)

    // Load delete-user query
    q = registry.Query("users/delete-user")
    // Pass named parameters to a query
    q.Bind(dbx.Params{"id": 3})
    // And execute it
    res, err = q.Execute()
}
```
