Создание простого проекта из примера:

https://docs.epics-controls.org/projects/how-tos/en/latest/getting-started/creating-ioc.html

``` bash
mkdir sampleIOC; cd sampleIOC
makeBaseApp.pl -t example sampleIOC
makeBaseApp.pl -i -t example sampleIOC
cd sampleIOC
```

Добавление модулей в проект:

В файл `sampleIOCApp/src/Makefile` добавить строки:

``` Makefile
#add asyn and streamDevice to this IOC production libs
#sampleIOC_LIBS += stream 
sampleIOC_LIBS += asyn
#sampleIOC_LIBS += calc - не установился
```

> По поводу модуля `stream` непонятно нужен ли он:
>
> Тут написано, что он не рекомендуется к использованию: https://github.com/epics-modules/stream
>
>Ему на замену StreamDevice (вроде бы он интегрирован в EPICS): 
https://paulscherrerinstitute.github.io/StreamDevice/

Также надо добавить в `sampleIOCApp/src/xxxSupport.dbd`:

``` Makefile
include "asyn.dbd"
registrar(drvAsynIPPortRegisterCommands)
registrar(drvAsynSerialPortRegisterCommands)
registrar(vxi11RegisterCommands)
```

### Добавление Peiffer.db

В каталог `sampleIOCApp/Db/` поместить `pfeiffer.db`

Для работы необходим модуль `calc`, пока он не установлен, необходимо закомментировать записи типа `scalcout`

В файл `sampleIOCApp/Db/Makefile` добавить БД в список:

``` Makefile
TOP=../..
include $(TOP)/configure/CONFIG
#----------------------------------------
#  ADD MACRO DEFINITIONS BELOW HERE

# Install databases, templates & substitutions like this
DB += pfeiffer.db
#DB += circle.db
#DB += dbExample1.db
#DB += dbExample2.db
#DB += sampleIOCVersion.db
#DB += dbSubExample.db
DB += user.substitutions

# If <anyname>.db template is not named <anyname>*.template add
# <anyname>_TEMPLATE = <templatename>

include $(TOP)/configure/RULES
#----------------------------------------
#  ADD EXTRA GNUMAKE RULES BELOW HERE
```

Пересборка проекта:

``` bash
make distclean
make
```

### Редактируем файл `iocBoot/iocsampleIOC/st.cmd`

- создание канала связи
- чтение БД с указанием значений переменных вторым параметром

Добавлены строки:

``` bash
# connect to the device ... IP-Address ! Port 2025 used by textronix, see manual
drvAsynIPPortConfigure("L0","127.0.0.1:80",0,0,0)

## Load record instances
dbLoadRecords("db/pfeiffer.db", "P=17ida:,D=Maxi1,C=1,CD=Tank Pressure,PORT=L0,DESC=17-ID")
```

Итоговый вид:

``` bash
#!../../bin/linux-x86_64/sampleIOC

#- You may have to change sampleIOC to something else
#- everywhere it appears in this file

< envPaths

cd "${TOP}"

## Register all support components
dbLoadDatabase "dbd/sampleIOC.dbd"
sampleIOC_registerRecordDeviceDriver pdbbase

## Load record instances
dbLoadTemplate "db/user.substitutions"
dbLoadRecords "db/sampleIOCVersion.db", "user=alex"
dbLoadRecords "db/dbSubExample.db", "user=alex"

# connect to the device ... IP-Address ! Port 2025 used by textronix, see manual
drvAsynIPPortConfigure("L0","127.0.0.1:80",0,0,0)

## Load record instances
dbLoadRecords("db/pfeiffer.db", "P=17ida:,D=Maxi1,C=1,CD=Tank Pressure,PORT=L0,DESC=17-ID")


#- Set this to see messages from mySub
#-var mySubDebug 1

#- Run this to trace the stages of iocInit
#-traceIocInit

cd "${TOP}/iocBoot/${IOC}"
iocInit

## Start any sequence programs
#seq sncExample, "user=alex"
```

### Запуск

```
cd iocBoot/iocsampleIOC/
chmod u+x st.cmd
./st.cmd
```


