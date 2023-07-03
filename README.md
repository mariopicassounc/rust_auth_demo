# Installation
## To start a project like this

### Setup diesel ORM

#### Create project
~~~
cargo new --lib <namefolder>
cd <namefolder>
~~~

In Cargo.toml file add:
~~~
[dependencies]
diesel = { version = "2.1.0", features = ["mysql"] }
dotenvy = "0.15"
~~~

#### Install the CLI tool
~~~
cargo install diesel_cli
~~~
You need to have installed:
<ol>
  <li>libpq for the PostgreSQL backend</li>
  <li>libmysqlclient for the Mysql backend</li>
  <li>libsqlite3 for the SQlite backend</li>
</ol>

If you will only use MySQL you can install it:
~~~
sudo apt-get install libmysqlclient-dev
~~~
And then retry with:
~~~
cargo install diesel_cli --no-default-features --features mysql
~~~


#### MySQL official docker image.

~~~
docker pull mysql

docker run -d \
  --name my_mysql_container \
  -e MYSQL_ROOT_PASSWORD=mysecretpassword \
  -e MYSQL_DATABASE=my_database_name \
  -e MYSQL_USER=my_username \
  -e MYSQL_PASSWORD=my_password \
  -p 3306:3306 \
  mysql:latest
~~~

To check in which IP is running:

Then setup a .env file:

~~~
echo DATABASE_URL=mysql://my_username:my_password@127.0.0.2:3306/my_database_name > .env
~~~

And run diesel-cli to connect:

~~~
diesel setup
~~~

If the last command succeed congrats. You have connected to the mysql container.

#### Create the tables in the MySQL DB using migrations.

~~~
diesel migration generate create_users
~~~

In migrations folder there are the up.sql and down.sql files and the tables are created in up.sql

With down.sql we can delete the tables.

We can apply our new migration:

~~~
diesel migration run
~~~

#### Check the tables in the container.
~~~
docker exec -it my_mysql_container mysql -uroot -p
~~~
Then in the mysql promt:
~~~
SHOW DATABASES;
use my_database_name;
DESCRIBE users;
DESCRIBE sessions;
~~~
