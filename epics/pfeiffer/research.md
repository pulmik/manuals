Переписка из которой следует, что для работы достаточно модулей async и stream:

https://epics.anl.gov/tech-talk/2019/msg01935.php

> Hey Patrick,
>
> I had a quick look at the data sheet. Unfortunately I could not find any information on the data grams.
> If the Ethernet communication is string based you can keep things simple and just use asyn+stream. That's what we are using for our devices from Pfeiffer (including the > pumps for small test setups).
>
> > Am 16.12.2019 um 16:42 schrieb Patrick Oppermann via Tech-talk:
> > Hello, everybody,
> > we would like to read a mass spectrometer from Pfeiffer.
> > 
> > https://www.pfeiffer-vacuum.com/en/products/measurement-analysis/analysis-equipment/residual-gas-analysis/residual-gas-analysis-in-ultra-high-vacuum/prismapro-qmg-250-m1-1-100-u/32109/prismapro-qmg-250-m1-1-100-u
> > Has anyone ever written a device driver for it?
> > Thank you very much,
> > Patrick
>
> --
> 
> Dr. Florian Feldbauer

Ссылка на проект с описанием БД для Pfeiifer:

https://www.imca.aps.anl.gov/~jlmuir/sw/pfeiffer-tpg-256-a.html

Ссылка на модуль calc:

https://epics-modules.github.io/calc/


Создание простого проекта из примера:

https://docs.epics-controls.org/projects/how-tos/en/latest/getting-started/creating-ioc.html

https://docs.epics-controls.org/en/latest/appdevguide/gettingStarted.html