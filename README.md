# Szoftverarchitektúrák képzés anyaga

## Standalone konzolos alkalmazás

Töltsd le a standalone konzolos alkalmazást a https://github.com/Training360/architecture-public/releases/download/1.0.0/LocationsCli-1.0.0.exe
címről! Indítsd el! Telepíteni fogja a `C:\Program Files\LocationsCli` helyre.

Parancssorban navigálj ebbe a könyvtárba, és indítsd el a `LocationsCli` beírásával az alkalmazást!

Valami hasonlót kell látnod:

```shell
C:\Program Files\LocationsCli> LocationsCli
1. List locations
2. Create location
3. Edit location
4. Delete location
5. Exit

Select a number, than press Enter!
```

## Standalone alkalmazás grafikus felülettel

Töltsd le a standalone alkalmazást grafikus felülettel a https://github.com/Training360/architecture-public/releases/download/1.0.0/locations-win32-64x.zip
címről! Tömörítsd ki, majd az `electron.exe` állományt indítsd el!

## Központi adatbázis

Amennyiben Docker telepítve van a gépedre, indítsd el az adatbázist a
következő paranccsal:

```script
docker run -d -e MYSQL_DATABASE=locations -e MYSQL_USER=locations -e MYSQL_PASSWORD=locations -e MYSQL_ALLOW_EMPTY_PASSWORD=yes -p 3306:3306 --name locations-dbclient-mariadb mariadb
```

Írd át az alkalmazás konfigurációs állományában, hogy az adatbázishoz kapcsolódjon!
Ennek kell benne szerepelnie:

```javascript
config.store = "db"
config.db.type = "mysql"
config.db.url = "mysql://locations:locations@localhost:3306/locations"
config.db.logging = false
```

Majd indítsd el az alkalmazást!

## SQL nyelv

* SQL konzol

```shell
docker exec -it locations-dbclient-mariadb mysql locations
```

* SQL utasítások

```sql
desc location;

select * from location;

insert into location(name, lat, lon) values ('Work2', 3, 3);

update location set name = 'Work3' where id = 3;

delete from location where id = 3;

select * from location left join tag on location.id = tag.locationId;
```

## NoSQL adatbázisok

MongoDB elindítása:

```shell
docker run -d -p27017:27017 --name locations-mongo mongo
```

Parancssoros kliens elindítása:

```shell
docker exec -it locations-mongo mongo locations
```

Parancsok:

```javascript
db.location.find()

db.location.insert({name: "Work", lat: 2, lon: 2})

db.location.update({_id: "5f8e8e8237d3c021af6da9b6"}, {$set: {name: "Work2"}})
```

## Többrétegű alkalmazások

Adatbázis és szerver alkalmazás elindítása:

```shell
docker network create locations-net

docker run -d -e MYSQL_DATABASE=locations -e MYSQL_USER=locations -e MYSQL_PASSWORD=locations -e MYSQL_ALLOW_EMPTY_PASSWORD=yes -p 3306:3306 --network locations-net --name locations-mariadb mariadb

docker run -d -e SPRING_DATASOURCE_URL=jdbc:mariadb://locations-mariadb/locations -p 8080:8080 --network locations-net --name=my-locations training360/locations
```

Napló listázása:

```shell
docker logs -f my-locations
```
