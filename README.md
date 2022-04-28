# Сборка pgModeler
Make pgModeler from source code.

## Подготовка окружения
Операционная система Windows XP Pro 64-bit.

ПО|Сайт|Установочный пакет|Путь установки
---|---|---|---
7zip 64-bit|[Посетить](https://www.7-zip.org/)|[Загрузить](https://www.7-zip.org/a/7z2107-x64.exe)|По умолчанию
MSYS2|[Посетить](https://www.msys2.org/)|[Загрузить](https://github.com/msys2/msys2-installer/releases/download/2022-03-19/msys2-x86_64-20220319.exe)|`C:\msys64`

## Сборка pgModeler
Все команды выполняем в консоли `C:\msys64\msys2_shell.cmd -mingw64`
### Обновляем систему и устанавливаем пакеты.
```console
pacman -Suy
pacman -Suy
pacman -S base-devel mingw-w64-x86_64-make mingw-w64-x86_64-gcc mingw-w64-x86_64-postgresql mingw-w64-x86_64-qt5 git
```

| Библиотека | Версия | Комментарий |
|---|---|---|
|[mingw-w64-x86_64-make](https://packages.msys2.org/package/mingw-w64-x86_64-make)|4.3|GNU make utility to maintain groups of programs|
|[mingw-w64-x86_64-gcc](https://packages.msys2.org/package/mingw-w64-x86_64-gcc)|11.2.0|GNU Compiler Collection (C,C++,OpenMP)|
|[mingw-w64-x86_64-postgresql](https://packages.msys2.org/package/mingw-w64-x86_64-postgresql)|14.2|Libraries for use with PostgreSQL|
|[mingw-w64-x86_64-qt5](https://packages.msys2.org/package/mingw-w64-x86_64-qt5)|5.15.2|Meta package for Qt5 components|
|[git](https://packages.msys2.org/base/git)|2.35.3-1|The fast distributed version control system|

### Создаём папку для приложения
```console
mkdir c:/pgModeler
```
### Создаём папку для исходного кода
```console
mkdir c:/repo
```
### Загружаем исходный код
На момент написания статьи последняя версия сборки 1.0.0-alpha.
Загружаем исходный код с [Github](https://github.com/pgmodeler/pgmodeler)
```console
cd c:/repo
git clone https://github.com/pgmodeler/pgmodeler.git
# Выбираем версию сборки
cd c:/repo/pgmodeler
git checkout 1.0.0-alpha
# Загружаем подключаемые библиотеки
cd c:/repo/pgmodeler
git clone https://github.com/pgmodeler/plugins.git
```
### Выполняем конфигурацию
```console
qmake -r CONFIG+=release PREFIX=C:/pgModeler pgmodeler.pro
```
### Выполняем компиляцию
```console
cd c:/repo/pgmodeler
make
```
### Перемещаем скомпилированные файлы в `C:\pgModeler`
```console
cd c:/repo/pgmodeler
make install
cd c:/pgModeler
windeployqt pgmodeler.exe gui.dll
```
### Копируем библиотеки в `C:\pgModeler`
```console
cd c:/msys64/mingw64/bin/
cp libicuin*.dll libicuuc*.dll libicudt*.dll libpcre2-16-0.dll libharfbuzz-0.dll \
   libpng16-16.dll libfreetype-6.dll libgraphite2.dll libglib-2.0-0.dll libpcre-1.dll \
   libbz2-1.dll libssl-1_1-x64.dll libcrypto-1_1-x64.dll libgcc_s_seh-1.dll \
   libstdc++-6.dll libwinpthread-1.dll zlib1.dll libpq.dll libxml2-2.dll liblzma-5.dll \
   libiconv-2.dll libintl-8.dll libbrotlidec.dll libbrotlicommon.dll \
   libdouble-conversion.dll libzstd.dll libmd4c.dll C:/pgModeler
```
## Удаляем файлы созданные в процессе компиляции
```console
cd c:/repo/pgmodeler
git submodule foreach --recursive "git clean -dfx" && git clean -dfx
```
## Ссылки на источники
1. [Сборка pgModeler](https://pgmodeler.io/support/installation)
