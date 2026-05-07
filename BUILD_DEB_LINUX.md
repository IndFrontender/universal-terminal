# 🐧 Сборка DEB пакета для Linux

## 📋 Требования

### Минимальные
- **ОС:** Ubuntu 18.04+, Debian 10+, Linux Mint 19+, Fedora 30+
- **Python:** 3.8 или выше
- **Память:** 2 GB RAM
- **Место на диске:** 500 MB

### Зависимости
Пакет теперь пытается подтянуть необходимые зависимости сам при установке через `postinst`.
```bash
python3
python3-pip
python3-tk
dpkg-dev
fakeroot
librsvg2-bin
```

Если в системе доступен репозиторий с `python3-customtkinter`, он будет установлен через `apt`. Иначе скрипт попробует добрать Python-зависимости через `pip`.
```bash
python3-customtkinter
clipboard
pyperclip
```

---

## 🚀 Быстрая сборка

### 1. Клонирование репозитория
```bash
git clone https://github.com/yourusername/terminal_commands_app.git
cd terminal_commands_app
```

### 2. Запуск скрипта сборки
```bash
chmod +x build_scripts/build_linux.sh
./build_scripts/build_linux.sh --auto
```

### 3. Результат
После успешной сборки в папке `dist/` появятся:
- `TerminalCommandsGuide` - портативная версия
- `terminal-commands-guide_2.1.0_amd64.deb` - DEB пакет с автодобором зависимостей

Если нужен автономный вариант, собери так:
```bash
./build_scripts/build_linux.sh --static
```

---

## 📦 Подробная пошаговая инструкция

### Шаг 1: Установка зависимостей

```bash
# Обновление репозиториев
sudo apt update

# Установка необходимых пакетов
sudo apt install -y \
    python3 \
    python3-pip \
    python3-tk \
    python3-venv \
    dpkg-dev \
    fakeroot \
    librsvg2-bin
```

### Шаг 2: Установка Python зависимостей

```bash
# Создание виртуального окружения (рекомендуется)
python3 -m venv venv
source venv/bin/activate

# Обновление pip
pip install --upgrade pip

# Установка зависимостей
pip install -r requirements.txt
```

### Шаг 3: Сборка executable

```bash
# Сборка через PyInstaller
pyinstaller --onefile \
    --windowed \
    --name "TerminalCommandsGuide" \
    --add-data "commands_data.py:." \
    --hidden-import=clipboard \
    --hidden-import=pyperclip \
    main.py
```

### Шаг 4: Создание структуры DEB пакета

```bash
# Переменные
PACKAGE_NAME="terminal-commands-guide"
VERSION="2.1.0"
ARCH="amd64"
DEB_DIR="deb_build"
PACKAGE_DIR="$DEB_DIR/$PACKAGE_NAME-$VERSION"

# Создание директорий
rm -rf "$DEB_DIR"
mkdir -p "$PACKAGE_DIR/DEBIAN"
mkdir -p "$PACKAGE_DIR/usr/bin"
mkdir -p "$PACKAGE_DIR/usr/share/applications"
mkdir -p "$PACKAGE_DIR/usr/share/icons/hicolor/256x256/apps"
mkdir -p "$PACKAGE_DIR/usr/share/doc/$PACKAGE_NAME"

# Копирование executable
cp dist/TerminalCommandsGuide "$PACKAGE_DIR/usr/bin/"
chmod +x "$PACKAGE_DIR/usr/bin/TerminalCommandsGuide"
```

### Шаг 5: Создание control файла

```bash
cat > "$PACKAGE_DIR/DEBIAN/control" << EOF
Package: terminal-commands-guide
Version: 2.1.0
Section: utils
Priority: optional
Architecture: amd64
Maintainer: TerminalCommandsApp <support@terminalcommands.app>
Description: Terminal Commands Guide - Cross-platform terminal commands reference
 Cross-platform application with terminal commands for Windows, Linux and macOS.
 .
 Features:
  - 221+ commands for Linux, Windows and macOS
  - Search and filter by categories
  - Copy commands to clipboard
  - Real command execution
  - Bilingual interface (RU/EN)
Depends: python3, python3-pip, python3-tk, python3-customtkinter
Recommends: gnome-terminal | konsole | xterm
Homepage: https://github.com/terminalcommands
EOF
```

### Шаг 6: Создание postinst скрипта

```bash
cat > "$PACKAGE_DIR/DEBIAN/postinst" << 'EOF'
#!/bin/bash
set -e

chmod +x /usr/bin/TerminalCommandsGuide

# Обновление кэша иконок и desktop файлов
if [ -x "/usr/bin/update-desktop-database" ]; then
    update-desktop-database /usr/share/applications 2>/dev/null || true
fi

if [ -x "/usr/bin/gtk-update-icon-cache" ]; then
    gtk-update-icon-cache -f /usr/share/icons/hicolor 2>/dev/null || true
fi

echo ""
echo "========================================"
echo "  Terminal Commands Guide v2.1 installed!"
echo "========================================"
echo ""
echo "Launch:"
echo "  - From applications menu: Terminal Commands Guide"
echo "  - From terminal: TerminalCommandsGuide"
echo ""
echo "Uninstall:"
echo "  sudo apt remove terminal-commands-guide"
echo ""
EOF

chmod +x "$PACKAGE_DIR/DEBIAN/postinst"
```

### Шаг 7: Создание .desktop файла

```bash
cat > "$PACKAGE_DIR/usr/share/applications/terminal-commands-guide.desktop" << EOF
[Desktop Entry]
Version=1.0
Type=Application
Name=Terminal Commands Guide
Comment=Cross-platform terminal commands reference
Exec=/usr/bin/TerminalCommandsGuide
Icon=terminal-commands-guide
Categories=Utility;System;Development;
Terminal=false
Keywords=terminal;commands;linux;reference;
StartupNotify=true
StartupWMClass=TerminalCommandsGuide
EOF
```

### Шаг 8: Сборка DEB пакета

```bash
cd deb_build
fakeroot dpkg-deb --build terminal-commands-guide-2.1.0
cd ..

# Копирование в dist
cp deb_build/terminal-commands-guide-2.1.0.deb dist/
```

---

## 📊 Структура DEB пакета

```
terminal-commands-guide_2.1.0_amd64.deb
├── DEBIAN/
│   ├── control          # Информация о пакете
│   ├── postinst         # Скрипт после установки
│   └── postrm           # Скрипт после удаления
├── usr/
│   ├── bin/
│   │   └── TerminalCommandsGuide  # Executable
│   ├── share/
│   │   ├── applications/
│   │   │   └── terminal-commands-guide.desktop  # Desktop файл
│   │   ├── icons/
│   │   │   └── hicolor/256x256/apps/
│   │   │       └── terminal-commands-guide.png  # Иконка
│   │   └── doc/
│   │       └── terminal-commands-guide/
│   │           ├── README          # Документация
│   │           ├── changelog.gz    # История изменений
│   │           └── copyright       # Лицензия
```

---

## 🧪 Тестирование DEB пакета

### Проверка содержимого
```bash
# Просмотр содержимого DEB файла
dpkg-deb --contents dist/terminal-commands-guide_2.1.0_amd64.deb

# Извлечение содержимого
dpkg-deb --extract dist/terminal-commands-guide_2.1.0_amd64.deb /tmp/test-install
```

### Проверка control информации
```bash
dpkg-deb --info dist/terminal-commands-guide_2.1.0_amd64.deb
```

### Лектинг пакета (проверка на ошибки)
```bash
lintian dist/terminal-commands-guide_2.1.0_amd64.deb
```

---

## 📋 Установка и тестирование

### Установка
```bash
sudo dpkg -i dist/terminal-commands-guide_2.1.0_amd64.deb
sudo apt install -f  # Исправление зависимостей если нужно
```

### Проверка установки
```bash
# Проверка наличия executable
which TerminalCommandsGuide

# Проверка версии
TerminalCommandsGuide --version  # Если реализовано

# Запуск
TerminalCommandsGuide
```

### Удаление
```bash
sudo apt remove terminal-commands-guide
```

---

## 🔧 Решение проблем

### Проблема: "unable to locate package python3-customtkinter"
**Решение:** Установить через pip
```bash
pip3 install customtkinter
```

### Проблема: "dpkg-deb: error: failed to read archive"
**Решение:** Проверить целостность
```bash
dpkg-deb --info dist/terminal-commands-guide_2.1.0_amd64.deb
```

### Проблема: "Icon theme not found"
**Решение:** Обновить кэш иконок
```bash
sudo gtk-update-icon-cache -f /usr/share/icons/hicolor
```

### Проблема: Приложение не запускается
**Решение:** Проверить зависимости
```bash
ldd /usr/bin/TerminalCommandsGuide
```

---

## 📦 Публикация в репозиторий

### Создание PPA (Ubuntu)
```bash
# Установка инструментов
sudo apt install -y devscripts debuild

# Сборка исходников
debuild -S -sa

# Загрузка на Launchpad
dput ppa:your-ppa terminal-commands-guide_2.1.0_source.changes
```

### Создание собственного репозитория
```bash
# Установка reprepro
sudo apt install -y reprepro

# Создание структуры
mkdir -p ~/repo/{conf,dists,pool}

# Создание conf/distributions
cat > ~/repo/conf/distributions << EOF
Origin: TerminalCommands
Label: TerminalCommandsGuide
Codename: stable
Architectures: amd64
Components: main
Description: Terminal Commands Guide Repository
EOF

# Добавление пакета
reprepro includedeb stable dist/terminal-commands-guide_2.1.0_amd64.deb
```

---

## 📊 Сравнение с другими форматами

| Формат | Размер | Преимущества | Недостатки |
|--------|--------|--------------|------------|
| **DEB** | ~28 MB | Интеграция с системой, автообновление | Только Debian/Ubuntu |
| **Portable** | ~25 MB | Работает везде | Нет интеграции |
| **AppImage** | ~30 MB | Универсальный | Больше размер |
| **Snap** | ~35 MB | Автообновление, песочница | Медленный запуск |
| **Flatpak** | ~30 MB | Песочница, универсальный | Требует runtime |

---

## 📞 Поддержка

### Логи сборки
```bash
# Лог PyInstaller
cat build/TerminalCommandsGuide/warn-TerminalCommandsGuide.txt

# Лог dpkg
dpkg-deb --deb-format=3.0 build/... 2>&1 | tee build.log
```

### Тестирование на чистых системах
Рекомендуется тестировать на:
- Ubuntu 18.04 LTS
- Ubuntu 20.04 LTS
- Ubuntu 22.04 LTS
- Debian 10 (Buster)
- Debian 11 (Bullseye)
- Linux Mint 20/21

---

**Версия:** 2.1  
**Дата:** 20.04.2026  
**Статус:** ✅ Готово к использованию
