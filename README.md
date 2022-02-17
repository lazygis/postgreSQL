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

quit the session using ``\``q

