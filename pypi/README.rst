PyPi репозиторий
================

Добавить зависимость
--------------------

1. Добавить зависимость в ``/repo/pypi/requeirements.txt``
2. Выполнить команду ``pip download -r requirements.txt -d <packages_path>``

.. code-block:: bash
 
   pip download -r requirements.txt -d /media/sv/repo/pypi/packages/

Установить зависимость через PIP
--------------------------------

Пример: установить sphinx

.. code-block:: bash

   pip install --no-index --find-links /media/sv/repo/pypi/packages/ sphinx

