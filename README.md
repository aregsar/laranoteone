## Create new project

Create the application

```bash
$ alias sail="vendor/bin/sail"
# create a new empty repo on github called blog
# run docker desktop
$ sail_new blog
# type password
# update .env and .env.example file settings to
DB_DATABASE=blog
SAIL_XDEBUG_MODE=develop,debug,coverage
# $ echo "SAIL_XDEBUG_MODE=develop,debug,coverage" >> .env
# $ echo "SAIL_XDEBUG_MODE=develop,debug,coverage" >> .env.example

$ sail_init
$ git_local_init
$ git_remote_init
```

Start the application containers

```bash
$ sail_up
```

## The output

```bash
aregsarkissian@Aregs-MacBook-Pro laranoteone % sail up -d
not found
[+] Running 10/10
 ⠿ Network laranoteone_sail               Cr...                            0.0s
 ⠿ Volume "laranoteone_sail-mysql"        Created                          0.0s
 ⠿ Volume "laranoteone_sail-redis"        Created                          0.0s
 ⠿ Volume "laranoteone_sail-meilisearch"  Created                          0.0s
 ⠿ Container laranoteone_selenium_1       Started                          1.2s
 ⠿ Container laranoteone_mysql_1          Started                          1.3s
 ⠿ Container laranoteone_redis_1          Started                          1.2s
 ⠿ Container laranoteone_mailpit_1        Started                          1.2s
 ⠿ Container laranoteone_meilisearch_1    Started                          1.0s
 ⠿ Container laranoteone_laravel.test_1   Started                          2.1s

aregsarkissian@Aregs-MacBook-Pro laranoteone % sail npm install

added 20 packages, and audited 21 packages in 8s

5 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
aregsarkissian@Aregs-MacBook-Pro laranoteone % sail composer install
Installing dependencies from lock file (including require-dev)
Verifying lock file contents can be installed on current platform.
Nothing to install, update or remove
Generating optimized autoload files
> Illuminate\Foundation\ComposerScripts::postAutoloadDump
> @php artisan package:discover --ansi

   INFO  Discovering packages.

  laravel/sail ................................................................................................................................ DONE
  laravel/sanctum ............................................................................................................................. DONE
  laravel/tinker .............................................................................................................................. DONE
  nesbot/carbon ............................................................................................................................... DONE
  nunomaduro/collision ........................................................................................................................ DONE
  nunomaduro/termwind ......................................................................................................................... DONE
  spatie/laravel-ignition ..................................................................................................................... DONE

82 packages you are using are looking for funding.
Use the `composer fund` command to find out more!
aregsarkissian@Aregs-MacBook-Pro laranoteone % sail artisan sail:publish

   INFO  Publishing [sail-docker] assets.

  Copying directory [vendor/laravel/sail/runtimes] to [docker] ................................................................................ DONE

aregsarkissian@Aregs-MacBook-Pro laranoteone % sail artisan migrate

   INFO  Preparing database.

  Creating migration table ............................................................................................................... 28ms DONE

   INFO  Running migrations.

  2014_10_12_000000_create_users_table ................................................................................................... 44ms DONE
  2014_10_12_100000_create_password_reset_tokens_table ................................................................................... 54ms DONE
  2019_08_19_000000_create_failed_jobs_table ............................................................................................. 40ms DONE
  2019_12_14_000001_create_personal_access_tokens_table .................................................................................. 65ms DONE

aregsarkissian@Aregs-MacBook-Pro laranoteone % sail test

   PASS  Tests\Unit\ExampleTest
  ✓ that true is true                                                                                                                                                          0.02s

   PASS  Tests\Feature\ExampleTest
  ✓ the application returns a successful response                                                                                                                              0.71s

  Tests:    2 passed (2 assertions)
  Duration: 1.01s

aregsarkissian@Aregs-MacBook-Pro laranoteone % sail down
[+] Running 7/7
 ⠿ Container laranoteone_laravel.test_1  Removed                                                                                                                                 0.6s
 ⠿ Container laranoteone_mysql_1         Removed                                                                                                                                 1.2s
 ⠿ Container laranoteone_redis_1         Removed                                                                                                                                 0.5s
 ⠿ Container laranoteone_mailpit_1       Removed                                                                                                                                 0.5s
 ⠿ Container laranoteone_meilisearch_1   Removed                                                                                                                                 0.5s
 ⠿ Container laranoteone_selenium_1      Removed                                                                                                                                 4.3s
 ⠿ Network laranoteone_sail              Removed

```



```bash
aregsarkissian@Aregs-MacBook-Pro laranoteone % sail up -d && sail npm run dev
not found
[+] Running 7/7
 ⠿ Network laranoteone_sail              Created                                                                                                                                 0.0s
 ⠿ Container laranoteone_mailpit_1       Started                                                                                                                                 1.1s
 ⠿ Container laranoteone_selenium_1      Started                                                                                                                                 1.0s
 ⠿ Container laranoteone_redis_1         Started                                                                                                                                 1.0s
 ⠿ Container laranoteone_meilisearch_1   Started                                                                                                                                 1.2s
 ⠿ Container laranoteone_mysql_1         Started                                                                                                                                 1.2s
 ⠿ Container laranoteone_laravel.test_1  Started                                                                                                                                 2.7s

> dev
> vite


  VITE v4.4.4  ready in 773 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: http://172.19.0.7:5173/
  ➜  press h to show help

  LARAVEL v10.14.1  plugin v0.7.8

  ➜  APP_URL: http://localhost

```

## Inspect the application

Open a new terminal tab

```bash
$ cd blog
# run application tests
$ sail_test
# open http://localhost
$ sail_web
```


## Process background jobs

if we want to process non sync queue items:

Change the queue type from sync to redis in the .env file

Open a new terminal tab

```bash
cd blog
sail_queue_listen
```

## Adding other sail services

```bash
$ sail_add
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



## The bash functions

```bash

function git_local_init {
    git init
    git add -A
    git commit -m "first commit"
}

function git_remote_init {
    #todo check for existance of .git directory
    reponame=${PWD##*/}
    accountname=aregsar
    if ! [ $# -eq 0 ]
    then
        accountname=$1
    fi
    git remote add origin git@github.com:$accountname/$reponame.git
    git push -u origin main
}

function  sail_new {
    if [ $# -eq 1 ]
    then
        reponame=$1
        curl -s "https://laravel.build/$reponame" | bash
        cd $reponame
        pwd
    else
        echo "needs exactly one argument"
    fi
}

function sail_init() {
    sail up -d
    sail npm install
    sail composer install
    sail artisan sail:publish
    sail artisan migrate
    sail test
    sail down
}

alias sail_up="sail up -d && sail npm run dev"

alias sail_down="sail down"

alias sail_test="sail test"

alias sail_web="open http://localhost"

alias sail_queue_listen="sail artisan queue:listen"
alias sail_add="sail artisan sail:add"


alias sail_migrate="sail artisan migrate:fresh --seed"
alias sail_routes="sail artisan route:list"
alias sail_routesc="sail artisan route:list --compact "
```
