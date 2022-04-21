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
## full command
psql -h <hostname or ip address> -p <port number of remote machine> -d <database name which you want to connect> -U <username of the database server>

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
## alter table
You can add, drop, alter columns;
```angular2html
alter table table-name drop column column-name;
alter table table-name alter column column-name type new-type;
alter table table-name add column column-name column-type
```
## run command in file
```angular2html
\i xxxxx.sql
```
## date and time
normal formats:
- Date - "YYYY-MM-DD"
- Time - "HH:MM:SS"
- Timestamp - "YYYY-MM-DD HH:MM:SS"
- Timestamptc - "Timestamp with time zone"
- built in postgreSQL function NOW()

## distinct and group by
- DISTINCT only returns unique rows in a result set - and row will only appear once.
- DISTINCT on limits duplicate removal to a set of columns
- GROUP BY is combined with aggregate functions like COUNT(), MAX(). SUM(), AVE() ...
```angular2html
select distinct column-name from table-name;
select distinct (distinct column-name) column-name-A, column-name-B... from table-name;
```
```angular2html
select count(column-name), column-name from table-name group by column-name;
## add where clause and having clause
## where cannot count() in the clause, but having can
select count(column-name), column-name from table-name
where clause group by column-name having clause;
```
Example:
```angular2html
select state from taxdata order by state ASC limit 5;
```
## subqueries
Can use a value or set of values in a query that are computed by another query.

People don't like use them in high performance systems.

```angular2html
# example code
select content from comment where account_id = (select id from account where email='123@gmail.com')
```

## Stored procedure
If you want to SQL automatically update the updated_time, you can define a function. And, each time the row has been changed, the time would be updated.
```angular2html
Create or replace function trigger_set_timestamp()
Returns Trigger as $$
Begin
    new.updated_at = Now();
    return new;
End;
$$ language plpgsql;
```
And, initiate the trigger.
```angular2html
Create Trigger set_timestamp
before update on Table-name
for each row
execute procedure trigger_set_timestamp();
```
drop a trigger
```angular2html
drop trigger trigger-name on table-name
```

## Demo reading and parsing files
```angular2html
SELECT [ ALL | DISTINCT [ ON ( expression [, ...] ) ] ]
    [ * | expression [ [ AS ] output_name ] [, ...] ]
    [ FROM from_item [, ...] ]
    [ WHERE condition ]
    [ GROUP BY [ ALL | DISTINCT ] grouping_element [, ...] ]
    [ HAVING condition ]
    [ WINDOW window_name AS ( window_definition ) [, ...] ]
    [ { UNION | INTERSECT | EXCEPT } [ ALL | DISTINCT ] select ]
    [ ORDER BY expression [ ASC | DESC | USING operator ] [ NULLS { FIRST | LAST } ] [, ...] ]
    [ LIMIT { count | ALL } ]
    [ OFFSET start [ ROW | ROWS ] ]
    [ FETCH { FIRST | NEXT } [ count ] { ROW | ROWS } { ONLY | WITH TIES } ]
    [ FOR { UPDATE | NO KEY UPDATE | SHARE | KEY SHARE } [ OF table_name [, ...] ] [ NOWAIT | SKIP LOCKED ] [...] ]

```

## loading and normalizing CSV data
```angular2html
\copy xy_raw(x, y) from "file-path" with delimiter ',' csv;
```
with header:
```angular2html
\copy unesco_raw(name,description,justification,year,longitude,latitude,area_hectares,category,state,region,iso) FROM 'whc-sites-2018-small.csv' WITH DELIMITER ',' CSV HEADER;
```
initialize and update the foreign table.
```angular2html
CREATE TABLE category (
    id SERIAL,
    title VARCHAR(128) UNIQUE,
    PRIMARY KEY(id)
);

insert into category (name) select distinct category from unesco_raw;
```
update the id for the original table
```angular2html
update unesco_raw set category_id = (select id from category where unesco_raw.category=category.name);
```
## how to generate test data
There are several function you can use to generate random test data for your dataset. There are:
- repeat(): it can generate long strings. Horizontal
- generate_series(): it can generate rows for the data. Vertically. Like Python Range.
- Random(): it can generat the random number from 0 to 1. 

"||" is used to concatenate strings
```angular2html
insert into table_name (column_name)
select (case when (random()<0.5)
        Then 'HelloWorld'
        else 'GoodBye'
        end) || repeat('Neon',5) || generate_series(100,200);
```

## Text function

Where clause:
- like/ilike/not like/not ilike
- similar to/no similar to (cover later as regular expressions)
- =, >, <, >=, <=, in, between

manipulate the result
- lower(), upper()
- strpos(column_name, 'string'). find the start position of the string.
- substr(column_name, start, interval). substring, start from "start" and extract "interval" characters.
- split_part(column name, spliter, n). split the result and return the nth part.
- translate(column_name, old_chars, new_chars). replace the old strings to the new ones.

    
```angular2html
## search for the content containing "150";
select content from textfun where content like '%150%';
select strpos(content,'o') from textfun where content like '%150%';
select substr(content,2,4) from textfun where content like '%150%';
select split_part(content,'o',1) from textfun where content like '%150%';
select translate(content,'eo','EO') from textfun where content like '%150%';
```

## Regular Expressions
```
^        Matches the beginning of a line
$        Matches the end of the line
.        Matches any character
\s       Matches whitespace
\S       Matches any non-whitespace character
*        Repeats a character zero or more times
*?       Repeats a character zero or more times (non-greedy)
+        Repeats a character one or more times
+?       Repeats a character one or more times (non-greedy)
[aeiou]  Matches a single character in the listed set
[^XYZ]   Matches a single character not in the listed set
[a-z0-9] The set of characters can include a range
(        Indicates where string extraction is to start
)        Indicates where string extraction is to end
```
## Tips for text search
- If you search using like and you know there is only one record that matches, ADD Limit 1.