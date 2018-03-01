# Visual Studio HOWTO

## Macros

```c++
_WIN32
_MSC_VER // see https://stackoverflow.com/questions/70013/how-to-detect-if-im-compiling-code-with-visual-studio-2008
```

## Common Shortcuts

Purpose  |  Shortcut
---------|----------
Bring up IDE Navigator <br> to show a list of all windows | `CTRL+TAB`
To Class View | `CTRL+SHIFT+C`
To Solution Explorer | `CTRL+ALT+L`
Select current doc in Solution Explorer | `CTRL+[+S`
From Solution Explorer to editor| `ESC`

## Lib/DLL

You can use `dumpbin` utility with /headers option

It returns whether the library was built for 32 or 64 bit architecture.

Check here for details.

Example usage:

`c:\>dumpbin libXYZ.lib /headers`
