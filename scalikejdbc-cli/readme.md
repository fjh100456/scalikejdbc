# ScalikeJDBC Command Line Interface

A simple console to connect database via JDBC.

## Getting started

Execute setup script as follows.

```sh
curl -L http://git.io/dbconsole | sh
```

The script downloads sbt launcher and setting up `dbconsole` command.

```sh
$ curl -L http://git.io/dbconsole | sh
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  3660  100  3660    0     0   2619      0  0:00:01  0:00:01 --:--:--  2619
--2012-11-29 20:25:26--  http://repo.typesafe.com/typesafe/ivy-releases/org.scala-sbt/sbt-launch/0.12.1/sbt-launch.jar
Resolving repo.typesafe.com... 23.21.39.75
Connecting to repo.typesafe.com|23.21.39.75|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1103618 (1.1M) [application/java-archive]
Saving to: `sbt-launch.jar'

100%[===============================================================================================================>] 1,103,618    571K/s   in 1.9s    

2012-11-29 20:17:41 (580 KB/s) - `sbt-launch.jar' saved [1103618/1103618]


command installed to /Users/k-sera/bin/scalikejdbc-cli/dbconsole
command installed to /Users/k-sera/bin/scalikejdbc-cli/dbconsole_config

Please execute `source ~/.bash_profile`
```

`dbconsole` is pretty simple.

```sh
$ dbconsole -h

dbconsole is an extended sbt console to connect database easily.

Usage:
  dbconsole [OPTION]... [PROFILE]

General options:
  -e, --edit    edit configuration, then exit
  -c, --clean   clean sbt environment, then exit
  -h, --help    show this help, then exit

```

Please try it now with sandbox database.

```sh
$ dbconsole sandbox

Starting sbt console for sandbox...

[info] Set current project to default-8d98e7 (in build file:/Users/seratch/bin/scalikejdbc-cli/)
[info] Starting scala interpreter...
[info] 
import scalikejdbc._
import scalikejdbc.StringSQLRunner._
initialize: ()Unit
Welcome to Scala version 2.9.2 (Java HotSpot(TM) Client VM, Java 1.6.0_29).
Type in expressions to have them evaluated.
Type :help for more information.

scala> "create table members(id bigint primary key, name varchar(256));".run
res0: List[Map[String,Any]] = List(Map(RESULT -> false))

scala> "insert into members values (1, 'Alice')".run
res1: List[Map[String,Any]] = List(Map(RESULT -> false))

scala> "insert into members values (2, 'Bob')".as[Boolean]
res2: Boolean = false

scala> "select * from members".run
res3: List[Map[String,Any]] = List(Map(ID -> 1, NAME -> Alice), Map(ID -> 2, NAME -> Bob))

scala> "select name from members".asList[String]
res4: List[String] = List(Alice, Bob)

scala> "select id from members".asList[Long]
res5: List[Long] = List(1, 2)

scala> "select name from members where id = 2".as[String]
res6: String = Bob

scala> "select name from members where id = 2".asOption[String]
res7: Option[String] = Some(Bob)

scala> "select count(1) from members".as[Long]
res8: Long = 2

scala> tables
COMPANIES
GROUPS
MEMBERS

scala> describe("members")

Table: MEMBERS
+-------+--------------+------+-----+---------+-----------------+-------------+
| Field | Type         | Null | Key | Default | Extra           | Description |
+-------+--------------+------+-----+---------+-----------------+-------------+
| ID    | BIGINT(19)   | NO   | PRI | NULL    |                 |             |
| NAME  | VARCHAR(256) | YES  |     | NULL    |                 |             |
+-------+--------------+------+-----+---------+-----------------+-------------+
Indexes:
  "PRIMARY_KEY_6" UNIQUE, (ID)

scala> :q

[success] Total time: 48 s, completed Nov 29, 2012 8:56:36 PM
```

## Configuration

Use `dbconsole -e` command to edit `~/bin/scalikejdbc-cli/config.properties`.

```properties
sandbox.jdbc.url=jdbc:h2:mem:sandbox
sandbox.jdbc.username=
sandbox.jdbc.password=
#mysql.jdbc.url=jdbc:mysql://localhost:3306/dbname
#postgres.jdbc.url=jdbc:postgresql://localhost:5432/dbname
#oracle.jdbc.url=jdbc:oracle:thin:@localhost:1521:dbname
```



