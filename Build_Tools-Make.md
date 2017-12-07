## Select gcc version

Multiple ways to specify gcc version to be used:
- `make` command line: `make CC=gcc-5` and `make CC=gcc-6` (you probably want to do `make clean` in between) or `make CXX=g++-5` for C++ code). 
- Config compiler in Makefile
- Alternatively, put `$HOME/bin/` early in your `$PATH` and add a symlink such as `ln -sv /usr/bin/gcc-6 $HOME/bin/gcc`
- set first priority to gcc-5 `sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-5 1`

