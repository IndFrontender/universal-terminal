# 🖥️ Terminal Commands Guide

Кроссплатформенное приложение-справочник команд терминала для **Windows**, **Linux** и **macOS**.

![Platform](https://img.shields.io/badge/platform-Windows%20%7C%20Linux%20%7C%20macOS-blue)
![Python](https://img.shields.io/badge/python-3.8+-green)
![License](https://img.shields.io/badge/license-MIT-yellow)
![Commands](https://img.shields.io/badge/commands-221+-brightgreen)

## 📋 Особенности

- ✅ **221+ команд** для трёх операционных систем (Windows: 54, Linux: 92, macOS: 75)
- ✅ **Реальное выполнение команд** — запуск в системном терминале с отображением результата
- ✅ **Кроссплатформенность** — работает на Windows, Linux и macOS
- ✅ **Поиск команд** — быстрый поиск по названию, описанию и тексту команды
- ✅ **Фильтрация** — по 8 категориям (Система, Файлы, Сеть, Навигация, Процессы, Реестр, Прочее)
- ✅ **Копирование** — копирование команды и результата в буфер обмена
- ✅ **Tooltip** — всплывающие подсказки с полным текстом команды
- ✅ **Двуязычность** — русский и английский интерфейсы
- ✅ **Тёмная тема** — современный дизайн в стиле IDE
- ✅ **Окно результата** — отображение вывода команды с возможностью копирования

## 🚀 Быстрый старт

### Установка и запуск из исходников

```bash
# Клонирование репозитория
git clone https://github.com/yourusername/terminal_commands_app.git
cd terminal_commands_app

# Установка зависимостей
pip install -r requirements.txt

# Запуск приложения
python main.py
```

### 📦 Готовые дистрибутивы (Windows)

**Портативная версия:**
- `dist/TerminalCommandsGuide.exe` (26 MB) - запустить напрямую
- `dist/TerminalCommandsGuide-Portable.zip` (26 MB) - распаковать и запустить

**Установщик (MSI):**
```powershell
# Требуется Inno Setup или WiX Toolset
# См. BUILD_INSTRUCTIONS.md
```

### Запуск приложения

```bash
python main.py
```

## 📦 Сборка исполняемых файлов

### Автоматическая сборка (для текущей ОС)

```bash
python build_scripts/build_all.py
```

### Ручная сборка по платформам

#### Windows (.exe)

```powershell
cd build_scripts
.\build_windows.ps1
```

**Результат:**
- `dist/TerminalCommandsGuide.exe` — portable версия
- `build/installer.wxs` — проект для создания MSI

#### Linux (.deb)

```bash
cd build_scripts
chmod +x build_linux.sh
./build_linux.sh
```

**Результат:**
- `dist/TerminalCommandsGuide` — portable версия
- `deb_build/terminal-commands-guide-1.0.0.deb` — DEB пакет

**Установка:**
```bash
sudo dpkg -i deb_build/terminal-commands-guide-1.0.0.deb
```

#### macOS (.app + .dmg)

```bash
cd build_scripts
chmod +x build_macos.sh
./build_macos.sh
```

**Результат:**
- `macos_build/TerminalCommandsGuide.app` — приложение
- `macos_build/TerminalCommandsGuide.dmg` — DMG образ

## 📖 Структура проекта

```
terminal_commands_app/
├── main.py                 # Главный файл приложения
├── commands_data.py        # База данных команд (221+ команд)
├── requirements.txt        # Зависимости Python
├── README.md              # Эта документация
├── .gitignore             # Git исключения
├── assets/                # Ресурсы приложения
│   └── app_icon.svg       # Иконка приложения
├── build_scripts/         # Скрипты сборки
│   ├── build_all.py       # Универсальный билдер
│   ├── build_windows.ps1  # Сборка для Windows
│   ├── build_linux.sh     # Сборка для Linux
│   ├── build_macos.sh     # Сборка для macOS
│   └── create_icon.py     # Генерация иконок
└── dist/                  # Готовые executables (после сборки)
```

## 🎯 Категории команд

| Категория | Описание | Примеры команд |
|-----------|----------|----------------|
| 🖥️ Система | Процессы, службы, информация о системе | ps, top, Get-Process, systemctl |
| 📁 Файлы | Работа с файлами и директориями | ls, cp, mv, Get-ChildItem, tar |
| 🌐 Сеть | Сетевые команды, ping, curl | ping, curl, ipconfig, Test-Connection |
| 📍 Навигация | Перемещение по файловой системе | cd, pwd, Get-Location |
| ⚙️ Процессы | Управление процессами | kill, Start-Process, launchctl |
| 🗂️ Реестр | Работа с реестром (Windows) | Get-ItemProperty, registry keys |
| 🔧 Прочее | Дополнительные команды | date, echo, history, aliases |

## 📝 Примеры команд

### Windows (PowerShell) — 54 команды
| Команда | Описание |
|---------|----------|
| `Get-Process` | Показать запущенные процессы |
| `Get-Service` | Показать работающие службы |
| `Test-Connection google.com` | Ping до Google |
| `Get-NetIPAddress` | IPv4 адреса |
| `Get-EventLog` | Последние события системы |
| `Get-ComputerInfo` | Информация о системе |
| `Get-LocalUser` | Локальные пользователи |
| `Get-PSDrive` | Диски и разделы |
| `Start-Process notepad` | Запустить Блокнот |
| `Get-ItemProperty (Run)` | Автозагрузка программ |

### Linux (bash) — 92 команды
| Команда | Описание |
|---------|----------|
| `ps aux` | Все процессы |
| `top / htop` | Монитор процессов |
| `free -h` | Использование памяти |
| `df -h` | Использование диска |
| `du -sh *` | Размер папок |
| `ping -c 4 google.com` | Ping |
| `curl -I https://example.com` | HTTP запрос |
| `ss -tulpn` | Открытые порты |
| `systemctl status` | Службы systemd |
| `journalctl -b` | Логи загрузки |
| `lscpu` | Информация о CPU |
| `lsblk` | Блочные устройства |

### macOS (Terminal) — 75 команд
| Команда | Описание |
|---------|----------|
| `ps aux` | Все процессы |
| `top -l 1` | Топ процессов |
| `vm_stat` | Виртуальная память |
| `sw_vers` | Версия macOS |
| `system_profiler` | Информация о hardware |
| `diskutil list` | Диски |
| `mdfind` | Поиск Spotlight |
| `networksetup` | Сетевые устройства |
| `launchctl list` | Launchd сервисы |
| `say "Hello"` | Синтез речи |
| `open .` | Открыть в Finder |

## 🛠️ Технологии

- **GUI:** CustomTkinter (современный интерфейс на базе Tkinter)
- **Выполнение команд:** subprocess (асинхронный запуск в потоках)
- **Сборка:** PyInstaller
- **Буфер обмена:** clipboard / pyperclip
- **Кроссплатформенность:** Python 3.8+
- **Терминальная интеграция:** PowerShell (Windows), bash/zsh (Linux/macOS)

## 🚀 Новые возможности (v2.0)

### Реальное выполнение команд
При нажатии на кнопку "▶️ Выполнить":
1. Появляется диалог подтверждения
2. Открывается системный терминал с выполненением команды
3. Результат отображается в терминале
4. Альтернативно — результат показывается в окне приложения

### Расширенная база команд
- **Windows:** 54 команды PowerShell (Система, Файлы, Сеть, Процессы, Реестр)
- **Linux:** 92 команды bash (Система, Файлы, Сеть, Навигация, Процессы)
- **macOS:** 75 команд (Система, Файлы, Сеть, Навигация, Процессы, AppleScript)

### Улучшенный интерфейс
- Счётчик показанных/всего команд
- Всплывающие уведомления при копировании
- Окно результата с подсветкой синтаксиса
- Копирование команды и результата в 1 клик

## 📸 Скриншоты

### Главный экран
```
┌─────────────────────────────────────────┐
│  🖥️ Terminal Commands Guide      🇷🇺 RU │
├─────────────────────────────────────────┤
│                                         │
│     Выберите операционную систему:      │
│                                         │
│         ┌─────────────────┐             │
│         │  🪟 Windows     │             │
│         └─────────────────┘             │
│                                         │
│         ┌─────────────────┐             │
│         │  🐧 Linux       │             │
│         └─────────────────┘             │
│                                         │
│         ┌─────────────────┐             │
│         │  🍎 macOS       │             │
│         └─────────────────┘             │
│                                         │
└─────────────────────────────────────────┘
```

### Экран команд
```
┌─────────────────────────────────────────┐
│ ← Назад  🪟 Windows    🔍 Поиск...      │
├─────────────────────────────────────────┤
│ Категория: [Все категории ▼]            │
├─────────────────────────────────────────┤
│ ┌─────────────────────────────────────┐ │
│ │ Get-Process              📁 Система │ │
│ │ Показать все запущенные процессы    │ │
│ │ ┌─────────────────────────────────┐ │ │
│ │ │ Get-Process  📋 Копировать     │ │ │
│ │ └─────────────────────────────────┘ │ │
│ └─────────────────────────────────────┘ │
└─────────────────────────────────────────┘
```

## 🔧 Требования

- Python 3.8 или выше
- pip (менеджер пакетов Python)
- Для сборки:
  - Windows: PowerShell, опционально WiX Toolset для MSI
  - Linux: dpkg-deb, bash
  - macOS: hdiutil, bash

## 📄 Лицензия

MIT License — свободное использование и модификация.

## 🤝 Вклад

Принимаетесь Pull Request с новыми командами и улучшениями!

## 📧 Контакты

Вопросы и предложения: создайте Issue в репозитории.
