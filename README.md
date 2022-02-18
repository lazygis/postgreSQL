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
## insert data in the table
First, if you want to make sure the table is clean, try the command
```angular2html
DELETE FROM library-name;
```
Then, add the data to the library.
```angular2html
INSERT INTO library-name (property_1, property_2) VALUES (value_1, value_2);
```

