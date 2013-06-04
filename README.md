# Statystyki NBA 2012/2013

### *Adam Grabowski*


## Co zostało zrobione?
1. Pobranie danych w formacie CSV ze strony www.spxn.com/Stats.asp
2. Oczyszczenie dane przy użyciu Google-Refine - usunięcie pustych kolumn, zmiana nazw kolumn, posortowanie
3. Export z Google-Refine w postaci JSON

## Kilka przykładów

```json
    {
      "Player Name" : "Kobe Bryant",
      "Games Played" : 78,
      "Field Goal %" : 46.3,
      "Three Point %" : 32.4,
      "Free Throw %" : 83.9,
      "Points Per Game" : 27.3
    }
    {
      "Player Name" : "Tim Duncan",
      "Games Played" : 69,
      "Field Goal %" : 50.2,
      "Three Point %" : 28.6,
      "Free Throw %" : 81.7,
      "Points Per Game" : 17.8
    }
    {
      "Player Name" : "Marcin Gortat",
      "Games Played" : 61,
      "Field Goal %" : 52.1,
      "Three Point %" : 0,
      "Free Throw %" : 65.2,
      "Points Per Game" : 11.1
    }
```

* Wynikowy plik .json :
https://github.com/AGR89/test/blob/master/NBA-2012-2013-Players-Stats.json

## Agregacje

import do bazy:

`mongoimport --db test --collection players --type json --file players.json --jsonArray`


Ilu jest zawodników w lidze: 

```json
db.players.count()
477
```

Zawodnicy ze średnią punktów na mecz powyżej 25:
```json
db.players.group( { 
key: { name: 1, games: 1, PPG: 1 }
, cond: { PPG: {$gt: 25} }
, reduce: function ( curr, result ) { }
, initial: {} } )

[
    {
		"name" : "Carmelo Anthony",
		"games" : 67,
		"PPG" : 28.7
	},
	{
		"name" : "Kevin Durant",
		"games" : 81,
		"PPG" : 28.1
	},
	{
		"name" : "Kobe Bryant",
		"games" : 78,
		"PPG" : 27.3
	},
	{
		"name" : "LeBron James",
		"games" : 76,
		"PPG" : 26.8
	},
	{
		"name" : "James Harden",
		"games" : 78,
		"PPG" : 25.9
	}
]
```



