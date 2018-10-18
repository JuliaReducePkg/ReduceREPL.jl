<p align="center">
  <img src="./docs/src/assets/logo.png" alt="Reduce.jl"/>
</p>

*Note* this is a minimzed version of the package Reduce.jl to only provide a REPL interface

# ReduceREPL.jl

*Symbolic parser generator for Julia language expressions using REDUCE algebra term rewriter*

[![Build Status](https://travis-ci.org/chakravala/Reduce.jl.svg?branch=master)](https://travis-ci.org/chakravala/Reduce.jl)
[![Build status](https://ci.appveyor.com/api/projects/status/kaqu2yri4vxyr63n?svg=true)](https://ci.appveyor.com/project/chakravala/reduce-jl)
[![Coverage Status](https://coveralls.io/repos/github/chakravala/Reduce.jl/badge.svg?branch=master)](https://coveralls.io/github/chakravala/Reduce.jl?branch=master)
[![codecov.io](http://codecov.io/github/chakravala/Reduce.jl/coverage.svg?branch=master)](http://codecov.io/github/chakravala/Reduce.jl?branch=master)
[![Docs Stable](https://img.shields.io/badge/docs-stable-blue.svg)](https://chakravala.github.io/Reduce.jl/stable)
[![Docs Latest](https://img.shields.io/badge/docs-latest-blue.svg)](https://chakravala.github.io/Reduce.jl/latest)
[![Join the chat at gitter](https://badges.gitter.im/Reduce-jl/Lobby.svg)](https://gitter.im/Reduce-jl/Lobby?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

The premise behind Reduce.jl is based on the idea that `Symbol` and `Expr` types can be translated into computer algebra rewrite commands and then automatically parsed back into Julia ASTs, essentially extending the Julia language into a fully programable symbolic AST rewrite environment.

REDUCE is a system for general algebraic computations of interest to mathematicians, scientists and engineers:

* exact arithmetic using integers and fractions; arbitrary precision numerical approximation;
* polynomial and rational function algebra; factorization and expansion of polynomials and rational functions;
* differentiation and integration of multi-variable functions; exponential, logarithmic, trigonometric and hyperbolic;
* output of results in a variety of formats; automatic and user controlled simplification of expressions;
* substitutions and pattern matching of expressions; quantifier elimination and decision for interpreted first-order logic;
* solution of ordinary differential equations; calculations with a wide variety of special (higher transcendental) functions;
* calculations involving matrices with numerical and symbolic elements; general matrix and non-commutative algebra;
* powerful intuitive user-level programming language; generating optimized numerical programs from symbolic input;
* Dirac matrix calculations of interest to high energy physicists; solution of single and simultaneous equations.

Interface for applying symbolic manipulation on [Julia expressions](https://docs.julialang.org/en/latest/manual/metaprogramming) using [REDUCE](http://www.reduce-algebra.com)'s term rewrite system:

* reduce expressions are `RExpr` objects that can `parse` into julia `Expr` objects and vice versa;
* interface link communicates and interprets via various reduce output modes using `rcall` method;
* interactive `reduce>` REPL within the Julia terminal window activated by `}` key;

Additional packages that depend on Reduce.jl are maintained at [JuliaReducePkg](https://github.com/JuliaReducePkg).

The upstream REDUCE software created by Anthony C. Hearn is maintained by collaborators on [SourceForge](https://sourceforge.net/p/reduce-algebra/).

## Setup

The `Reduce` package provides the base functionality to work with Julia and Reduce expressions, provided that you have `redcsl` in your path. On GNU/Linux/OSX/Windows, `Pkg.build("Reduce")` will automatically download a precompiled binary for you. If you are running a different Unix operating system, the build script will download the source and attempt to compile `redcsl` for you, success depends on the build tools installed. Automated testing for **Travis CI** and **appveyor** using Linux, OSX, and Windows are fully operational `using Reduce`.

```Julia
julia> Pkg.add("Reduce"); Pkg.build("Reduce")
julia> using Reduce
Reduce (Free CSL version, revision 4521),  11-March-2018 ...
```
For linux users who wish to speed up frequent precompilation, it is possible to disable extra precompilation scripts by setting the environment variable `ENV["REDPRE"] = "0"` in julia (only effective when `Reduce` is being compiled).

View the documentation [stable](https://chakravala.github.io/Reduce.jl/stable) / [latest](https://chakravala.github.io/Reduce.jl/latest) for more features and examples.

## Usage

Reduce expressions encapsulated into `RExpr` objects can be manipulated within julia using the standard syntax. Create an expression object either using the `RExpr("expression")` string constructor or `R"expression"`. Additionally, arbitrary julia expressions can also be parsed directly using the `RExpr(expr)` constructor. Internally `RExpr` objects are represented as an array that can be accessed by calling `*.str[n]` on the object.

The output of `rcall` will be the same as its input type.
```Julia
julia> "int(sin(y)^2, y)" |> rcall
"( - cos(y)*sin(y) + y)/2"
```

### Output mode
Various output modes are supported. While in the REPL, the default `nat` output mode will be displayed for `RExpr` objects.
This same output can also be printed to the screen by calling `print(nat(r))` method.

### REPL interface
Similar to <kbd>?</kbd> help and <kbd>;</kbd> shell modes in Julia, `Reduce` provides a `reduce>` REPL mode by pressing <kbd>shift</kbd>+<kbd>]</kbd> as the first character in the julia terminal prompt. The output is in `nat` mode.
```Julia
reduce> df(atan(golden_ratio*x),x);

          2              2
 sqrt(5)*x  + sqrt(5) - x  + 1
-------------------------------
           4      2
       2*(x  + 3*x  + 1)
```

## Background

The `Reduce` package currently provides a robust interface to directly use the CSL version of REDUCE within the Julia language and the REPL. This is achieved by interfacing the abstract syntax tree of `Expr` objects with the parser generator for `RExpr` objects and then using an `IOBuffer` to communicate with `redpsl`.

> REDUCE is a system for doing scalar, vector and matrix algebra by computer, which also supports arbitrary precision numerical approximation and interfaces to gnuplot to provide graphics. It can be used interactively for simple calculations but also provides a full programming language, with a syntax similar to other modern programming languages.
> REDUCE has a long and distinguished place in the history of computer algebra systems. Other systems that address some of the same issues but sometimes with rather different emphasis are Axiom, Macsyma (Maxima), Maple and Mathematica.
> REDUCE is implemented in Lisp (as are Axiom and Macsyma), but this is completely hidden from the casual user. REDUCE primarily runs on either Portable Standard Lisp (PSL) or Codemist Standard Lisp (CSL), both of which are included in the SourceForge distribution. PSL is long-established and compiles to machine code, whereas CSL is newer and compiles to byte code. Hence, PSL may be faster but CSL may be available on a wider range of platforms.

Releases of `Reduce.jl` enable the general application of various REDUCE functionality and packages to manipulate the Julia language to simplify and compute new program expressions at run-time. Intended for uses where a symbolic pre-computation is required for numerical algorithm code generation.

> Julia is a high-level, high-performance dynamic programming language for numerical computing. It provides a sophisticated compiler, distributed parallel execution, numerical accuracy, and an extensive mathematical function library. Julia’s Base library, largely written in Julia itself, also integrates mature, best-of-breed open source C and Fortran libraries for linear algebra, random number generation, signal processing, and string processing.
> The strongest legacy of Lisp in the Julia language is its metaprogramming support. Like Lisp, Julia represents its own code as a data structure of the language itself. Since code is represented by objects that can be created and manipulated from within the language, it is possible for a program to transform and generate its own code. This allows sophisticated code generation without extra build steps, and also allows true Lisp-style macros operating at the level of abstract syntax trees.

## Troubleshooting

If the `reduce>` REPL is not appearing when `}` is pressed or the Reduce pipe is broken, the session can be restored by simply calling `Reduce.Reset()`, without requiring a restart of `julia` or reloading the package. This kills the currently running Reduce session and then re-initializes it for new use.

Otherwise, questions can be asked on gitter/discourse or submit your issue or pull-request if you require additional features or noticed some unusual edge-case behavior.

### OhMyREPL Compatibility

Reduce.jl is compatible with the [OhMyREPL.jl](https://github.com/KristofferC/OhMyREPL.jl) package.

Place `using Reduce` as first package to load in the `~/.juliarc.jl` startup file to ensure the REPL loads properly (when also `using OhMyREPL`). Otherwise, if you are loading this package when Julia has already been started, load it after `OhMyREPL`.
