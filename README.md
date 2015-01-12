## Dane Techniczne
 ```
 Procesor: i5-3317U
 RAM: 4GB
 Dysk: 500GB
 OS: Windows 8
 ```
 <script src="https://embed.github.com/view/geojson/AdamZielke/zadanie1/master/zapytanie_Gdansk.geojson"></script>
 
##Zestawienie czasów : 

| Baza Danych |                    Import            |         
|-------------|:----------------------------------------------:|
| Mongo 2.8.0 RC4 |   00:15:03,16   | 
| PostgreSQL  |  00:18:23,59 |  
  


<h2>Zadanie 1</h2>

<h3><b>a)</b></h3>
<p>Import pliku</p>

  ```bash
  $ mongoimport --type csv -c Train --file Train2.csv --headerline
  ```  

<h3><b>b)</b></h3>

  ```bash
  db.Train.count()
  ```
  Liczba obiektów: 6034195 
  
<h3><b>c)</b></h3>

<p>Skrypt konwertujący tagi na tablicę:</p>
 ```js
db.train.find( { "tags" : { $type : 2 } } ).snapshot().forEach(
 function (x) {
  if (!Array.isArray(x.tags)){
    x.tags = x.tags.split(' ');
    db.train.save(x);
}});
 ```
<h2><b>PostgreSQL</b></h2>

<h2><b>Zadanie 1a</b></h2>

<h3><b>Stworzenie tabeli w PostgreSQL :</b></h3>

```sh
CREATE TABLE train
(
  "Id" integer NOT NULL,
  "Title" text NOT NULL,
  "Body" text NOT NULL,
  "Tags" text NOT NULL,
  CONSTRAINT "PK" PRIMARY KEY ("Id")
)
WITH (
  OIDS=FALSE
);
```

***

<h3><b>Zadanie 1b</b></h3>

Wgranie danych do PostgreSQL :

```sh
COPY train FROM 'D:\\baza\\Train.csv' DELIMITER ',' CSV HEADER;
```

Sprawdzenie zaimportowanych rekordów :

```sh
select count(*) as ilosc from train
```
***
<h3><b>Zadanie 1d</b></h3>

<p>Import do mongo</p>
 ```bash
 mongoimport -d geo -c schools < Szkolywyzsze.json
 ```
 
## Koordynaty miast:
  <b>Gdańsk 54.360, 18.639</b>
  
  <b>Łódź 51.783, 19.466</b>
  
  <b>Warszawa 52.259, 21.020</b>
  
## Zapytania

#### Szkoły w odległości do 10km od Gdańska:
 ```js
 db.schools.find( { loc : { $near :
                         { $geometry :
                             { type : "Point" ,
                               coordinates: [ 18.639, 54.360 ] } },
                           $maxDistance : 10000
              } }, { _id: 0 } )
 ```
#### Rezultat: [JSON](geojson/zapytanie_Gdansk.json), [GeoJson](geojson/zapytanie_Gdansk.geojson)
#### Szkoły  w odległości do 10km od Łodzi:
 ```js
 db.schools.find( { loc : { $near :
                         { $geometry :
                             { type : "Point" ,
                               coordinates: [ 19.466, 51.783 ] } },
                           $maxDistance : 10000
              } }, { _id: 0 } )
 ```
#### Rezultat: [JSON](geojson/zapytanie_Lodz.json), [GeoJson](geojson/zapytanie_Lodz.geojson)
#### Szkoły w odległości do 10km od Warszawy:
 ```js
 db.schools.find( { loc : { $near :
                         { $geometry :
                             { type : "Point" ,
                               coordinates: [ 21.020, 52.259 ] } },
                           $maxDistance : 10000
              } }, { _id: 0 } )
 ```
#### Rezultat: [JSON](geojson/zapytanie_Warszawa.json), [GeoJson](geojson/zapytanie_Warszawa.geojson)
