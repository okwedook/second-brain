# `make`
`make` is a tool for building executable files in bigger projects

# Makefiles
`Makefile` or `<name>.mk` is just a file in project directory and it specifies the instructions for [`make`](#`make`). It can run any code (the language is [[Turing-complete]]), but is mostly used to gather needed files and build an executable without recompiling unchanged files, while making the process simple and automated in most scenarios.

In case of C++ it usually builds object files from translation units and later links them into needed executables.