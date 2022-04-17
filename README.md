#  Сборка pgModeler
Make pgModeler from source code.

## Подготовка окружения
Операционная система Windows XP Pro 64-bit.

ПО|Сайт|Установочный пакет|Путь установки
---|---|---
7zip 64-bit|[Посетить](https://www.7-zip.org/)|[Загрузить](https://www.7-zip.org/a/7z2107-x64.exe)|По умолчанию
Git 2.6.3 64-bit|[Посетить](https://github.com/git-for-windows/git/releases/tag/v2.6.3.windows.1)|[Загрузить](https://github.com/git-for-windows/git/releases/download/v2.6.3.windows.1/Git-2.6.3-64-bit.exe)|`C:\Git`
Perl 5.24 32-bit|[Посетить](https://www.activestate.com/products/perl/)|[Загрузить](https://www.softportal.com/get-70-activeperl.html)|`C:\Perl`
Python 2.7 64-bit|[Посетить](https://www.python.org/downloads/release/python-2718/)|[Загрузить](https://www.python.org/ftp/python/2.7.18/python-2.7.18.amd64.msi)|`C:\Python27`
C++ compiler 32-bit|[Посетить](https://sourceforge.net/projects/mingw-w64/)|[Загрузить](https://sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win32/Personal%20Builds/mingw-builds/8.1.0/threads-win32/sjlj/i686-8.1.0-release-win32-sjlj-rt_v6-rev0.7z)|`C:\mingw-w64\mingw32`
PostgreSQL 10.20 32-bit|[Посетить](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads#windows)|[Загрузить](https://www.enterprisedb.com/postgresql-tutorial-resources-training?uuid=e9a60157-955a-4f24-a738-07cff51ccb43&campaignId=70138000000rYFmAAM)

## Сборка Qt
###Загружаем исходный код
```console
git clone git://code.qt.io/qt/qt5.git
```
*Зеркало исходного кода Qt на [Github](https://github.com/qt/qt5).*
```console
git clone https://github.com/qt/qt5.git
```
###Выбираем версию сборки
```console
cd qt5
git checkout 5.6
```
###Загружаем исходный код модулей
```console
perl init-repository
```
###Выполняем конфигурацию
```console
.\configure -release -opensource -confirm-license -platform win32-g++ -no-openssl -system-proxies -no-freetype -plugin-sql-odbc -plugin-sql-sqlite -opengl desktop -prefix C:\Qt\Qt-5.6.4
```
###Выполняем компиляцию
```console
mingw32-make release -j4
```
###Перемещаем скомпилированные файлы в `C:\Qt\Qt-5.6.4\`
```console
mingw32-make install
```


##Сборка pgModeler
###Загружаем исходный код
```console
git clone https://github.com/pgmodeler/pgmodeler.git
```
###Выбираем версию сборки
```console
cd pgmodeler
git checkout main
```
На момент написания статьи версия сборки 0.9.4.
###
Правим пути в файле `pgmodeler.pri`.
Было.
```
windows {
  !defined(PGSQL_LIB, var): PGSQL_LIB = C:/msys64/mingw64/bin/libpq.dll
  !defined(PGSQL_INC, var): PGSQL_INC = C:/msys64/mingw64/include
  !defined(XML_INC, var): XML_INC = C:/msys64/mingw64/include/libxml2
  !defined(XML_LIB, var): XML_LIB = C:/msys64/mingw64/bin/libxml2-2.dll
```
Стало.
```
windows {
  !defined(PGSQL_LIB, var): PGSQL_LIB = C:/PostgreSQL/10/bin/libpq.dll
  !defined(PGSQL_INC, var): PGSQL_INC = C:/PostgreSQL/10/include
  !defined(XML_INC, var): XML_INC = C:/PostgreSQL/10/include/libxml
  !defined(XML_LIB, var): XML_LIB = C:/PostgreSQL/10/bin/libxml2.dll
```
###Устанавливаем путь к Qt
```console
C:\Perl\site\bin;C:\Perl\bin;C:\Program Files (x86)\AMD APP\bin\x86_64;C:\Program Files (x86)\AMD APP\bin\x86;%SystemRoot%\system32;%SystemRoot%;%SystemRoot%\System32\Wbem;%SYSTEMROOT%\System32\WindowsPowerShell\v1.0\;C:\Program Files\Intel\WiFi\bin\;C:\Program Files\Common Files\Intel\WirelessCommon\;C:\Program Files (x86)\ATI Technologies\ATI.ACE\Core-Static;C:\mingw-w64\mingw32\bin;C:\Python27;C:\Qt\Qt-5.11.3\bin
```
###Выполняем конфигурацию
```console
qmake PREFIX+=C:/pgModeler -r -spec win32-g++ CONFIG+=release pgmodeler.pro
```
###Выполняем компиляцию
```console
mingw32-make -j4
```
###Перемещаем скомпилированные файлы в `C:\pgModeler`
```console
mingw32-make install
```

## Ссылки на источники
1. [Cобираем qt-4.8.7 и qt creator при помощи mingw-w64 на windows (10), бонусом настравиваем на работу с github](https://habr.com/ru/post/367163/)
2. [Сборка pgModeler](https://habr.com/ru/post/423377/)