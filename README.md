# Rust auth tutorial
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

## Authentication
### JWT
Conceptual video: https://www.youtube.com/watch?v=soGRyl9ztjI

Deep dive how it works: https://www.youtube.com/watch?v=_XbXkVdoG_0

The server sign with his private key a JSON Web Token and send it to the client.

https://jwt.io to decode jwt

The JWT has three parts (the three are encoded each one in base64):

Header: cryptography algotithm
~~~
{
  "alg": "HS256",
  "typ": "JWT"
}
~~~

Payload: info about the client 
~~~
{
  "sub": "1234567890",
  "name": "John Doe",
  "iat": 1516239022
}
~~~
Signature: the server sign with his private key the payload and the header.
~~~
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  server-256-bit-secret
)
~~~

The client stores the JWT in LocalStorage for example.
Then sends it in the header of all the requests to the server.

The server:
    result = hash(
        base64UrlEncode(header) + "." +
        base64UrlEncode(payload),
        server-256-bit-secret
    )

So, if the result equals to the signature of the JWT it's OK, it's a authenticated request.

## Authorization
### OAuth

Access delegation

Conceptual video: https://www.youtube.com/watch?v=t4-416mg6iU

Deep dive how it works: https://www.youtube.com/watch?v=3pZ3Nh8tgTE

#### Terms 
Protected Resource: We allow access of these resources to a third-party
Resource Owner: The person using the apps
Resource Server and Authorization Server: Google or Facebook commonly
Client: The app that needs the protected resources of other server



