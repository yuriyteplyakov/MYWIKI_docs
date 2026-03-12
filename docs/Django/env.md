
# Создание виртуального окружения

Чтобы отрабатывали локальные скрипты, нужно:
От имени администратора в powershell

> Set-ExecutionPolicy Restricted
> Подтвердить: Yes

- Создание окружения (создастся в папке которой находимся)
`python -m venv venv`

> cd C:\...\Nazvanie_OkruzheniaEnv\Scripts

- Активация окружения
> .\activate.ps1

- либо лучше
> venv\Scripts\activate

- Выход из окружения
> deactivate

Поиск всех виртуальных окружений на компьютере в PS от имени админа
Get-ChildItem -Path C:\ -Directory -Filter "*venv*" -Recurse -ErrorAction SilentlyContinue | Select-Object FullName
Get-ChildItem -Path C:\ -Directory -Filter "*env*" -Recurse -ErrorAction SilentlyContinue | Where-Object {$_.Name -ne "Windows"} | Select-Object FullName

CMD
# Очистка временных файлов Python
del /q /f /s %TEMP%\pip-* >nul 2>&1
del /q /f /s %TEMP%\virtualenv-* >nul 2>&1
del /q /f /s %TEMP%\venv-* >nul 2>&1

# Очистка кэша pip
rmdir /s /q %LocalAppData%\pip\cache >nul 2>&1