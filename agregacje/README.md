
# Agregacje

Korzystałem z bazy danych "restauracje"






#### Import bazy danych
```js

 time  mongoimport --host 127.0.0.1 -d test -c comp < companies.json


```









### Przykładowy rekord

```js

db.restauracje.findOne();

Wynik:

{
	"_id" : ObjectId("566c9b4f6bd1d003d2e7fa1a"),
	"cuisine" : "Delicatessen",
	"grades" : [
		{
			"date" : ISODate("2014-05-29T00:00:00Z"),
			"grade" : "A",
			"score" : 10
		},
		{
			"date" : ISODate("2014-01-14T00:00:00Z"),
			"grade" : "A",
			"score" : 10
		},
		{
			"date" : ISODate("2013-08-03T00:00:00Z"),
			"grade" : "A",
			"score" : 8
		},
		{
			"date" : ISODate("2012-07-18T00:00:00Z"),
			"grade" : "A",
			"score" : 10
		},
		{
			"date" : ISODate("2012-03-09T00:00:00Z"),
			"grade" : "A",
			"score" : 13
		},
		{
			"date" : ISODate("2011-10-14T00:00:00Z"),
			"grade" : "A",
			"score" : 9
		}
	],
	"restaurant_id" : "40356483",
	"properties" : {
		"prop0" : "value0"
	},
	"geometry" : {
		"building" : "7114",
		"type" : "Point",
		"zipcode" : "11234",
		"coordinates" : [
			-73.9068506,
			40.6199034
		],
		"name" : "Avenue U"
	},
	"type" : "Feature",
	"name" : "Brooklyn"
}

```

### Ilosc rekordow

```js

db.restauracje.find().count();

Wynik:

25358

```



# Aggregation Pipeline





### Pokaz sume punktow w restauracjach gatunku A.

```js

db.restauracje.aggregate([ { $match: { "grades.grade": "A"}},{$unwind: "$grades"}, {$group: { _id: "$name", total: {$sum: "$grades.score"}}}  ]);

wynik :

{ "_id" : "Missing", "total" : 761 }
{ "_id" : "Staten Island", "total" : 35262 }
{ "_id" : "Manhattan", "total" : 426880 }
{ "_id" : "Queens", "total" : 232052 }
{ "_id" : "Bronx", "total" : 93378 }
{ "_id" : "Brooklyn", "total" : 243503 }


```

### Pokaz srednia restauracji w ktorych wystepuje  type "Feature" i posortuj od najmniejszego

```js
 db.restauracje.aggregate([ { $match: { "type": "Feature"}},{$unwind: "$grades"}, {$group: { _id: "$name", total: {$avg: "$grades.score"}}}, {$sort: {total: 1}}  ]);

wynik:

{ "_id" : "Missing", "total" : 9.632911392405063 }
{ "_id" : "Bronx", "total" : 11.036186099942562 }
{ "_id" : "Staten Island", "total" : 11.370957711442786 }
{ "_id" : "Manhattan", "total" : 11.418151216986018 }
{ "_id" : "Brooklyn", "total" : 11.447723132969035 }
{ "_id" : "Queens", "total" : 11.634865110930088 }

```

### Wyswietl rekordy z iloscia najwieksza punktow przeskakujac 2 i wyswietlajac 4 z nich.

```js

db.restauracje.aggregate([ { $match: { "type": "Feature"}},{$unwind: "$grades"}, {$group: { _id: "$name", maksymalny: {$max: "$grades.score"}}}, {$sort: {maksymalny: 1}}, {"$skip": 2}, {"$limit": 4}

Wynik:

{ "_id" : "Bronx", "maksymalny" : 82 }
{ "_id" : "Queens", "maksymalny" : 84 }
{ "_id" : "Brooklyn", "maksymalny" : 86 }
{ "_id" : "Manhattan", "maksymalny" : 131 }


```
