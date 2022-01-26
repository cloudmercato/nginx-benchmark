nginx-benchmark
===============

Lua site allowing custom HTTP benchmark

Install
=======

::

  apt install -y nginx libnginx-mod-http-lua

  cp benchmark.conf /path/to/sites/benchmark
  # rm /path/to/sites/default
  cp benchmark.html /var/www/benchmark.html

  service nginx restart

Start
=====

Go to http://<ip_or_hostname>

Made with ❤️ by `Cloud Mercato <https://cloud-mercato.com>`_
