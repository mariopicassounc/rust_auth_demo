# Installation
## To start a project like this

### Setup diesel ORM

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

Then install the CLI tool
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


We will use MySQL official docker image

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