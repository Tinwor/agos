# AGPS
### Or, for  friends, Another Golang Project Structure
![Just a random image about golang](https://talks.golang.org/2013/advconc/race.png)

The main purpose of this repository is describe the general behaviour of an architecture that I'm using often for some simple Golang projects.
The idea behind this project architecture is create indipendent ~~component~~ (sorry I mean struct) and try to avoid code repetion. 

### Generic overview
The project structure is splitted in the following macro categories: 

 - Models: Files containing data structures are included in this folder.
 - Repositories: Files that communicates with databases (SQL or NO-SQL, it has no relevance) data manipulation (and their interfaces) are stored in this folder.
 - Services: this folder contains all method that modify data coming from Repositories files or use cases (this folder also contains the Mapping files to and from DTO).
 - Controllers: Files containing API definitions are stored in this folder (this files must contains only the API definitions and route handling, data manipulation is delegated to Services folder)
 - Middlewares: in this folder are stored the middlewares of your project (if you don't know what is a middleware take a look [here](https://en.wikipedia.org/wiki/Middleware) and then the [Best pratices and example](https://www.nicolasmerouze.com/middlewares-golang-best-practices-examples/).
 - Utils: this folder contains all the other files: e.g. a configurator that load configurations from files.

And this is the general overview, let's create an example and see how it works.
For this simple project I will use [postgres](github.com/lib/pq) to store data, [httprouter](https://github.com/julienschmidt/httprouter) for handling request and [toml](https://github.com/BurntSushi/toml) for file configuration.

### **Create the model**
For this very simple and basic application we will use a db with only one table (books).
Here the sql statement to create the table books in your test db.
```SQL
CREATE TABLE books
(
    uid serial NOT NULL,
    isbn character varying(100) NOT NULL,
    title character varying(100) NOT NULL,
    description character varying(500) NOT NULL,
    inserted date NOT NULL,
    quantity NUMERIC(9) NOT NULL,
    price NUMERIC(6,2) NOT NULL
    CONSTRAINT books_pkey PRIMARY KEY (uid)
)
WITH (OIDS=FALSE);
```
Well, create our model file in ```GOPATH/my/folder/project/models/postgres``` and write some Go code:
```Golang
package  postgres

import "time"

type Book struct {
    Uid interfaces
    Isbn string
    Title string
    Description string
    Inserted time.Time
    Quantity int 
    Price float32
}
```
Now we can create the repository to connect the data inside the database to all the other upper layer.  
To make this layer transparent to the other we will also create an interface for each repository and we will use it later.
So inside ``` GOPATH/my/folder/project/repositories/irepositories``` we will create the interface and ``` GOPATH/my/folder/project/repositories/``` we will create the repository implementation.

```Golang
package irepositories

import "my/folder/project/models/postgres"
type(
    IBookRepository interface {
        AddNewBook(b Book) (bool, error)
        EditBook(b Book, id int) (bool, eror)
        DeleteBook(id int) (bool, error)
    }
)
```