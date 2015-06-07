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

The launcher DOES NOT CHECK if the `afl-fuzz` instance errored out. Before
starting a multiple launch, you should start `afl-fuzz` once manually with your
desired `-i` `-o` `-x` (etc) options to make sure everything works.

If you don't supply a base name, the launcher will pick a random one.

Example:
```
./afl-launch -i ~/testcases/pdf -o ~/fuzzing/pdf -n 4  -- pdftoppm @@
```

A note on the `-f` flag - the idea is that you pass a template like
`/dev/shm/whatever.xml` and the launcher will substitute it as `-f
/dev/shm/<BASENAME>-S12.xml` when it invokes `afl-fuzz`. This is so that you can
have AFL create testcase files on a ramdisk, and avoid stressing your disks.
Queue entries and crashes are still saved as usual in the location specified
by `-o`. Don't be an idiot like me and run everything on a ramdisk.

Another note about ttys - this tool just spawns all the processes and then
exits. If you want them to stay running unattended then the easiest and (IMHO)
best way is just to run it inside a `screen` session (`man screen`).

### They launched.. now what?

Use `afl-whatsup <LOCATION>` with the same location you used for -o to get the
afl-fuzz summary output. For bonus points, be a unix nerd and do like `watch
-t -n 60 "afl-whatsup ~/fuzzing/targetname | tail -n 11"`

This is what that looks like:
```
Every 60.0s: afl-whatsup ~/fuzzing/targetname | tail -n 11     Sun Jun  7 10:40:36 2015

Summary stats
=============

       Fuzzers alive : 48
      Total run time : 71 days, 9 hours
         Total execs : 641 million
    Cumulative speed : 4993 execs/sec
       Pending paths : 250 faves, 133096 total
  Pending per fuzzer : 5 faves, 2772 total (on average)
       Crashes found : 5597 locally unique
```

## Installation

You should follow the [instructions](https://golang.org/doc/install) to
install Go, if you haven't already done so.

Download, build and install `afl-launch`:
```bash
$ go get -u github.com/bnagy/afl-launch
```

## TODO

Nothing on the list. Open an issue if you want something.

## Contributing

* Fork and send a pull request
* Report issues

## License & Acknowledgements

BSD style, see LICENSE file for details.

