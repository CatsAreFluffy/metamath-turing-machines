# What is NQL?

Not-Quite-Laconic is a language and compiler for generating Turing machines with small state counts.
It is directly inspired by [Adam Yedidia's Laconic](https://github.com/adamyedidia/parsimony),
and the language is similar, but the implementation is less so.
Laconic and the programme of constructing small Turing machines to establish upper bounds on provable BB(k) values are [discussed on Scott Aaronson's blog](http://www.scottaaronson.com/blog/?p=2725).

NQL uses a different compilation methodology based on register machines and constructing binary decision diagrams to compress large programs without an explicit call stack.
It achieves "a few" times smaller state counts than Laconic for the range of programs relevant to the BB(k) problem; e.g. 1919 states for a ZF proof enumerator.
On very small programs it cannot compete with hand-coding, and behavior on much larger programs is unknown; Laconic *may* regain the upper hand,

# Differences from Laconic

 * NQL does not support negative numbers.  The `-` operator is actually monus and clamps at zero.

 * NQL implements only procs, not funcs; in other words your procedures are copied for every distinct combination of variables passed to them.
   This is less of a problem than it appears because BDD compression can share any subtrees that don't use arguments; still, it's best to use variables consistently, or copy to globals.

 * NQL supports global variables which can be referenced from any function.

 * NQL functions cannot be recursive (in other words, the call graph must be directed acyclic).  Very deep call graphs are also somewhat problematic for the state count.

 * NQL does not have any native support for lists (for this reason, Yedidia's "friedman.lac" has not yet been ported).

 * NQL presently distinguishes between numbers and booleans, and trying to mix them will cause an assertion fault.

 * NQL doesn’t require return statements.

 * NQL has a `switch` statement, which produces more parsimonious code than long elsif chains.

 * Minor syntactic differences: `ifelse` is replaced with a Perl-style `if ... elsif ... else`; global variables do not have a specific type.

# Setup

1. Make sure `python3` is installed.  This code is tested on 3.4.3
2. Make sure the "pyparsing" package is installed.  For instance: `pip3 install pyparsing`.  You may need to bootstrap pip first.

# Running

Generate a Turing machine from NQL sources:

    python3 nqlaconic.py --print-tm zf.nql

Print intermediate call tree code (very relevant for debugging):

    python3 nqlaconic.py --print-subs zf.nql

Built-in Turing machine executor:

    python3 nqlaconic.py --run-tm squaresaresmall.nql