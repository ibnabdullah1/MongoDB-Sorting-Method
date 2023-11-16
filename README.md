# MongoDB-Sorting-Method
# How to sort a single field
The basic approach to sorting results in MongoDB is to append the .sort() method onto a query. The ```.sort() ```method takes a document as an argument specifying the fields to sort as well as the sort direction.
The most basic way to sort results it to provide a document specifying a single field indicating the column name with a value of ```1``` indicating an ascending sort:
Note that we're providing a MongoDB projection as the second argument to ```.find()``` to only display certain fields. We're also appending the ```.pretty()``` method to make the output more readable.
```db.students.find({}, {_id: 0,first_name: 1,last_name: 1,dob: 1}).sort({dob: 1}).pretty()```

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




To reverse the ordering, set the sort column to ```-1``` instead of ```1```:
```db.students.find({}, {
    _id: 0,
    first_name: 1,
    last_name: 1,
    dob: 1
}).sort({
    dob: -1
}).pretty()```


``` {
    "first_name" : "Lain",
    "last_name" : "Singh",
    "dob" : ISODate("2013-06-22T00:00:00Z")
}
{
    "first_name" : "Rose",
    "last_name" : "Southby",
    "dob" : ISODate("2011-03-03T00:00:00Z")
}
{
    "first_name" : "Nixie",
    "last_name" : "Languin",
    "dob" : ISODate("2011-02-11T00:00:00Z")
}
{
    "first_name" : "Carol",
    "last_name" : "Apple",
    "dob" : ISODate("2010-10-30T00:00:00Z")
}
{
    "first_name" : "Anthony",
    "last_name" : "Apple",
    "dob" : ISODate("2009-08-16T00:00:00Z")
}
{
    "first_name" : "Spencer",
    "last_name" : "Burton",
    "dob" : ISODate("2008-12-04T00:00:00Z")```




# How to sort on additional fields
MongoDB can use additional fields to control sorting for cases where the primary sort field contains duplicates. To do so, you can pass the extra fields and their sort order within the document that you pass to the sort() function.

For example, if we sort the student documents by last_name, we can get an alphabetical list of students based on that one field:

```db.students.find({}, {
    _id: 0,
    first_name: 1,
    last_name: 1,
}).sort({
    last_name: 1
}).pretty()```

```{ "first_name" : "Carol", "last_name" : "Apple" }
{ "first_name" : "Anthony", "last_name" : "Apple" }
{ "first_name" : "Spencer", "last_name" : "Burton" }
{ "first_name" : "Nixie", "last_name" : "Languin" }
{ "first_name" : "Lain", "last_name" : "Singh" }
{ "first_name" : "Rose", "last_name" : "Southby" }```

However, there are two students with the last name of "Apple" and the returned ordering isn't alphabetical when considering their first name as well.

# To fix this, we can use first_name as a secondary sort field:


```db.students.find({}, {
    _id: 0,
    first_name: 1,
    last_name: 1,
}).sort({
    last_name: 1,
    first_name: 1
}).pretty()```

```{ "first_name" : "Anthony", "last_name" : "Apple" }
{ "first_name" : "Carol", "last_name" : "Apple" }
{ "first_name" : "Spencer", "last_name" : "Burton" }
{ "first_name" : "Nixie", "last_name" : "Languin" }
{ "first_name" : "Lain", "last_name" : "Singh" }
{ "first_name" : "Rose", "last_name" : "Southby" }```

After that further specification, the results match the conventional alphabetical ordering that we would expect for names.

# How to sort using embedded document fields
MongoDB can also sort results based on the values included in embedded documents. To do so, use dot notation to drill down to the appropriate field in the embedded document.

For example, you can sort the student data based on the city where they live, which is a component of the address within each document. Keep in mind that when using dot notation, you need to quote the field names to ensure that they are interpreted correctly:

```db.students.find({}, {
    _id: 0,
    first_name: 1,
    last_name: 1,
    "address.city": 1
}).sort({
    "address.city": 1
}).pretty()```


```{
    "first_name" : "Lain",
    "last_name" : "Singh",
    "address" : {
        "city" : "Brighton"
    }
}
{
    "first_name" : "Carol",
    "last_name" : "Apple",
    "address" : {
        "city" : "Camden"
    }
}
{
    "first_name" : "Anthony",
    "last_name" : "Apple",
    "address" : {
        "city" : "Camden"
    }
}
{
    "first_name" : "Rose",
    "last_name" : "Southby",
    "address" : {
        "city" : "Nambles"
    }
}
{
    "first_name" : "Spencer",
    "last_name" : "Burton",
    "address" : {
        "city" : "Zoofreid"
    }
}
{
    "first_name" : "Nixie",
    "last_name" : "Languin",
    "address" : {
        "city" : "Zoofreid"
    }
}```

You can couple this with additional sort fields to ensure that the results are ordered exactly as you'd like them to be:

```db.students.find({}, {
    _id: 0,
    first_name: 1,
    last_name: 1,
    "address.city": 1,
    "address.street": 1
}).sort({
    "address.city": 1,
    "address.street.name": 1,
    "address.street.number": 1,
    last_name: 1,
    first_name: 1,
}).pretty()```
