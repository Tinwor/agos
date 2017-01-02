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