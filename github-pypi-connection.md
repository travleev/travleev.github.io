---

---


# Для чего нужен PyPi?

С точки зрения пользователя пакета, для которого простота установки -- главное.

Python Packageing Authority (PyPA) рекомендует ``pip`` и в качестве наиболее
стандартного метода установки -- установку из Python Package Index (PyPI).
При этом можно указать не только название пакета, но и конкретную версию. 

Также есть возможность устанавливать пакеты напрямую с сервера VCS. Для этого 
необходимо указывать тип сервера и путь к нему. 

Также есть возможность установки из других мест, включая локальную файловую систему.

Как я понимаю, только установка из PyPI позволяет указать конкретный номер
версии. Кроме того, команда для установки из PyPI наиболее короткая -- содержит
только название пакета и, опционально, желаемую версию. Можно предположить, что
для пользователя важно, чтобы команда установки была короткой и ему не нужно
было вникать в дополнительные сущности, такие как репозиторий на github.com или
делать дополнительные шаги, например, скачивать дистрибутив. 

# Как загружать в PyPI новую версию

PyPA рекомендует использовать ``twine`` для закачивания на PyPI дистрибутивов, 
созданных с помощью ``setuptools`` и ``wheel``. 

Про формат номера версии PEP 440 описывает схему, в которую укладывается семантический формат.

PyPA
[описывает](https://packaging.python.org/guides/single-sourcing-package-version/#single-sourcing-the-version)
несколько способов указания номера версии, который используется как в
``setup.py`` так и во время выполнения пакета, например, для определения
``package.__version__``. Следует обратить внимание и попробовать последний, 7-й
способ.  В нем используется пакет ``setuptools_csm``, который может
генерировать версию на основе информации, взятой из git. 


# Опыт реализации на примере ``twps``
Файл ``twps/version.py`` не следует закачивать в репо. Дело в том, что если он там находится, необходимо
следить за тем, чтобы он обновлялся в соответствии со статусом репо. Этот файл обновляется в этом месте
только после команды ``pip install -e .`` про которую можно забыть (пакет-то уже один раз установлен и
используются файлы прямо из репо). Для пользователя, который решил установить пакет из github, наличие 
этого файла, в случае его несоответствия текущему состоянию репо, приведет к тому, что пользователь
будет видеть неправильную версию. Поэтому я заменил логику импортирования этого файла: если его нет, это
означает что используется не дистрибутив а репо, выдается спец. версия. 

На PyPI можно закачать только дистрибутивы с форматом версии ``X.Y.Z``, поэтому перед закачиванием НЕОБХОДИМО
обновить tag у репо. Затем можно на основе этого тэга определить релиз (это делается прямо в веб-интерфейсе). Вот
важные команды::

    >git describe
    >git tag -a 2.0.1 -m "message for the new tag"
    >git push 2.0.1  # otherwise not uploaded
    
Возник вопрос, какую версию приписать изменениям, связанным с введением
``setuptools_scm``. С одной стороны, новый номер версии необходим, чтобы
дистрибутив можно было загрузить на PyPI. С другой стороны, такие изменения не предсталяют
ни изменение публичного API, ни введение нового функционала, ни исправление багов, поэтому 
новой версии не требуют. Вообще-то следует руководствоваться как раз вторым соображением, так
как детали генерации дистрибутива и вся остальная кухня работы с настройками пользователя не
интересует. В этом конкретном примере мне изменение номера было необходимо, чтобы довести этот 
пример до конца -- загрузить дистрибутив в PyPI, поэтому я просто увеличил последнее число. 


