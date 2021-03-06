[![Build Status](https://cloud.drone.io/api/badges/xran-deex/ats-http/status.svg)](https://cloud.drone.io/xran-deex/ats-http)

# ats-http

## Quick start (Docker)
```bash
docker run -it --rm -v $PWD:/code -p 8888:8888 \
-e "STATICLIB=1" -e "OUTDIR=/code/.libs" xrandeex/ats2-libz:0.4.0 \
make -C /code runtest
```

## Dependencies
Until optional dependencies are working, you will need libz installed.
```bash
sudo apt install zlib1g-dev
```
To run the examples, you will need sqlite3 as well.
```bash
sudo apt install libsqlite3-dev
```

## Example
``` ats
#include "../ats-http.hats"
staload "libats/libc/SATS/string.sats"

staload $REQ
staload $RESP

implement main(argc, argv) = 0 where {
    // make the server
    var server = make_server(8888)
    // use 6 threads
    val () = set_thread_count(server, 6)

    // setup a get response handler
    val () = get(server, "/hello", lam (req,resp) =<cloptr1> copy("Hello World") where {
        val () = set_status_code(resp, 200)
        val () = set_content_type(resp, "text/plain")
    })

    // run the server
    val () = run_server(server)
    val () = free_server(server)
}
```

## Build
Build a shared library
``` bash
make
```
Build a static library
``` bash
STATICLIB=1 make
```

Run the test app
```bash
./tests/target/tests

or

STATICLIB=1 make runtest
```