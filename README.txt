# You can use the mongodb shell with jetbrain ide.
# Ctrl+Enter for an execution.
# Ctrl for a new line.

# Go to https://www.json-generator.com to generate random data for testing.

# Database - collection - document - _id

# use to create db if not existing auto on the fly
use test_db

# show db using
db

# drop db
db.dropDatabase()

# recreate
use test_db

# create collection if not existing auto on the fly
db.players.insert(
    {  
     "position":"Right Wing",
     "id":8465166,
     "weight":200,
     "height":"6' 0\"",
     "imageUrl":"http://1.cdn.nhle.com/photos/mugs/8465166.jpg",
     "birthplace":"Seria, BRN",
     "age":37,
     "name":"Craig Adams",
     "birthdate":"April 26, 1977",
     "number":27
    }
)

db.players.insert([
    {
    },
    {
    },
])

# show all collections
show collections

# give out all detailed documents
db.players.find()

# pretty output
db.players.find().pretty()

# give out one document
db.players.findOne()

# remove document
db.players.remove(
    {"_id": ObjectId("")}
)

# update document
db.players.update(
    {"_id": ObjectId("")},
    {  
     "position":"Right Wing",
     "id":8465166,
     "weight":220,
     "height":"6' 0\"",
     "imageUrl":"http://1.cdn.nhle.com/photos/mugs/8465166.jpg",
     "birthplace":"Seria, BRN",
     "age":37,
     "name":"Craig Adams",
     "birthdate":"April 26, 1977",
     "number":27
    }
)

# filter via matched key value
db.players.find(
    {"position": "Goalie"}
)
db.players.findOne(
    {"position": "Goalie"}
)
db.players.find(
    {"position": "Defenseman", "age": 21}
)
db.players.find(
    {
        $or:[
            {"position": "Left Wing"},
            {"position": "Right Wing"},
        ]
    }
)

# gt gte ne
db.players.find(
    {"age": {$gt: 30}}
)

# output desired key value
db.players.find(
    {"position": "Center"},
    {"name":1, _id:0}
)

# limit document
db.players.find(
    {"position": "Center"},
    {"name":1, _id:0}
).limit(3)

# skip document of the first couple 
db.players.find(
    {"position": "Center"},
    {"name":1, _id:0}
).skip(2)

# drop collection
db.players.drop()


use bank

db.users.insert([{},{}])

db.users.find(
    {"age": {$lt:23}}
)

# about the query process
db.users.find(
    {"age": {$lt:23}}
).explain("executionStats")

# ensure index
db.users.ensureIndex({"age": 1})

db.users.getIndexes()

db.users.find(
    {"age": {$lt:23}}
).explain("executionStats")

db.users.dropIndex({"age": 1})

# Do not index every thing ! Indexes need to be updated every time you insert a new document !
# Only index those which can be used a lot and identify the document !

# Aggregation and Groups
db.users.aggregate({
    $group: {
        _id: "$eyeColor",
        total: {$sum: 1}
    }
})

db.users.aggregate({
    $group: {
        _id: "$gender",
        avgAge: {$avg: "$age"}
    }
})

db.users.aggregate({
    $group: {
        _id: "$gender",
        richest: {$max: "$balance"}
    }
})


##### Practical Usage
1   Pymongo Query with Dictionary inside Dictionary?

    {"ONE": {"TWO": {"THREE":"5"}}}

    dbaccess.find("ONE.TWO.THREE": {"$gt": 0})


2   How do I query an array of dictionaries in MongoDB?
    {
        "t": "m",
        "y": "n",
        "A":[ 
                {
                 "name": "x",
                 "value": "1"
                },
                {
                 "name": "y",
                 "value": "2"
                },
                {
                 "name": "z",
                 "value": "1"
                }
            ]
    }

    db.collection.find( {
      "A": { $elemMatch: { name: "x", value: "1" } }
    })