Доступные приложения с зависимостями
====================================

MAX
---

Скачать
~~~~~~~

.. code-block:: bash

   # Добавить репозиторий
   sudo mkdir -p /etc/apt/keyrings
   curl -fsSL https://download.max.ru/linux/deb/public.asc | sudo gpg --dearmor -o /etc/apt/keyrings/max.gpg >/dev/null
   echo "deb [arch=amd64 signed-by=/etc/apt/keyrings/max.gpg] https://download.max.ru/linux/deb stable main" | sudo tee /etc/apt/sources.list.d/max.list

   # Обновить индексы
   sudo apt update

   # Скачаем сам МАКС без зависимостей
   sudo apt-get install --download-only -o Dir::Cache=/media/sv/repo/deb max

   # Просмострим зависимости МАКС
   apt-cache depends max

   # Скачаем зависимости МАКС
   sudo apt-get install --download-only --reinstall -o Dir::Cache=/media/sv/repo/deb \
      libxcb-xinerama0 \
      libxcb-composite0 \
      libxcb-ewmh2 \
      libva-x11-2 \
      libva-drm2 \
      libvdpau1 \
      libnotify4 \
      libxcb-dri2-0 \
      libopengl0 \
      libxcb-cursor0 \
      libxkbcommon-x11-0 \
      libxcb-icccm4 \
      libxcb-keysyms1 \
      libxss1 \
      libglib2.0-0t64 \
      gsettings-desktop-schemas \
      ca-certificates \
      desktop-file-utils \
      hicolor-icon-theme \
      pipewire


Установить
~~~~~~~~~~

Мессанджер MAX

Зависимости собраны для Debian 13.5

.. code-block:: bash

   sudo dpkg -i deps/max/*.deb
