Webdav-client
=============

|PyPI version|

Пакет webdav-client обеспечивает легкую и удобную работу с
webdav-серверами(Yandex.Disk). В данный пакет включены следующие
компоненты: webdav API, resource API и webdav tool.

Исходники на https://github.com/designerror/webdav-client

Установка
=========

-  `pip <https://pypi.python.org/pypi/pip/>`__ install
   `webdav-client <https://pypi.python.org/pypi/webdav-client>`__
-  `easy\_install <https://pypi.python.org/pypi/setuptools>`__
   `webdav-client <https://pypi.python.org/pypi/webdav-client>`__

Webdav API
==========

Webdav API - представляет из себя набор webdav-методов работы с
облачными хранилищами. В этот набор входят следующие методы: check,
free, info, list, mkdir, clean, copy, move, download, upload, publish,
unpublish, published.

*Настройка клиента*
===================

.. code:: python

    import webdav-client as webdav
    options = {
        'webdav_hostname': "https://webdav.yandex.ru",
        'webdav_login': "login",
        'webdav_paassword': "password"
    }
    client = webdav.Client(options)

*Синхронные методы*
===================

**Проверка существования ресурса**

.. code:: python

    client.check("dir1/file1")
    client.check("dir1/")

**Получение информации о ресурсе**

.. code:: python

    client.info("dir1/file1")
    client.info("dir1/")

**Проверка свободного места**

.. code:: python

    free_size = client.free()

**Получение списка ресурсов**

.. code:: python

    files1 = client.list()
    files2 = client.list("dir1")

**Создание директории**

.. code:: python

    client.mkdir("dir1/dir2")

**Удаление ресурса**

.. code:: python

    client.clean("dir1/dir2/")

**Копирование ресурса**

.. code:: python

    client.copy(remote_path_from="dir1/file1", remote_path_to="dir2/file1")

**Перемещения ресурса**

.. code:: python

    client.move(remote_path_from="dir1/file1", remote_path_to="dir2/file1")

**Загрузка ресурса**

.. code:: python

    client.download_sync(remote_path="dir1/file1", local_path="~/Downloads/file1")
    client.download_sync(remote_path="dir1/dir2/", local_path="~/Downloads/dir2/")

**Выгрузка ресурса**

.. code:: python

    client.upload_sync(remote_path="dir1/file1", local_path="~/Documents/file1")
    client.upload_sync(remote_path="dir1/dir2/", local_path="~/Documents/dir2/")

**Публикация ресурса**

.. code:: python

    link = client.publish("dir1/file1")

**Отмена публикации ресурса**

.. code:: python

    client.unpublish("dir1/file1")

**Обработка исключений**

.. code:: python

    try:
        ...
    except WebDavException as e:
        loggin_except(e)

*Ассинхронные методы*
=====================

**Загрузка ресурса**

.. code:: python

    client.download_async(remote_path="dir1/file1", local_path="~/Downloads/file1", callback=callback)
    client.download_async(remote_path="dir1/dir2/", local_path="~/Downloads/dir2/", callback=callback)

**Выгрузка ресурса**

.. code:: python

    client.upload_async(remote_path="dir1/file1", local_path="~/Documents/file1", callback=callback)
    client.upload_async(remote_path="dir1/dir2/", local_path="~/Documents/dir2/", callback=callback)

Resource API
============

Resource API - используя концепцию ООП, обеспечивает работу с облачными
хранилищами на уровне ресурсов.

**Получение ресурса**

.. code:: python

    res1 = client.resource("dir1/file1")

**Работа с ресурсом**

.. code:: python

    res1.rename("file2")

    res1.move("dir1/file2")

    res1.copy("dir2/file1")

    info = res1.info()

    res1.read_from(buffer)

    res1.read(local_path="~/Documents/file1")

    res1.read_async(local_path="~/Documents/file1", callback)

    res1.write_to(buffer)

    res1.write(local_path="~/Downloads/file1")

    res1.write_async(local_path="~/Downloads/file1", callback)

Webdav tool
===========

Webdav tool - кросплатформенная утилита, обеспечивающая удобную работу с
webdav-серверами прямо из Вашей консоли. Помимо полной реализации
методов из webdav API, также добавлены методы синхронизации содержимого
локальной и удаленной директории.

*Аутентификация*
================

.. code:: bash

    $ webdav login https://wedbav.yandex.ru -p http://127.0.0.1:8080
    webdav_login: w_login
    webdav_password: w_password
    proxy_login: p_login
    proxy_password: p_password

*Работа с утилитой*
===================

.. code:: bash

    $ webdav -h
    $ webdav check
    success
    $ webdav check file1
    not success
    $ webdav free
    245234120344
    $ webdav ls dir1
    file1
    ...
    fileN
    $ webdav mkdir dir2
    $ webdav copy dir1/file1 -t dir2/file1
    $ webdav move dir2/file1 -t dir2/file2
    $ webdav download dir1/file1 -t ~/Downloads/file1
    $ webdav download dir1/ -t ~/Downloads/dir1/
    $ webdav upload dir2/file2 -f ~/Documents/file1
    $ webdav upload dir2/ -f ~/Documents/
    $ webdav publish di2/file2
    https://yadi.sk/i/vWtTUcBucAc6k
    $ webdav unpublish dir2/file2
    $ webdav pull dir1/ -t ~/Documents/dir1/
    $ webdav push dir1/ -f ~/Documents/di1/
    $ webdav info dir1/file1
    { 'name': 'file1', 'modified': 'Thu, 23 Oct 2014 16:16:37 GMT', 'size': '3460064', 'created': '2014-10-23T16:16:37Z'}

.. |PyPI version| image:: https://badge.fury.io/py/webdav-client.svg
   :target: http://badge.fury.io/py/webdav-client