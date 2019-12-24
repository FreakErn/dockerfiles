# m-cpuminer-v2
Dockerized m-cpuminer-v2

Just add the necessary parameter like you would append to `./m-minerd`

`docker run -d --rm freakern/m-cpuminer-v2 -o stratum+tcp://pool.mineproject.ru:6033 -u XXXXXXXXXXXXXX`

or

`docker run -d --rm freakern/m-cpuminer-v2 -a m7m -o stratum+tcp://pool_url:pool_port -u pool_user.worker -p password -t thread_numbers`
