# Compiler

# Linker

## `ldconfig`

### Find out if library is in path

https://unix.stackexchange.com/questions/282199/find-out-if-library-is-in-path

```sh
# crawl all the usual library paths, list all the available libraries and reconstruct the cache(require root)
ldconfig

# crawl all the usual library paths, list all the available libraries, without reconstructing the cache

# To take into account libraries in LD_LIBRARY_PATH, you need pass additional libraries to the command line like:
`/sbin/ldconfig -v -N`

/sbin/ldconfig -N -v $(sed 's/:/ /' <<< $LD_LIBRARY_PATH)

```
