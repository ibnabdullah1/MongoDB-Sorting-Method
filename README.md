# MongoDB-Sorting-Method
# How to sort a single field
The basic approach to sorting results in MongoDB is to append the .sort() method onto a query. The ```.sort() ```method takes a document as an argument specifying the fields to sort as well as the sort direction.
The most basic way to sort results it to provide a document specifying a single field indicating the column name with a value of ```1``` indicating an ascending sort:
Note that we're providing a MongoDB projection as the second argument to ```.find()``` to only display certain fields. We're also appending the ```.pretty()``` method to make the output more readable.
```
db.students.find({}, {_id: 0,first_name: 1,last_name: 1,dob: 1}).sort({dob: 1}).pretty()
```

The above query will return the students organized by their date of birth in the default ascending order:

```{
    "first_name" : "Spencer",
    "last_name" : "Burton",
    "dob" : ISODate("2008-12-04T00:00:00Z")
}
{
    "first_name" : "Anthony",
    "last_name" : "Apple",
    "dob" : ISODate("2009-08-16T00:00:00Z")
}
{
    "first_name" : "Carol",
    "last_name" : "Apple",
    "dob" : ISODate("2010-10-30T00:00:00Z")
}
{
    "first_name" : "Nixie",
    "last_name" : "Languin",
    "dob" : ISODate("2011-02-11T00:00:00Z")
}
{
    "first_name" : "Rose",
    "last_name" : "Southby",
    "dob" : ISODate("2011-03-03T00:00:00Z")
}
{
    "first_name" : "Lain",
    "last_name" : "Singh",
    "dob" : ISODate("2013-06-22T00:00:00Z")
}```
