## Create new project

```bash
# create a new empty repo on github called blog
# run docker desktop
$ sail_new blog
# type password
$ git_local_init
$ git_remote_init
# update .env and .env.example file settings to
# DB_DATABASE=blog
# SAIL_XDEBUG_MODE=develop,debug,coverage
$ sail_init
$ sail_up
```

Open new terminal tab:

```bash
$ cd blog
sail_test
sail_web
```

Open third terminal tab if we want to process non sync queue items:

```bash
$ cd blog
sail_work
```

## Adding services

```bash
$ sail artisan sail:install
#   [0] mysql
#   [1] pgsql
#   [2] mariadb
#   [3] redis
#   [4] memcached
#   [5] meilisearch
#   [6] minio
#   [7] mailhog
#   [8] selenium
# enter all the service options needed
$ 0,3,5,6,7
```
