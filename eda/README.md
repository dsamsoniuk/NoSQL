
# EDA

## Zad 2a
### Import danych MongoDB
#### Pobrany plik:

> RC_2015-01.bz2

#### Polecenie importu:

> time bunzip2 -c RC_2015-01.bz2 | mongoimport --drop --host 127.0.0.1 -d test -c reddit

#### Czas

>  real 108m21.75s </br>
>  user 163m32.455s </br>
>  sys 6m6.874s


#### Historia procesora

![screen1](https://github.com/dsamsoniuk/NoSQL/blob/master/eda/img/s2_g.png?raw=true)
#### Waga zaimportowanej bazy danych

> 29.5 GB

## Wnioski
| ---- | MongoDB | Postgresql |
| ---- |:---------:|:----:|
|Czas trwania importu danych|108m 21s| brak|
|Czas zliczenia wierszy| 0s| brak|

## Zad 2b
### Zliczanie rekordów
#### Polecenie

> db.reddit.count()

#### Wynik

> 53830000

## Zad 2c
### Przykładowe polecenia (MongoDB)
#### Polecenie

```js
 db.reddit.find({author:'jaggazz'}, {_id:0, author : 1, body : 1,id:1, name:1 }).limit(1)
```
#### Wynik

```js
{
  "id": "cnas905",
  "author": "jaggazz",
  "body": "I don't know how to describe it.  Gently pinched two spots weiner length apart and just twisted them about 3or 4 times.",
  "name": "t1_cnas905"
}
```


#### Polecenie

```js
db.reddit.find({"score" :2, "author" : "BSMason"},{ "_id":0}).skip(2).limit(1)
```

#### Wynik

```js
{
  "created_utc": "1420071568",
  "downs": 0,
  "body": "I have been challenged in this discussion greatly.  I would say that as a  PB, and given our congregational vows at every baptism, I think the whole church should be involved in sharing God's gifts on behalf of the covenant children.  And between you and I (haha), there are very real difficulties for some mothers that aren't a result of laziness, unwillingness, or even lack of understanding of the subjects.  Thank you for the discussion.",
  "distinguished": null,
  "id": "cnasrhy",
  "author_flair_css_class": "",
  "author_flair_text": "RCUS, yo",
  "gilded": 0,
  "controversiality": 0,
  "edited": false,
  "subreddit_id": "t5_2riuy",
  "subreddit": "Reformed",
  "parent_id": "t1_cnasltc",
  "ups": 2,
  "retrieved_on": 1425124043,
  "name": "t1_cnasrhy",
  "link_id": "t3_2qtzje",
  "score_hidden": false,
  "score": 2,
  "author": "BSMason",
  "archived": false
}
```





### Przykładowe polecenia (Postgresql)

> chwilowo brak

# Geojson

### Mapa punktowa

Mapa punktowa która zawiera 5 budynków restauracji:

![mapa 1](https://github.com/dsamsoniuk/NoSQL/blob/master/lokalizacje_geo.geojson)
