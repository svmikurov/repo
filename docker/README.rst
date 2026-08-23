Docker
======

План действий

Подготовить Dockerfile, который:

- Берет базовый образ Python (который вы сохранили на диск).
- Копирует папку с Sphinx-зависимостями внутрь образа.
- Устанавливает Sphinx из локальных .whl файлов.
- (Опционально) Устанавливает MAX из .deb файлов.

Собрать образ на машине с доступом к диску.

Сохранить образ в .tar файл на диск.

В закрытом контуре загрузить образ из .tar и запустить.

Dockerfile
----------

.. code-block:: bash

   # Берем базовый образ Python с диска (или из локального реестра)
   FROM python:3.11-slim

   # Отключаем интернет-индексы для pip (работаем только локально)
   ENV PIP_NO_INDEX=true
   ENV PIP_FIND_LINKS=/mnt/pypi

   # Создаем папку для пакетов внутри контейнера
   WORKDIR /app

   # Копируем папку с Sphinx-зависимостями внутрь образа
   COPY /media/sv/repo/pypi/packages /mnt/pypi

   # Устанавливаем Sphinx и его зависимости локально
   RUN pip install --no-cache-dir sphinx

   # (Опционально) Устанавливаем MAX, если он нужен в этом образе
   # Для этого копируем папку с .deb файлами MAX
   COPY /media/sv/repo/deb/max /mnt/deb/max
   RUN dpkg -i /mnt/deb/max/*.deb || apt-get --fix-broken install -y

   # Точка входа (опционально)
   CMD ["sphinx-build", "--help"]

**Что здесь важно**:

FROM python:3.13-slim — образ должен быть уже сохранен на диске (вы его сохранили ранее командой docker save).

PIP_NO_INDEX и PIP_FIND_LINKS заставляют pip искать пакеты только в папке /mnt/pypi.

Установка MAX — это демонстрация, как ставить .deb пакеты внутри Docker-образа.

В открытом контуре
------------------

Загружаем базовый образ Python (если его нет)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

   # Скачиваем образ из интернета (на машине с доступом)
   docker pull python:3.13-slim

   # Сохраняем его на диск
   docker save -o /media/sv/repo/docker_images/python-3.13-slim.tar python:3.13-slim


Загружаем базовый образ в Docker (на сборочной машине)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash
   docker load -i /media/sv/repo/docker/images/python-3.13-slim.tar

Собираем образ с Sphinx
~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

   cd /media/sv/repo/dockerfiles/sphinx
   docker build -t sphinx-image:latest .

Сохраняем собранный образ на диск
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

   docker save -o /media/sv/repo/docker/images/sphinx-image.tar sphinx-image:latest


В закрытом контуре
------------------

.. code-block:: bash

   # Загружаем базовый образ (если он не загружен)
   docker load -i /media/sv/repo/docker.images/python-3.13-slim.tar

   # Загружаем образ с Sphinx
   docker load -i /media/sv/repo/docker.images/sphinx-image.tar