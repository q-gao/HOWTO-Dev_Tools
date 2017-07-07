# DOS Batch

```dos
@echo off
REM  PushMasterJsonToProjects.bat [<iTJsonScriptPath> [<json_file_base_name>]]
rem get argument number
set argNum=0
for %%x in (%*) do Set /A argNum+=1

if %argNum% GTR 0 (
	set iTJsonPath= %1
 else (
    set iTJsonPath=iTJsonPath
)
if %argNum% GTR 1 (
	set jsonBaseName=%2
) else (
	set jsonBaseName=Algorithm.json
)

rem bat file SUCKS!! the loop variable name MUST have one character
for %%p in (Essential-PM1-8998-SLPI-JDI-JDI-BU21150-v1
			Essential-PM1-8998-SLPI-Sharp-Sharp-QTC800S-v1
			) do (
	@echo python %iTJsonPath%\iTJsonSync.py Master\%jsonBaseName% %%p\%jsonBaseName%	
	python %iTJsonPath%\iTJsonSync.py Master\%jsonBaseName% %%p\%jsonBaseName%	
)
```
