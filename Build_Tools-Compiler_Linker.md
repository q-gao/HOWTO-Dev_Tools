# Compiler

# Linker

[Linux Static and Dynamic Lib Tutorial](http://www.yolinux.com/TUTORIALS/LibraryArchives-StaticAndDynamic.html): very easy to follow
> The fix is to resolve dependencies of (*.so not found)

## `ldconfig`

### Find out if library is in path

https://unix.stackexchange.com/questions/282199/find-out-if-library-is-in-path

```sh
# crawl all the usual library paths, list all the available libraries and reconstruct the cache(require root)
ldconfig

# crawl all the usual library paths, list all the available libraries, without reconstructing the cache
/sbin/ldconfig -v -N

# To take into account libraries in LD_LIBRARY_PATH, you need pass additional libraries to the command line like:
/sbin/ldconfig -N -v $(sed 's/:/ /' <<< $LD_LIBRARY_PATH)

```
