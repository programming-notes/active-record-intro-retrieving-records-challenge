#Active Record Intro:  Retrieving Records

## Summary
In this challenge, we will begin to explore Active Record models, the classes backed up by our database.  We'll learn about some of the different methods available for getting records out of our database.  We'll explore both the methods that we'll rely on day-in-and-day-out as well as some helpful options that we'll use less regularly.  For a more comprehensive list of options, we can always reference the [RailsGuides document on the Active Record Query Interface][RailsGuides Query Interface].  

```ruby
class Dog < ActiveRecord::Base
end
```
*Figure 1.*  Code for the class `Dog`.

We'll continue working with the database schema that we built over the previous Active Record Intro challenges.  We'll switch our focus from the database schema (i.e., migrations) to working with our models.  We'll be working with the class `Dog` throughout the challenge (see Figure 1).  The class is defined in the file `app/models/dog.rb`.

As we can see, the class itself is empty; we define the class, specify that it inherits from `ActiveRecord::Base`, and then close it.  There are no methods defined within the class itself.  All the methods we need for interacting with the database are inherited.

Among the inherited methods, are class methods for retrieving data from the database.  We ask the classes to handle building instances of themselves based on the data in the database—for example, the class `Dog` builds dog instances based on the data in the dogs table.  We are going to explore those some of these class methods in this challenge.


## Releases
### Pre-release: Setup
```
$ bundle install
$ bundle exec rake db:create
$ bundle exec rake db:migrate
$ bundle exec rake db:seed
```
*Figure 2*.  Setting up and seeding the database.

Before we begin pulling data out of our database, we need to create our database and seed it with data.  All the files necessary for this are provided:  the migrations and the seeds file.  We simply need to run the Rake tasks (see Figure 2).

When we run the Rake task to seed the database, we'll be inserting records into the dogs table.  The code that is executed is written in the file `db/seeds.rb`.  This file has been written to add three dogs to our database:  Jayda, Tenley, and Eleanor.

```
bundle exec rake console
```
*Figure 3*.  Executing the Rake task to open IRB with our environment loaded.

We're going to work with our `Dog` class from within the Rake console (i.e., IRB with our environment loaded).  Let's begin by opening the console (see Figure 1).  Once it's open, we can begin interacting with our models.  As we work through each release, we should execute the provided example code ourselves and look at the return values.


### Release 0: All of the Dogs
```ruby
Dog.all
```
*Figure 4*.  Retrieving all the dogs from the database.

Active Record provides a method for returning a collection of all the dogs in the database:  `.all` (see Figure 4).  `.all` is a class method that returns a collection of all of the records in the `dogs` table as instances of the `Dog` class.  Calling `Dog.all` tells Active Record to generate and execute a SQL query, and we can see the query in the console output: `SELECT "dogs".* FROM "dogs"`.  When the data is retrieved from the database, each record is mapped to an instance of the `Dog` class.  All of the instances are placed in a collection, an object that acts like an array.


### Release 1: Dogs Where a Condition is Met
```ruby
Dog.where(age: 1)
```
*Figure 5*.  Passing conditions to `.where` as a hash.

`.where` is a class method that can accept a hash argument that specifies the values of specific fields on the database table (see Figure 5).  In this case, we want all the dogs whose age is 1.  The SQL executed is equivalent to `SELECT "dogs".* FROM "dogs"  WHERE "dogs"."age" = 1`.

The dogs table will be searched for any records that match our conditions.  The assumption is that there could be more than one, so the `Dog` objects are returned in a collection—even if there is only one match.
  
```
Dog.where("age = ? and name like ?", 1, '%Te%')
```
*Figure 6*.  Passing conditions to `.where` as a string.

`.where` can also accept a string, as if we were writing part of the SQL query ourselves (see Figure 6).


- `Dog.order(age: :desc)`

  `::order` allows us to retrieve records ordered by specified attributes.

- `Dog.limit(2)`

  `::limit` returns a maximum number of records equal to the number specified.

- `Dog.count`

  `::count` tells us how many records are in the table.
 
- `Dog.pluck(:name, :age)`

	`::pluck` allows us to retrieve just the values of specified fields.

  
-  `Dog.first`

  `::first` is a class method that returns the first record in the `dogs` table, ordered by primary key.  Optionally, you can pass an argument to get multiple objects back (e.g., `Dog.first(5)`).
  
- `Dog.find(1)`

  `::find` allows us to search for records by primary key.  In our case, we're searching by `id`.  We can also specify an array of ID's, if we're looking for multiple records (e.g., `Dog.find [1, 2]`

- `Dog.order(name: :asc).where(age: 1).limit(1)`

  It's also possible to chain these methods together.  Active Records will interpret the method chain into one SQL statement:  `SELECT  "dogs".* FROM "dogs"  WHERE "dogs"."age" = 1  ORDER BY "dogs"."name" ASC LIMIT 1`

- `exit`

  This will exit the console, just like IRB.  Alternatively, use *control + d*.


### Release 1: `Person` and `Rating` classes

We've explored some of the class methods provided for getting records out of the database.  To complete this challenge, define the `Person` and `Rating` classes, so that they are backed by the database—the `Dog` class if needed.

Tests have been provided.  From the command line, run `bundle exec rake spec` to see the failing tests.  Make all the tests pass before submitting.


[RailsGuides Query Interface]: http://guides.rubyonrails.org/active_record_querying.html

