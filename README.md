# getopts
Friendly [`getopts`](http://en.wikipedia.org/wiki/Getopts) for [Oh My Fish!][omf].

[![MIT License](https://img.shields.io/badge/license-MIT-007EC7.svg?style=flat-square)](LICENSE)

Focus on writing your function, not on argument parsing. `getopts` does the hard work of parsing function arguments for you in a standard way.


## Usage
> `getopts <definition>... -- [ARGV...]`

`getopts` parses a series of arguments by matching each one against a list of option definitions. Each definition has the following grammar:

    <definition>  ::= <letter> <value> | <word> <value>
    <value>       ::= "" | ":" | "::"

A definition beginning with `<letter>` defines a short option, while a definition beginning with `<word>` defines a long option. A short option will match `-<letter>`, while a long option will match `--<word>`.

If a `<letter>` or `<word>` is followed by a `:`, the option is expected to have an argument, which may be supplied separately or next to the option without spaces in the same string. To indicate optional arguments, use an additional `:` character after a `:` at the end of the definition.

Both required and optional values for arguments can be supplied either in the same string as the option, or in the string following the option. For short options, the value can be appended without spaces, e.g, `-<letter>value`. For long options, use a `=` character after the option, e.g, `--<word>=value`.


## Description
`getopts` obtains options and their arguments from a list of parameters that, as indicated by each `<definition>`, are single letters preceded by a `-` or words preceded by `--` and possibly followed by an argument value.

fish `getopts` follows the specifications described in the [Utility Syntax Guidelines][utilconv]. The following is a summary of the features:

- Short options; single letters preceded by `-`, and long options; words preceded by `--`, are both supported.

- Single letters may be grouped. `-abc` → `-a -b -c`

- Options required to take an argument can specify the argument either in the same string as the option or separated from the by a space. (1) `-a argument`, (2) `-aargument`

- Options that can take an argument optionally shall specify the argument in the same string as the option argument if in short option style: `-aargument`, or separated by a `=` if in long form: `--long-form=argument`. If a blank space is used, the following argument will be treated independently.

- Options can appear multiple times in the same argument list. `getopts` will print every match sequentially on each call, and should default to the short form of the option if available.


## Examples
```fish
  function my_utility
    getopts l long x: o:: optional:: -- $argv | while read -l opt value
      switch $opt
        case -l --long
          echo handle `-l --long`
        case -x
          echo handle `-x` w/ argument `$value`
        case -o --optional
          echo handle `-o --optional` w/ optional argument `$value`
        case _
          echo operand: `$value`
      end
    end
  end
```


## Links and inspiration
- [UNIX Utility Conventions][utilconv]
- http://man7.org/linux/man-pages/man1/getopt.1.html
- https://github.com/fishery/getopts
- zparseopts: http://linux.die.net/man/1/zshmodules


## License
[MIT][mit] © [Oh My Fish!][omf]


[mit]:            http://opensource.org/licenses/MIT
[omf]:            https://www.github.com/oh-my-fish
[license-badge]:  https://img.shields.io/badge/license-MIT-007EC7.svg?style=flat-square
[utilconv]:       http://pubs.opengroup.org/onlinepubs/7908799/xbd/utilconv.html
