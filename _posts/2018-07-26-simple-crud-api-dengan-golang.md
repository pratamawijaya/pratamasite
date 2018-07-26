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

	// daftar api
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

pada fungsi main, kita mendefiniskan inisiasi echo, 
```golang
e := echo.New()

```
lalu langkah selanjutnya adalah inisiasi database, disertai pembuatan tablenya
```golang

db := initDB("storage.db")
migrate(db)

```

selanjutnya define daftar api yang akan kita buat

```golang
	e.GET("/tasks", handlers.GetTasks(db))
	e.POST("/tasks", handlers.PutTask(db))
	e.PUT("/tasks", handlers.EditTask(db))
	e.DELETE("/tasks/:id", handlers.DeleteTask(db))
```

selanjutnya untuk menjalankan server pada port 8000, gunakan kode berikut
```golang
e.Logger.Fatal(e.Start(":8000"))
```

langkah selanjutnya buat dua buah folder yakni **handlers** dan **models**, 

- handlers : digunakan untuk komunikasi dengan models, return dari handlers ini sebaiknya langsung berupa JSON
- models : digunakan untuk akses query ke database

class **models/task.go**

```golang
package models

import (
	"database/sql"
)

type Task struct {
	ID     int    `json:"id"`
	Name   string `json:"name"`
	Status int    `json:"status"`
}

type TaskCollection struct {
	Tasks []Task `json:"items"`
}

func GetTasks(db *sql.DB) TaskCollection {
	sql := "SELECT * FROM tasks"
	rows, err := db.Query(sql)
	// Exit if the SQL doesn't work for some reason
	if err != nil {
		panic(err)
	}
	// make sure to cleanup when the program exits
	defer rows.Close()

	result := TaskCollection{}
	for rows.Next() {
		task := Task{}
		err2 := rows.Scan(&task.ID, &task.Name, &task.Status)
		// Exit if we get an error
		if err2 != nil {
			panic(err2)
		}
		result.Tasks = append(result.Tasks, task)
	}
	return result
}

func PutTask(db *sql.DB, name string, status int) (int64, error) {
	sql := "INSERT INTO tasks(name, status) VALUES(?,?)"

	// Create a prepared SQL statement
	stmt, err := db.Prepare(sql)
	// Exit if we get an error
	if err != nil {
		panic(err)
	}
	// Make sure to cleanup after the program exits
	defer stmt.Close()

	// Replace the '?' in our prepared statement with 'name'
	result, err2 := stmt.Exec(name, status)
	// Exit if we get an error
	if err2 != nil {
		panic(err2)
	}

	return result.LastInsertId()
}

func EditTask(db *sql.DB, taskId int, name string, status int) (int64, error) {
	sql := "UPDATE tasks set name = ?, status = ? WHERE id = ?"

	stmt, err := db.Prepare(sql)

	if err != nil {
		panic(err)
	}

	result, err2 := stmt.Exec(name, status, taskId)

	if err2 != nil {
		panic(err2)
	}

	return result.RowsAffected()
}

func DeleteTask(db *sql.DB, id int) (int64, error) {
	sql := "DELETE FROM tasks WHERE id = ?"

	// Create a prepared SQL statement
	stmt, err := db.Prepare(sql)
	// Exit if we get an error
	if err != nil {
		panic(err)
	}

	// Replace the '?' in our prepared statement with 'id'
	result, err2 := stmt.Exec(id)
	// Exit if we get an error
	if err2 != nil {
		panic(err2)
	}

	return result.RowsAffected()
}

```

untuk handlersnya 

```golang
package handlers

import (
	"database/sql"
	"net/http"
	"simpleapi/models"
	"strconv"

	"github.com/labstack/echo"
)

type H map[string]interface{}

func GetTasks(db *sql.DB) echo.HandlerFunc {
	return func(c echo.Context) error {
		return c.JSON(http.StatusOK, models.GetTasks(db))
	}
}

func PutTask(db *sql.DB) echo.HandlerFunc {
	return func(c echo.Context) error {

		var task models.Task

		c.Bind(&task)

		id, err := models.PutTask(db, task.Name, task.Status)

		if err == nil {
			return c.JSON(http.StatusCreated, H{
				"created": id,
			})
		} else {
			return err
		}

	}
}

func EditTask(db *sql.DB) echo.HandlerFunc {
	return func(c echo.Context) error {

		var task models.Task
		c.Bind(&task)

		_, err := models.EditTask(db, task.ID, task.Name, task.Status)

		if err == nil {
			return c.JSON(http.StatusOK, H{
				"updated": task,
			})
		} else {
			return err
		}
	}
}

func DeleteTask(db *sql.DB) echo.HandlerFunc {
	return func(c echo.Context) error {
		id, _ := strconv.Atoi(c.Param("id"))

		_, err := models.DeleteTask(db, id)

		if err == nil {
			return c.JSON(http.StatusOK, H{
				"deleted": id,
			})
		} else {
			return err
		}

	}
}

```
