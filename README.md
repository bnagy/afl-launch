afl-launch
=======

## About

`afl-launch` is a simple program to spawn afl fuzzing instances from the
command line. It provides no compelling features; it is simply my version of
this tool.

```
Usage of ./afl-launch:
  -f="": Filename template (substituted and passed via -f)
  -i="": afl-fuzz -i option (input location)
  -m=-1: afl-fuzz -m option (memory limit)
  -n=1: Number of instances to launch
  -name="": Base name for instances. Fuzzers will work in <output>/<BASE>-[M|S]<N>
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

A note on the `-f` flag - the idea is that you pass a template like
/dev/shm/whatever.xml and the launcher will substitute it as `-f
/dev/shm/<BASENAME>-S12.xml` when it invokes afl-fuzz. This is so that you can
have AFL create testcase files on a ramdisk, and avoid stressing your disks.
Queue entries that exercise new paths are still saved as usual in the location
specified by `-o`.

### They launched.. now what?

Use `afl-whatsup <LOCATION>` with the same location you used for -o to get the afl-fuzz summary output. For bonus points, be a unix nerd and do like `watch -t -n 60 afl-whatsup ~/fuzzing/targetname`

This is what it will look like right at the start:
```
status check tool for afl-fuzz by <lcamtuf@google.com>

Individual fuzzers
==================

>>> qwyaq-M0 (0 days, 0 hrs) <<<

  cycle 1, lifetime speed 0 execs/sec, path 0/1 (0%)
  pending 1/1, coverage 3.36%, no crashes yet

>>> qwyaq-S1 (0 days, 0 hrs) <<<

  cycle 1, lifetime speed 0 execs/sec, path 0/1 (0%)
  pending 1/1, coverage 3.36%, no crashes yet

>>> qwyaq-S2 (0 days, 0 hrs) <<<

  cycle 1, lifetime speed 0 execs/sec, path 0/1 (0%)
  pending 1/1, coverage 3.36%, no crashes yet

>>> qwyaq-S3 (0 days, 0 hrs) <<<

  cycle 1, lifetime speed 0 execs/sec, path 0/1 (0%)
  pending 1/1, coverage 3.36%, no crashes yet


Summary stats
=============

       Fuzzers alive : 4
      Dead or remote : 37 (excluded from stats)
      Total run time : 0 days, 0 hours
         Total execs : 0 million
    Cumulative speed : 0 execs/sec
       Pending paths : 4 faves, 4 total
  Pending per fuzzer : 1 faves, 1 total (on average)
       Crashes found : 0 locally unique
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

