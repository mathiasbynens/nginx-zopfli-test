# Nginx Zopfli test

This repository contains some files that make it easy to test whether [Nginx](http://nginx.org/) is correctly serving [Zopfli](https://code.google.com/p/zopfli/)-pre-compressed files using [the gzip_static module](http://wiki.nginx.org/HttpGzipStaticModule).

## Instructions

Move the `gzip-test` folder to your Nginx document root (`/usr/local/var/www` by default).

Then, run Nginx with `gzip off; gzip_static on` in the `nginx.conf`. You can use the provided `nginx.conf` file for this:

```bash
nginx -c "$(pwd)/nginx.conf"
```

Then, test the various files:

```bash
$ curl -s -H "Accept-Encoding: gzip,deflate" "http://localhost:8080/gzip-test/no-compression.js" | wc -c
240196
$ curl -s -H "Accept-Encoding: gzip,deflate" "http://localhost:8080/gzip-test/gzip.js" | wc -c
71100
$ curl -s -H "Accept-Encoding: gzip,deflate" "http://localhost:8080/gzip-test/zopfli.js" | wc -c
67379
```

If those are the numbers you’re getting, Nginx’s gzip_static module is serving the Zopflinated file correctly. For easy comparison, here are the real file sizes:

```
$ ls -lsa
total 1696
  0 drwxr-xr-x  7 Mathias  wheel     238 May  8 22:31 .
  0 drwxr-xr-x  7 Mathias  wheel     238 May  8 22:25 ..
472 -rw-r--r--@ 1 Mathias  wheel  240196 May  8 22:30 gzip.js
144 -rw-r--r--  1 Mathias  wheel   71100 May  8 22:30 gzip.js.gz
472 -rw-r--r--@ 1 Mathias  wheel  240196 May  8 22:30 no-compression.js
472 -rw-r--r--@ 1 Mathias  wheel  240196 May  8 22:30 zopfli.js
136 -rw-r--r--  1 Mathias  wheel   67379 May  8 22:30 zopfli.js.gz
```
