# Postgres with replication slot example

Simple example of using docker for Postgres + Wal2JSON enabled.

If you want to see how to use this with Rails [check out rei-docker-rails7-postgres-template](https://github.com/wiseleyb/rei-docker-rails7-postgres-template)

```
docker compose up -d
docker container ls
docker exec -it postgres_repl bash
su postgres
psql postgres
```

```
CREATE TABLE users (
  name varchar(40)
);

SELECT 'init'
FROM pg_create_logical_replication_slot('repl_slot_test',
                                      'wal2json');

select *
from pg_replication_slots
where
plugin = 'wal2json';

insert into users (name) values ('bob');
select * from users;

SELECT *
FROM pg_logical_slot_peek_changes('repl_slot_test',
                                NULL,
                                NULL,
                                'pretty-print',
                                '1');

SELECT *
FROM pg_logical_slot_get_changes('repl_slot_test',
                               NULL,
                               NULL,
                               'pretty-print',
                               '1');
```

## wal2json

The wal2json code used if you're interested:

[https://github.com/eulerto/wal2json](https://github.com/eulerto/wal2json)





