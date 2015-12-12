
# EDA

## Zad 2a
### Import danych MongoDB
#### Pobrany plik:

> RC_2015-01.bz2

#### Polecenie importu:

> time bunzip2 -c RC_2015-01.bz2 | mongoimport --drop --host 127.0.0.1 -d test -c reddit

![screen1](https://github.com/dsamsoniuk/NoSQL/blob/master/eda/img/s1_g.png?raw=true)
![screen1](https://github.com/dsamsoniuk/NoSQL/blob/master/eda/img/s2_g.png?raw=true)


## Wnioski
|- |MongoDB|Postgresql|
|-|:---------:|:----:|
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

> db.reddit.find({author:'jaggazz'}, {author : 1, body : 1}).limit(1)

#### Wynik

> { "\_id" : ObjectId("566ac57f71a205f9b2542d9b"), "body" : "I don't know how to describe it.  Gently pinched two spots weiner length apart and just twisted them about 3or 4 times.", "author" : "jaggazz" }


### Przykładowe polecenia (Postgresql)

> cos tam

## Geojson
=======
| | |
|---|---|
|System|linux 14.04 x64|

>>>>>>> 309ea7b2a194358c93b71ef51700c6f46647a3c2
