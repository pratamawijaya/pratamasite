---
layout: single
title:  "Membuat Simple CRUD Webservice dengan Golang"
date:   2018-07-26 09:00:00 +0700
excerpt: "Membuat simple webservice api dengan bahasa pemrograman golang"
categories: 
    - Programming
tags : 
    - golang
    - sqlite
    - crud
    - api
    - webservice
---

Pada artikel kali ini saya membahas bagaimana membuat sebuah simple CRUD webservice dengan bahasa GoLang, disini saya menggunakan [Echo](https://github.com/labstack/echo) sebagai minimalis webframeworknya.

pertama buat folder baru di $GOPATH

```
cd $GOPATH
mkdir simpleapi
```

lalu buat file **app.go**

```golang
package main

import (
	"database/sql"
	"simpleapi/handlers"

	"github.com/labstack/echo"
	_ "github.com/mattn/go-sqlite3"
)

func main() {
	e := echo.New()
	db := initDB("storage.db")
	migrate(db)

	e.GET("/tasks", handlers.GetTasks(db))
	e.POST("/tasks", handlers.PutTask(db))
	e.PUT("/tasks", handlers.EditTask(db))
	e.DELETE("/tasks/:id", handlers.DeleteTask(db))

	e.Logger.Fatal(e.Start(":8000"))
}

func initDB(filepath string) *sql.DB {
	db, err := sql.Open("sqlite3", filepath)

	if err != nil {
		panic(err)
	}

	if db == nil {
		panic("db nil")
	}

	return db
}

func migrate(db *sql.DB) {
	sql := `
    CREATE TABLE IF NOT EXISTS tasks(
        id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
		name VARCHAR NOT NULL,
		status INTEGER
    );
    `

	_, err := db.Exec(sql)
	// Exit if something goes wrong with our SQL statement above
	if err != nil {
		panic(err)
	}
}

```
