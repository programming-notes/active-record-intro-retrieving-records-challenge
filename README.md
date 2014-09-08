#Active Record Intro:  Retreiving Records

## Summary

In this challenge, we'll continue working with the database schema that we've built over the previous Active Record Intro challenges.  We'll switch our focus from writing migrations to edit our database schema to working with our models.  Specifically, how to pull records from the database.

```ruby
class Dog < ActiveRecord::Base
end
```

*Figure 1.*  Code for `Dog` class.

We have a pre-written class that we'll work with:  `Dog`.  The class is defined in the file `app/models/dog.rb`, whose code is shown in Figure 1.

Class `Dog` is empty.  There are no methods defined within the class itself; however, it inherits a number of class methods from `ActiveRecord::Base`.  Among these inherited methods, are class methods for retreiving data from the database.  We are going to explore those some of those methods in this challenge.

## Releases

### Pre-release: Create, Migrate, and Seed the Database

1. Run Bundler to ensure that the proper gems have been installed.

2. Use the provided Rake task to create the database.

3. Use the provided Rake task to migrate the database.

4. Use the provided Rake task to seed the database.  Run `bundle exec rake db:seed`.  This will execute the code written in `db/seeds.rb`.  This file has been written to add three dogs to our database:  Tenley, Jayda, and Eleanor.

### Release 0: Exploring

We're going to work with our class from within the console.  To open the console, from the command line, run `bundle exec rake console`.

This executes the `console` Rake task, opening IRB with our environment loaded.  We can interact with our models in the console.  We can select records from the database, insert new records, destroy records, etc.  We're going to sample the class methods for retreiving records that our `Dog` class inherits from ActiveRecord::Base.  For a more comprehensive list of options, see the [RailsGuides](http://guides.rubyonrails.org/active_record_querying.html).  

From within the console run ...

- `Dog.all`

  `::all` is a class method that returns all of the records in the `dogs` table as instances of the `Dog` class.  The individual instances are returned within a collection, an `ActiveRecord::Relation` object that acts much like an array.

  Calling `Dog::all` tells Active Record to generate and execute a SQL query.  We can see the SQL that was executed in the console output: `SELECT "dogs".* FROM "dogs"`.

- `Dog.where(age: 1)`

  `::where` is a class method that can accept a hash argument that specifies the values of specific fields on the database table.  In our case, we want all the dogs whose age is 1.  `::where` will look for all records with values that match our conditions, so as with `::all`, the resulting `Dog` objects are returned in an `ActiveRecord::Relation`.
  
  The SQL executed was `SELECT "dogs".* FROM "dogs"  WHERE "dogs"."age" = 1`.
  
-  `Dog.where("age = ? and name like ?", 1, '%Te%')`

  `::where` can also accept string conditionals, as if you were writing the SQL yourself.

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

