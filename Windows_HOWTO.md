# DOS Batch

```dos
@echo off
REM  PushToProjects.bat [<JsonPath> [<json_file_base_name>]]
rem get argument number
set argNum=0
for %%x in (%*) do Set /A argNum+=1

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
