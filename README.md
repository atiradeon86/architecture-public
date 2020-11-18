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

delete location where id = 3;

select * from location left join tag on location.id = tag.locationId;
```
