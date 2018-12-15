# DOS Batch

```bat
@echo off
REM  PushToProjects.bat [<JsonPath> [<json_file_base_name>]]
rem get argument number
set argNum=0
for %%x in (%*) do Set /A argNum+=1

REM see https://ss64.com/nt/if.html for operators
if %argNum% GTR 0 (
	set JsonPath= %1
 else (
    set JsonPath=JsonPath
)
if %argNum% GTR 1 (
	set jsonBaseName=%2
) else (
	set jsonBaseName=Algorithm.json
)

rem bat file SUCKS!! the loop variable name MUST have one character
for %%p in (Dir-1
			Dir-2
			) do (
	@echo python %JsonPath%\iTJsonSync.py Master\%jsonBaseName% %%p\%jsonBaseName%	
	python %JsonPath%\iTJsonSync.py Master\%jsonBaseName% %%p\%jsonBaseName%	
)
```

# Useful Batch Scripts

## timeit

Like `time` command in Linux

```bat
@echo off
@setlocal

set start=%time%

:: Runs your command
cmd /c %*

set end=%time%
set options="tokens=1-4 delims=:.,"
for /f %options% %%a in ("%start%") do set start_h=%%a&set /a start_m=100%%b %% 100&set /a start_s=100%%c %% 100&set /a start_ms=100%%d %% 100
for /f %options% %%a in ("%end%") do set end_h=%%a&set /a end_m=100%%b %% 100&set /a end_s=100%%c %% 100&set /a end_ms=100%%d %% 100

set /a hours=%end_h%-%start_h%
set /a mins=%end_m%-%start_m%
set /a secs=%end_s%-%start_s%
set /a ms=%end_ms%-%start_ms%
if %ms% lss 0 set /a secs = %secs% - 1 & set /a ms = 100%ms%
if %secs% lss 0 set /a mins = %mins% - 1 & set /a secs = 60%secs%
if %mins% lss 0 set /a hours = %hours% - 1 & set /a mins = 60%mins%
if %hours% lss 0 set /a hours = 24%hours%
if 1%ms% lss 100 set ms=0%ms%

:: Mission accomplished
set /a totalsecs = %hours%*3600 + %mins%*60 + %secs%
echo command took %hours%:%mins%:%secs%.%ms% (%totalsecs%.%ms%s total)
```

# Admin

**Check File System**

Open CMD as admin run `SFC /scannow`
