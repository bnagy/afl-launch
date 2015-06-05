afl-launch
=======

## About

`afl-launch` is a simple program to spawn afl fuzzing instances from the
command line. It provides no compelling features; it is simply my version of
this tool.

```
Usage of ./afl-launch:
  -i="": afl-fuzz -i option (input location)
  -m=-1: afl-fuzz -m option (memory limit)
  -n=1: Number of instances to launch
  -name="": Base name for instances. Names will be <BASE>-[M|S]-<N>
  -no-master=false: Launch all instances with -S
  -o="": afl-fuzz -o option (output location)
  -t=-1: afl-fuzz -t option (timeout)
  -x="": afl-fuzz -x option (extras location)
```

The launcher DOES NOT CHECK if the `afl-fuzz` instance errored out. You should
start afl-fuzz manually with your desired `-i` `-o` `-x` (etc) options to make
sure everything works.

If you don't supply a base name, the launcher will pick a random one.

Example:
```
./afl-launch -i ~/testcases/pdf -o ~/fuzzing/pdf -n 4  -- pdftoppm @@
```

## Installation

You should follow the [instructions](https://golang.org/doc/install) to
install Go, if you haven't already done so.

Download, build and install `afl-launch`:
```bash
$ go get -u github.com/bnagy/afl-launch
```

## TODO

## Contributing

Fork and send a pull request.

Report issues.

## License & Acknowledgements

BSD style, see LICENSE file for details.

