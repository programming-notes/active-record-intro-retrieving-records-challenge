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
Sometimes we don't want all the records for a given model.  In our example, we won't always need to get all the dogs out of the database.  Maybe we only want the dogs with a specific name or dogs of a particular breed.

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


### Release 2: Getting Dogs in a Certain Order
```
Dog.order(age: :desc)
```
*Figure 7*  Ordering dogs by age from oldest to youngest.

Just as we might not always want to get all the dogs, sometimes we'll want to get dogs returned to us in a certain order: alphabetically by license, oldest to youngest, etc.  `.order` allows us to retrieve records ordered by specified attributes (see Figure 7).


### Release 3: Limiting the Number of Records Returned
```
Dog.limit(2)
```
*Figure 8*.  Limiting the number of records returned to 2.

Sometimes we only want to return a certain number of records.  When we visit a blog, we aren't shown every post ever written to the blog, but maybe only the most recent handful.  `.limit` allows us to specify a maximum number of records to return (see Figure 8).


### Release 4:  A Count of How Many Records Exist
```
Dog.count
```
*Figure 9*.  Retrieving a count of records in the dogs table.

In addition to retrieving records from the database, we can also ask an Active Record model to tell us how many records exist in the database with the `.count` method (see Figure 9).


### Release 5:  Getting Specific Attributes
```
Dog.pluck(:name, :age)
```
*Figure 10*.  Retrieving the names and ages of the dogs.

We don't always need to get all of the data for our dogs.  Perhaps we only need the names, or the license numbers.  `.pluck` allows us to retrieve just the values of specified fields (see Figure 10).

In the methods that we looked at in earlier releases (e.g., `.all`), each record was mapped to an instance of the `Dog` class.  And, the instances were grouped in a collection that was like an array.  `.pluck` doesn't create `Dog` class instances; it just gives us the values we ask for.


### Release 6:  Getting the First Dog
```
Dog.first
```
*Figure 11*. Retrieving the first dog, ordered by primary key.

`.all`, `.where`, `.order`, etc. are methods that returned collections of objects.  Sometimes we are interested in getting just one dog—not a collection of dogs. `.first` is one such method (see Figure 11).  It returns the first record in the `dogs` table, ordered by primary key.  

Optionally, you can pass an argument to get multiple objects back (e.g., `Dog.first(2)`); this would return a collection.


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

