# postgreSQL
This is the learning notes for the PostgresSQL

## Install the software and start
I installed PostgreSQL on mac. Use the command "brew"
```angular2html
brew install postgresql
```
Here is a useful link for installation. [Link](https://www.codementor.io/@engineerapart/getting-started-with-postgresql-on-mac-osx-are8jcopb#iii-getting-started)

## Create an account
First, you need to set your super user password, when you fist log in.
```
psql -U postgres
```

Then, **super user** can create another credential for yourself, because it is risky to use super user to operate the database. Here is the command.
```angular2html
create user username with password 'your password';
```
Remember, string shoudl use ' and each command should end with ;

Then you can create your own database
```angular2html
create database database-name with owner 'owner name'
```

quit the session using ``\q``

## log in to your database
Now, you can use your new credential to log in to your newly created database.
```angular2html
psql database-name user-name
```
The longer version of the command line is:
```angular2html
psql -h host-site -p port-number -U database-name user-name
```
check with tables
```angular2html
\dt
```
check with more details
```angular2html
\d+ table-name
```
Create table, you need to set property types too, like "VARCHAR(128)"
```angular2html
create table table-name(
col-name type,
col-name type,
col-name type
);
```
## add columns to the table
```
alter table table-name add column column-name column-type column-constraints;
alter table table-name add primary key (column-name);
```
## make the combination of multiple keys unique
When you create table, you can set the combination of several keys unique.
```angular2html
create table track(
id serial,
title varchar(128),
album varchar(128),
unique (title, album),
primary key(id)
);
```
In the situation, there is no record have the identical title and album at the same time.
## insert data in the table
First, if you want to make sure the table is clean, try the command
```angular2html
DELETE FROM table-name;
```
Then, add the data to the library.
```angular2html
INSERT INTO table-name (property_1, property_2) VALUES (value_1, value_2);
```
## delete data in the tabel
```angular2html
DELETE FROM table-name WHERE fileter
```
filter can be something like **email = '1234@gmail.com'**;
## update the data in the table
```angular2html
UPDATE table-name SET field='updated item' WHERE filter;
```
It will update all records that satisify the filter.

## retrieve data from the table
Retrieves a group of records -- you can either retrieve all the records or a subset of the records with a WHERE clause.
```angular2html
SELECT * FROM table-name;
SELECT * FROM table-name WHERE filter
```
## sort data in the table
Sort the retrieved data by a field. The default order is ascending. You can change it by adding "DESC" toward the end.
```angular2html
SELECT * FROM table-name ORDER BY field (DESC)
```
## wildcard matching in WHERE
This operation would be slower than the only WHERE function.
```angular2html
SELECT * FROM table-name WHERE field LIKE '%e%'
```
%e% here is an example of the filter you want.

## limit the number of retrieved records
When you used the WHERE and ORDER BY, and you don't want to check all the satisfied records, you can use LIMIT/OFFSET. OFFSET means starts from row n.
```angular2html
SELECT * FROM table-name ORDER BY field DESC OFFSET 1 LIMIT 2
```
This example gets the 2nd and 3rd records after ordered by one field and in a descending order. 
## count the number of records
```angular2html
SELECT COUNT(*) FROM table-name WHERE filter
```

## relational dataset
Use foreign keys to connect multiple tables. "on delete cascade" in the code means when the user delete the one in a "one to many" relationship, the "many" entries would be deleted, too.
```angular2html
create table artist(
id serial,
name varchar(128) unique,
primary key(id)
);

create table album(
id serial,
title varchar(128) unique,
artist_id integer references artist(id) on delete cascade,
primary key(id)
);

```
## select data with JOIN
When you want to show keys in multiple tables linked by foreign keys, you should use JOIN.
```angular2html
Select album.title, artist.name
From album
Join artist ON album.artist = artist.id
```
If you want to show all combinations, use cross join.
```angular2html
select track.title, track.genre_id, genre.id, genre.name
from track
cross join genre.
```