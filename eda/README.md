
# EDA

## Zad 2a

#### Roczny zbiór danych wpisów (ocen) z wikipedii (od 21 lipca 2011 do lipca 2012)

#### [Wikipedia artykuly ](https://datahub.io/dataset/wikipedia-article-ratings)

## Import danych - MongoDB
#### Nazwa plik:

```bash
 aft4.tsv
```

#### Polecenie importu:

```bash
time  mongoimport --host 127.0.0.1 -d test -c wiki --type tsv --headerline --file aft4.tsv

# Czas

# real 34m2.933s </br>
# user 31m38.542s </br>
# sys 4m30.744s
```

#### Historia procesora

![screen1](https://github.com/dsamsoniuk/NoSQL/blob/master/eda/img/s2_g.png?raw=true)

#### Waga zaimportowanej bazy danych (różne formaty)

||tsv|csv|json|sql
| --- | :--: | :--: | :--: | :--: |
|Waga bazy w różnych formatach|2.77GB|2.8GB|10.4GB|14GB|


## Import danych Postgresql
#### Przed importem do bazy musiałem zrobić kilka poprawek:
#### * modyfikacja danych w bazie mongo tzn.
#### * zmiana wartości w zmiennych z Infinity oraz NaN na 0 lub treść brak.
#### Było to wymagane ponieważ przy imporcie bazy danych do postgresa występowały błąd znaku.

#### Historia procesora

![screen1](https://github.com/dsamsoniuk/NoSQL/blob/master/eda/img/postgres-proces.png?raw=true)

```js
db.wiki.update({ $or: [{page_title: Infinity},{page_title: NaN}] },{$set: {page_title: "brak"}},false,true)
```

#### Export danych z mongo do pliku wikipedia.json

```bash
mongoexport --db test --collection wiki --out wikipedia.json
```
#### Import bazy danych do postgresa

```bash
time ./pgfutter --db damian --user damian --pw damian12 json wikipedia.json
```

|  | MongoDB | Postgresql |
| ---- |:---------:|:----:|
|Czas trwania importu danych|31m| 35m|
|Czas zliczenia wierszy| 0s| 9m|

### Wnioski

##### Import do bazy danych mongo byl nieco krótszy i latwiejszy natomiast import do postgresa wymagal modyfikacji danych co pochlonelo czas oraz troche dluzszy czas samego importu juz przygotowanej bazy.

## Zad 2b
### Ilość rekordów



```bash
# MongoDB

db.wiki.count()

# Postgresql - Czas (9min)

select count(*) from import.wikipedia;

#Wynik

47207444
```

## Zad 2c


### Przykładowe polecenia (MongoDB)
#### Polecenie 1 . Wyswielt pierszy rekord

```js

db.wiki.findOne()

{
	"_id" : ObjectId("566eecdc71b9e788d9150668"),
	"timestamp" : NumberLong("20110722000002"),
	"page_id" : 29543332,
	"page_title" : 0,
	"page_namespace" : 0,
	"rev_id" : 419784624,
	"user_id" : 0,
	"rating_key" : 3,
	"rating_value" : 5
}

```

#### Polecenie 2. Wyznacz ilosc wierszy ktore maja id strony 26219828

```js
db.wiki.find({page_id: 26219828},{}).count()

216
```

#### Polecenie 3. Wyswielt 3 rekordy od 49

```js
db.wiki.find({},{_id:0, page_id:1,page_title:1, user_id:1 }).skip(49).limit(3)

{ "page_id" : 32490439, "page_title" : "NULL", "user_id" : 1 }
{ "page_id" : 25748, "page_title" : "Router_(computing)", "user_id" : 0 }
{ "page_id" : 32490439, "page_title" : "NULL", "user_id" : 1 }

```



### Przykładowe polecenia (Postgresql)

#### Polecenie 1. Wyswielt 3 rekordy od 49

```js
select data->>'page_id' as page_id ,data->>'page_title' as page_title from import.wikipedia offset 49 limit 3

page_id  |     page_title     
----------+--------------------
32490439 | NULL
25748    | Router_(computing)
32490439 | NULL
```


#### Polecenie 2. Wyswielt rekord wedlug page_id= 26219828

```js
select data->>'page_id' as page_id ,data->>'page_title' as page_title,data->>'timestamp' as time from import.wikipedia where data->>'page_id'='26219828' limit 1

page_id  |    page_title     |               time               
----------+-------------------+----------------------------------
26219828 | Carlos_Bake_Shop | {"$numberLong":"20110722000030"}
```


#### Polecenie 3 wyswietl pierszy rekord
```js
select * from import.wikipedia limit 1

 {"_id":{"$oid":"566eecdc71b9e788d9150668"},"timestamp":{"$numberLong":"20110722000002"},"page_id":29543332,"page_title":0.0,"page_namespace":0,"rev_id":419784624,"user_id":0,"rating_key":3,"rating_value":5}
```





# Geojson


#### Zmien nazwe street na name, rownież musialem to zrobic z reszta zmiennych

```js
db.restauracje.update({},{$rename:{"borough":"name"}},false,true)
```

#### Wyswietl 6 restauracji

```js
db.restauracje.find({},{_id:0,type:1,"geometry.coordinates":1,"geometry.type":1,"geometry.name":1}).limit(6)
```

### Mapa punktowa
Mapa punktowa która zawiera 6 budynków restauracji:

![mapa 1](https://github.com/dsamsoniuk/NoSQL/blob/master/mapa_punktowa.geojson)
