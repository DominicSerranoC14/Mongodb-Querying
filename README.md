# Mongodb-Querying
This exercise uses a sample database of restaurants to query multiple sets of requested information. The requests are defined in the exercises section.


### Exercises
#

1. Provide a query showing just the names (and nothing else) of the Italian restaurants.
```
Dominics-MacBook-Air(mongod-3.2.9) test>
db.restaurants.find({cuisine: 'Italian'}, {name: true, _id: false}).limit(3)

{
  "name": "Philadelhia Grille Express"
}
{
  "name": "Isle Of Capri Resturant"
}
{
  "name": "Marchis Restaurant"
}
```
#

2. Provide a query showing how many Bakeries have a name that start with M.
```
Dominics-MacBook-Air(mongod-3.2.9) test>
db.restaurants.find({cuisine: 'Bakery', name: /^M/}, {name: true}).length()

62
```


3. Provide a query showing the zip codes (and id's) of all restaurants with the word "Ice" in their cuisine.
```
Dominics-MacBook-Air(mongod-3.2.9) test>
db.restaurants.find({cuisine: /^Ice/}, {'address.zipcode': true}).limit(3)

{
  "_id": ObjectId("57d6d6dcce82c84ae24c270f"),
  "address": {
    "zipcode": "11226"
  }
}
{
  "_id": ObjectId("57d6d6dcce82c84ae24c2716"),
  "address": {
    "zipcode": "11004"
  }
}

{
  "_id": ObjectId("57d6d6dcce82c84ae24c2719"),
  "address": {
    "zipcode": "11218"
  }
}
```


4. Provide a query showing the street and street number of Chinese restaurants ordered by zip code.
```
Dominics-MacBook-Air(mongod-3.2.9) test>
db.restaurants.find({cuisine: 'Chinese'}, {'address.zipcode': true, 'address.building
': true, 'address.street': true}).sort({'address.zipcode': 1}).limit(3)
{
  "_id": ObjectId("57d6d6dcce82c84ae24c2f2d"),
  "address": {
    "building": "224",
    "street": "West 35 Street",
    "zipcode": "10001"
  }
}
{
  "_id": ObjectId("57d6d6dcce82c84ae24c4a8d"),
  "address": {

    "building": "413415",
    "street": "9 Avenue",
    "zipcode": "10001"
  }
}
{
  "_id": ObjectId("57d6d6dcce82c84ae24c4c5d"),
  "address": {
    "building": "423",
    "street": "9 Avenue",
    "zipcode": "10001"
  }
}
```


5. Show only the American restaurants in Manhattan.
```
Dominics-MacBook-Air(mongod-3.2.9) test> db.restaurants.find({cuisine: 'American ', borough: 'Manhattan'}, {name: true}).limit(3)
{
  "_id": ObjectId("57d6d6dcce82c84ae24c2711"),
  "name": "1 East 66Th Street Kitchen"
}
{
  "_id": ObjectId("57d6d6dcce82c84ae24c2718"),
  "name": "Glorious Food"
}
{
  "_id": ObjectId("57d6d6dcce82c84ae24c271e"),
  "name": "P & S Deli Grocery"
}
```


6. Provide a query showing the restaurants that have been graded exactly 4 times.
```
Dominics-MacBook-Air(mongod-3.2.9) test>
db.restaurants.find({grades: {$size: 4}}, {name: true, 'grades.score': true}).limit(2
)

{
  "_id": ObjectId("57d6d6dcce82c84ae24c2707"),
  "grades": [
    {
      "score": 8
    },
    {
      "score": 23
    },
    {
      "score": 12

    },
    {
      "score": 12
    }
  ],
  "name": "Wendy'S"
}
{
  "_id": ObjectId("57d6d6dcce82c84ae24c2709"),
  "grades": [
    {
      "score": 2
    },
    {
      "score": 11
    },
    {
      "score": 12
    },
    {
      "score": 12
    }
  ],
  "name": "Dj Reynolds Pub And Restaurant"
}
```


7. Provide a query showing only id, name and 2 grades from each restaurant on Broadway.
```
Dominics-MacBook-Air(mongod-3.2.9) test>
db.restaurants.find({'address.street': 'Broadway'}, {name: true, grades: {$slice: 2}}
).limit(2)

{
  "_id": ObjectId("57d6d6dcce82c84ae24c271a"),
  "grades": [
    {
      "date": ISODate("2014-01-21T00:00:00Z"),
      "grade": "A",
      "score": 12
    },
    {
      "date": ISODate("2013-01-04T00:00:00Z"),
      "grade": "A",

      "score": 11
    }
  ],
  "name": "Bully'S Deli"
}
{
  "_id": ObjectId("57d6d6dcce82c84ae24c273e"),
  "grades": [
    {
      "date": ISODate("2014-03-08T00:00:00Z"),
      "grade": "A",
      "score": 12
    },
    {
      "date": ISODate("2013-09-28T00:00:00Z"),
      "grade": "A",
      "score": 10

    }
  ],
  "name": "Peter Luger Steakhouse"
}
```


8. Provide a query showing the 5 pizza restaurants in the Bronx with the highest score on an evaluation.
```
Dominics-MacBook-Air(mongod-3.2.9) test>
db.restaurants.find({'grades.score': {$gt: 80}}, {name: true}).sort({'grades.score':
1}).limit(5)

{
  "_id": ObjectId("57d6d6dcce82c84ae24c4dc7"),
  "name": "Anella"
}
{
  "_id": ObjectId("57d6d6ddce82c84ae24c5b21"),
  "name": "Cafe R"
}
{
  "_id": ObjectId("57d6d6ddce82c84ae24c82f8"),
  "name": "La Potencia Restaurant"
}
{
  "_id": ObjectId("57d6d6dcce82c84ae24c3812"),
  "name": "Spicy Shallot"
}
{
  "_id": ObjectId("57d6d6dcce82c84ae24c3843"),
  "name": "Bistro Caterers"
}
```


9. Provide a query to find all of the restaurants in Brooklynn and list only the 21st-26th results when ordered alphabetically by name
```
Dominics-MacBook-Air(mongod-3.2.9) test>
db.restaurants.find({borough: 'Brooklyn', name: /^A/}, {name: true}).sort({name: 1}).
skip(20).limit(5)

{
  "_id": ObjectId("57d6d6ddce82c84ae24c803f"),
  "name": "Abide Brooklyn Pita"
}
{
  "_id": ObjectId("57d6d6ddce82c84ae24c68dd"),
  "name": "Abigael'S Brooklyn"
}

{
  "_id": ObjectId("57d6d6dcce82c84ae24c3ed6"),
  "name": "Abilene"
}
{
  "_id": ObjectId("57d6d6dcce82c84ae24c344b"),
  "name": "Abir Halal Restaurant"
}
{
  "_id": ObjectId("57d6d6ddce82c84ae24c7af5"),
  "name": "Absolute Coffee"
}
```


10. Provide a query that returns all pizza and Italian restaurants in reverse alphabetic order.
```
Dominics-MacBook-Air(mongod-3.2.9) test>
db.restaurants.find({cuisine: 'Italian', cuisine: 'Pizza'}, {name: true}).sort({name:
 -1}).limit(5)

{
  "_id": ObjectId("57d6d6ddce82c84ae24c6b78"),
  "name": "Zz'S Pizza & Grill"
}
{
  "_id": ObjectId("57d6d6dcce82c84ae24c51d7"),

  "name": "Yummy Fried Chicken, Pizza, & Grill"
}
{
  "_id": ObjectId("57d6d6ddce82c84ae24c63a3"),
  "name": "Yankee'S Sk Pizza"
}
{
  "_id": ObjectId("57d6d6dcce82c84ae24c286a"),
  "name": "Yankee Jz Pizza"
}
{
  "_id": ObjectId("57d6d6ddce82c84ae24c7fb3"),
  "name": "Xochil Pizza"
}
```
