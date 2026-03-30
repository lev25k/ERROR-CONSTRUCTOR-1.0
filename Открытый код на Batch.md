@echo off
reg add "HKCU\Console" /v "CodePage" /t REG_DWORD /d 1251 /f >nul 2>&1
chcp 1251
color 0F 
cls
net session >nul 2>&1
if %errorlevel% neq 0 (
    title КОНСТРУКТОР ОШИБОК - Ошибка: нужны права администратора
    echo Запрос прав администратора...
    powershell start -verb runas '%0'
    exit /b
)
echo Скрипт запущен с правами администратора!
timeout /t 0 /nobreak >nul 2>nul
if not exist "C:\Windows\errors" (
    echo Папка C:\Windows\errors не найдена. Создаю...
    timeout /t 0 /nobreak >nul 2>nul
    mkdir "C:\Windows\errors"
    echo Папка успешно создана!
) else (
    timeout /t 0 /nobreak >nul 2>nul
    echo Папка C:\Windows\errors уже существует. Действий не требуется.
)
pause 
cls
:menu
mode con: cols=60 lines=25
title КОНСТРУКТОР ОШИБОК - Меню
echo.
echo 			КОНСТРУКТОР ОШИБОК
echo 		тут вы можете создать свою ошибку
echo.
timeout /t 0 /nobreak >nul 2>nul
echo ===================================
echo СОЗДАТЬ НОВУЮ ОШИБКУ
echo (нажмите W)
echo ===================================
timeout /t 0 /nobreak >nul 2>nul
echo.
echo ===================================
echo ОТКРЫТЬ СОХРАНЁННУЮ ОШИБКУ
echo (нажмите S)
echo ===================================
timeout /t 0 /nobreak >nul 2>nul
echo.
echo ===================================
echo ПОСМОТРЕТЬ СОХРАНЁННЫЕ
echo (нажмите G)
echo ===================================
timeout /t 0 /nobreak >nul 2>nul
echo.
echo ===================================
echo ВЫЙТИ ИЗ КОНСТРУКТОРА
echo (нажмите Q)
echo ===================================
timeout /t 0 /nobreak >nul 2>nul
choice /c WSGQ /n /m ""
set "result=%errorlevel%"
if "%result%"=="4" (
    cls
    goto quitanimation
) else if "%result%"=="3" (
    start C:\Windows\errors
    cls
    goto menu
) else if "%result%"=="2" (
    cls
    goto created
) else if "%result%"=="1" (
    cls
    goto errorcreate
)



:quitanimation
title КОНСТРУКТОР ОШИБОК - Выход
color 07
echo.
echo 			КОНСТРУКТОР ОШИБОК
echo 		тут вы можете создать свою ошибку
echo.
echo ===================================
echo СОЗДАТЬ НОВУЮ ОШИБКУ
echo (нажмите W)
echo ===================================
echo.
echo ===================================
echo ОТКРЫТЬ СОХРАНЁННУЮ ОШИБКУ
echo (нажмите S)
echo ===================================
echo.
echo ===================================
echo ПОСМОТРЕТЬ СОХРАНЁННЫЕ
echo (нажмите G)
echo ===================================
echo.
echo ===================================
echo ВЫЙТИ ИЗ КОНСТРУКТОРА
echo (нажмите Q)
echo ===================================
timeout /t 0 /nobreak >nul 2>nul
cls
color 08
echo.
echo 			КОНСТРУКТОР ОШИБОК
echo 		тут вы можете создать свою ошибку
echo.
echo ===================================
echo СОЗДАТЬ НОВУЮ ОШИБКУ
echo (нажмите W)
echo ===================================
echo.
echo ===================================
echo ОТКРЫТЬ СОХРАНЁННУЮ ОШИБКУ
echo (нажмите S)
echo ===================================
echo.
echo ===================================
echo ПОСМОТРЕТЬ СОХРАНЁННЫЕ
echo (нажмите G)
echo ===================================
echo.
echo ===================================
echo ВЫЙТИ ИЗ КОНСТРУКТОРА
echo (нажмите Q)
echo ===================================
timeout /t 0 /nobreak >nul 2>nul
cls
exit

:errorcreate
title КОНСТРУКТОР ОШИБОК - Создание ошибок
mode con: cols=68 lines=10
echo Вы точно хотите создать новое окно с ошибкой?
echo Создать [Y]
echo Нет, выйти в меню [N]
choice /c YN /n /m ""
set "result=%errorlevel%"
if "%result%"=="2" (
    cls
    goto menu
) else if "%result%"=="1" (
    cls
    goto errorcreatego
)
:errorcreatego
mode con: cols=80 lines=36
echo ===========================================================
set "message="
set /p "message=1. Введите текст который будет в окне (Enter для подтверждения): "
echo.
echo    Текст ошибки: %message%
pause
echo ===========================================================
set "title="
set /p "title=2. Введите текст который будет в заголовке окна (Enter для подтверждения): "
echo.
echo    Текст заголовка окна: %title%
pause
echo ===========================================================
set "logo="
set /p "logo=3. Введите код значка который будет в окне: 0 - нет значка, 16 - значок ошибки, 32 - значок вопроса, 48 - значок предупреждения, 64 - информационный значок (Enter для подтверждения): "
echo.
echo    Код значка: %logo%
pause
echo ===========================================================
set "errorname="
set /p "errorname=4. Введите название вашей ошибки (Enter для подтверждения): "
echo.
echo    Название вашей ошибки: %errorname%
pause
echo ===========================================================
echo.
cls
echo ==============================
echo Окно с вашей ошибкой готово к созданию, создать его?
echo Да [Y]
echo Нет [N]
echo ==============================
echo Проверьте, всё ли верно:
echo.
echo Текст в окне ошибки: %message%
echo Заголовок окна ошибки: %title%
echo Код значка: %logo%
echo Название этой ошибки: %errorname%
choice /c YN /n /m ""
set "result=%errorlevel%"
if "%result%"=="2" (
    cls
    goto errorcreate
) else if "%result%"=="1" (
    cls
    goto windowcreated
)
:windowcreated
cd/
cd Windows
cd errors
echo MsgBox "%message%", %logo%, "%title%" > %errorname%.vbs
echo Файл %errorname%.vbs создан! Открыть?
echo Открыть [Y]
echo Создать новую ошибку [N]
choice /c YN /n /m ""
set "result=%errorlevel%"
if "%result%"=="2" (
    cls
    goto errorcreate
) else if "%result%"=="1" (
    cd\
    cd Windows
    cd errors
    start %errorname%.vbs
)
pause 
goto errorcreate
:created
cd\
cd Windows
cd errors
dir
echo ===============================================
set "filename="
set /p "filename=Введите название вашей ошибки которую хотите открыть и нажмите Enter: "
echo Открытие ошибки %filename%.vbs...
start C:\Windows\errors\%filename%.vbs
goto menu