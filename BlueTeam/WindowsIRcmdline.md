### Extract Strings from a File:

batch

@echo off
setlocal EnableDelayedExpansion

set "file_path=C:\Path\To\File.exe"

echo Strings extracted from %file_path%:
findstr /R "^" "%file_path%"

endlocal

### Calculate File Hashes:

batch

@echo off

set "file_path=C:\Path\To\File.exe"

echo Calculating MD5 hash:
certutil -hashfile "%file_path%" MD5
echo.
echo Calculating SHA1 hash:
certutil -hashfile "%file_path%" SHA1
echo.
echo Calculating SHA256 hash:
certutil -hashfile "%file_path%" SHA256

### Analyze PE Headers:

batch

@echo off

set "file_path=C:\Path\To\File.exe"

echo PE Header information for %file_path%:
dumpbin /headers "%file_path%"

View Digital Signatures:

batch

@echo off

set "file_path=C:\Path\To\File.exe"

echo Digital signatures for %file_path%:
signtool verify /pa /v "%file_path%"

### Monitor Process Activity:

batch

@echo off

echo Monitoring process activity...
timeout /t 5 >nul
tasklist /v

### Check Windows Event Logs:

batch

@echo off

echo Checking Windows Event Logs...
timeout /t 5 >nul
wevtutil qe Security /rd:true /f:text

### Scan with Windows Defender:

batch

@echo off

set "file_path=C:\Path\To\File.exe"

echo Scanning %file_path% with Windows Defender...
timeout /t 5 >nul
"%ProgramFiles%\Windows Defender\MpCmdRun.exe" -Scan -ScanType 3 -File "%file_path%"

### Analyze Registry Changes:

batch

@echo off

echo Monitoring registry changes...
timeout /t 5 >nul
reg query HKLM\Software
