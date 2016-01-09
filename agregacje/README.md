
# Agregacje

```js
1.
db.wiki.aggregate([{ $group: {_id: "page_id", totalPop: { $sum: "$rating_key"} }}, {$match: {totalPop: { $gte: 1*1*2} } }  ]);

wynik :

2. db.wiki.aggregate([{ $project: {page_id:1, page_idEq29543332: { $eq:["page_id", 29543332]} } }, { $group: {_id: "page_id", totalPop: { $sum: "$rating_key"} }}, {$match: {totalPop: { $gte: 1} } }, {$sort: { pop: 1}}  ]);

wynik:


```
