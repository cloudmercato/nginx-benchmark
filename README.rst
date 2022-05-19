nginx-benchmark
===============

Lua site allowing custom HTTP benchmark

Motivation
==========

While many HTTP client benchmark tools exist, none of them really express the capacity of a HTTP server to hold a load.

Two options are mainly used:

1. Set various files and served as an approximation of what could be the traffic
2. Use a backend application to produce whatever it's possible

The solution #1 suffers from the volume then File System behavor. It could beneficial or not but it's not linked to nginx itself.

The solution #2 comes with its batch of biais created by all the step from the application handler, network to programming language.

We make to the choice to create a simple nginx site allowing to test the server without backend. Here's the main guidelines:

- The operations should measure the capacity from a server to handle requests
- We use only the integrated Lua as programming language
- We do not depend on anything other than Lua and the OS

Install
=======

The following installation instructions cover installation on Ubuntu.
It would likely be similar under other Debian variants.

::

  apt-get update -y
  apt install -y nginx libnginx-mod-http-lua

  cp benchmark.conf /etc/nginx/sites-enabled/
  rm /etc/nginx/sites-enabled/default
  cp benchmark.html /var/www/benchmark.html

  service nginx restart
  
You can also install `nullfs <https://github.com/abbbi/nullfsvfs>`_ to remove the overhead from (future) upload tests.

Start
=====

Go to `http://<ip_or_hostname>` to consult endpoints via your browser.

Or test them with you usual HTTP benchmark tools like: ::

  # ab -c 128 -n 10000 http://myserver/urandom?size=2048
  This is ApacheBench, Version 2.3 <$Revision: 1807734 $>
  Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
  Licensed to The Apache Software Foundation, http://www.apache.org/

  Benchmarking myserver (be patient)
  Completed 1000 requests
  Completed 2000 requests
  Completed 3000 requests
  Completed 4000 requests
  Completed 5000 requests
  Completed 6000 requests
  Completed 7000 requests
  Completed 8000 requests
  Completed 9000 requests
  Completed 10000 requests
  Finished 10000 requests


  Server Software:        nginx/1.14.0
  Server Hostname:        myserver
  Server Port:            80

  Document Path:          /urandom?size=2048
  Document Length:        22 bytes

  Concurrency Level:      128
  Time taken for tests:   0.929 seconds
  Complete requests:      10000
  Failed requests:        0
  Total transferred:      1790000 bytes
  HTML transferred:       220000 bytes
  Requests per second:    10761.34 [#/sec] (mean)
  Time per request:       11.894 [ms] (mean)
  Time per request:       0.093 [ms] (mean, across all concurrent requests)
  Transfer rate:          1881.13 [Kbytes/sec] received

  Connection Times (ms)
                min  mean[+/-sd] median   max
  Connect:        0    1   0.2      1       5
  Processing:     5   11   0.8     11      19
  Waiting:        4   11   0.9     11      19
  Total:          7   12   0.7     12      20

  Percentage of the requests served within a certain time (ms)
    50%     12
    66%     12
    75%     12
    80%     12
    90%     12
    95%     13
    98%     13
    99%     14
   100%     20 (longest request)
   
Tips
====

The default Nginx configuration may not produce the best performance, so we advise you to the results nginx-benchmark produce and the reality of your application. In order to help you reaching the best level of performance here's some tips:

- In Nginx configuration:

  - `worker_connections <http://nginx.org/en/docs/ngx_core_module.html#worker_connections>`_: number of simultaneous connections that can be opened by a worker process.
  - `worker_processes <http://nginx.org/en/docs/ngx_core_module.html#worker_processes>`_: number of worker processes.
  - `worker_rlimit_nofile <http://nginx.org/en/docs/ngx_core_module.html#worker_rlimit_nofile>`_: maximum number of open files (RLIMIT_NOFILE) for worker processes.
  
- In your Linux kernel:

  - net.core.somaxconn


Made with ❤️ by `Cloud Mercato <https://cloud-mercato.com>`_
