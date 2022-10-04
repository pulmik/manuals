## Установка пакетов в Linux (Debian, RaspberryOS)

``` bash
sudo apt update
sudo apt install {packet_name1} {packet_name2} ...
```

## Скачивание репозитория GitHub или любого другого Git-репозитория

``` bash
git clone {https://адрес}
```

## Инструкция по установке Asyn

- [*docs.epics-controls.org* - Add the asyn package](https://docs.epics-controls.org/projects/how-tos/en/latest/getting-started/installation.html#add-the-asyn-package)
- [Страница модуля **asyn** на GitHub](https://github.com/epics-modules/asyn)

После скачивания модуля необходимо внести изменения в файл `configure/RELEASE` изменив переменные `EPICS_BASE` и `SUPPORT`, а также закомментировать (или оставить закомментированными) строки с указанием на модули IPAC, SNCSEQ, CALC, SSCAN.

``` bash
cd ~/EPICS/support
git clone https://github.com/epics-modules/asyn.git
cd asyn
nano configure/RELEASE
make
```

Содержимое файла `~/EPICS/support/asyn/configure/RELEASE`:

``` bash
# RELEASE Location of external products

HOME=/home/alex

# Define the following Required or Optional
# either in this file, or in a RELEASE.local

SUPPORT=$(HOME)/EPICS/support

# Optional
# IPAC is only necessary if support for Greensprings IP488 is required
# IPAC release V2-7 or later is required.
# It can be obtained here: https://github.com/epics-modules/ipac
# IPAC=$(SUPPORT)/ipac-2-15

# Optional
# SEQ is required for testIPServer
# It can be obtained here: https://www-csr.bessy.de/control/SoftDist/sequencer/Manual.html
# SNCSEQ=$(SUPPORT)/seq-2-2-5

# Optional
# For sCalcout support in asynOctet - applications include asynCalc.dbd
# It can be obtained here: https://github.com/epics-modules/calc
# CALC=$(SUPPORT)/calc-3-7-3

# Optional
# If CALC was built with SSCAN support then SSCAN must be defined for testEpicsApp
# It can be obtained here: https://github.com/epics-modules/sscan
# SSCAN=$(SUPPORT)/sscan-2-11-3

# Required
# EPICS_BASE 3.14.6 or later is required
# It can be obtained here: https://github.com/epics-base/epics-base
EPICS_BASE=$(HOME)/EPICS/epics-base

-include $(TOP)/../RELEASE.local
-include $(TOP)/../RELEASE.$(EPICS_HOST_ARCH).local
-include $(TOP)/configure/RELEASE.local
```

## Инструкция по установке Modbus

- [Страница модуля **modbus** на GitHub](https://github.com/epics-modules/modbus)
- [Документация](https://epics-modbus.readthedocs.io/en/latest/#)

После скачивания модуля необходимо внести изменения в файл `configure/RELEASE` изменив переменные `EPICS_BASE` и `SUPPORT`, а также убедиться, что переменная `ASYN` содержит путь к модулю ASYN.

``` bash
cd ~/EPICS/support
git clone https://github.com/epics-modules/modbus.git
cd modbus
nano configure/RELEASE
make
```

Содержимое файла `~/EPICS/support/modbus/configure/RELEASE`:

``` bash
#RELEASE Location of external products
# Run "gnumake clean uninstall install" in the application
# top directory each time this file is changed.
#
# NOTE: The build does not check dependancies on files
# external to this application. Thus you should run
# "gnumake clean uninstall install" in the top directory
# each time EPICS_BASE, SNCSEQ, or any other external
# module defined in the RELEASE file is rebuilt.
HOME=/home/alex

SUPPORT=$(HOME)/EPICS/support

ASYN=$(SUPPORT)/asyn

# If you don't want to install into $(TOP) then
# define INSTALL_LOCATION_APP here
#INSTALL_LOCATION_APP=<fullpathname>

# EPICS_BASE usually appears last so other apps can override stuff:
EPICS_BASE=$(HOME)/EPICS/epics-base

#Capfast users may need the following definitions
#CAPFAST_TEMPLATES=
#SCH2EDIF_PATH=
-include $(TOP)/../RELEASE.local
-include $(TOP)/../RELEASE.$(EPICS_HOST_ARCH).local
-include $(TOP)/configure/RELEASE.local
```
Запуск:

``` bash
user@debian:~$ cd ~/EPICS/support/modbus/iocBoot/iocTest
user@debian:~/EPICS/support/modbus/iocBoot/iocTest$ ../../bin/linux-x86_64/modbusApp Koyo1.cmd
```

