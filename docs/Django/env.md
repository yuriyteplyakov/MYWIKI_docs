
# Создание виртуального окружения

Чтобы отрабатывали локальные скрипты, нужно:
От имени администратора в powershell

> Set-ExecutionPolicy Restricted
> Подтвердить: Yes

- Создание окружения (создастся в папке которой находимся)
`python -m venv Nazvanie_OkruzheniaEnv`

> cd C:\...\Nazvanie_OkruzheniaEnv\Scripts

- Активация окружения
> .\activate.ps1

- Выход из окружения
> deactivate