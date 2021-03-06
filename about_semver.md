---
date: 2020.01.22
---

[Оказывается](https://semver.org/#spec-item-1),
необходимое условие использования семантического формата номера версии --
наличие публичного API. 

Сейчас в различных пакетах формат номеров такой:

| package    | Format   | Current version |
|------------|----------|-----------------|
| numjuggler | X.Y[a].Z | 2.42a.29        |
| twps       | X.Y.Z    | 2.0.1           |
| pirs2      | X.Y[a].Z | 2.25a.12        |
| mip        | X.Y[a].Z | 0.0.0           |
| tovtk      | X.Y.Z    | 1.4.2           |


Использование необязательной ``"a"`` во втором числе -- для обозначения версий, 
в которых я не слежу за обратной совместимостью при добавлении функций. Я 
ввел такую систему, видимо, работая над pirs, так как там было некогда следить 
за обратной совместимостью со "старыми" подпакетами. Сейчас я склоняюсь к тому, 
что семантический формат номера, где первое число увеличивается каждый раз, когда
вводятся обратно несовместимые изменения, подходит лучше, даже если нет четко
сформулированного публичного API. 

Для публикации пакетов на PyPi (я как раз сейчас с этим разбираюсь) необходимо
использование некоторого формата нумерации версий. Пока я склоняюсь к тому, чтобы
все пакеты перед публикацией на PyPi перевести на сематнический формат ``X.Y.Z``

В семантическом формате роль флага ``"a"`` играет версия, начинающаяся с 0. По
[предложенному правилу](https://semver.org/#spec-item-4), для такой версии
публичный API не должен рассматриваться как стабильный. 

Понятие публичного API неприменимо для инструментов командной строки
``numjuggler`` или ``tovtk``, так как функции из этих пакетов не предполагаются
для импорта из других программ. Однако для них есть спецификация параметров
командной строки, которая и выполняет роль описания интерфейса между программой
и пользователем. Для этих пакетов первый номер в версии следует изменять, когда
изменяется формат параметров командной строки. Сюда-же следует отнести и ``twps``. 

Два других пакета, упомянутых в таблице выше, ``pirs2`` и ``mip``. Они как раз
содержат объекты, которые предполагается использоать из других программ. Здесь
понятие публичного API полностью применимо. Следует отметить, для ``pirs2``
декларация публичного API, то есть описание классов и т.п. есть, но устаревшее.
А для ``mip`` такого описания почти нет.

