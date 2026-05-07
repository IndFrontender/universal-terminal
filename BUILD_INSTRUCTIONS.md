# 📦 Build Instructions / Инструкции по сборке

## ✅ Windows - ГОТОВО

### Созданные файлы:
- `dist/TerminalCommandsGuide.exe` (25.92 MB) - портативный исполняемый файл
- `dist/TerminalCommandsGuide-Portable.zip` (25.63 MB) - ZIP архив

### Сборка EXE (выполнено):
```bash
cd terminal_commands_app
pip install -r requirements.txt
pyinstaller --onefile --windowed --name "TerminalCommandsGuide" main.py
```

### Создание MSI (опционально):

**Вариант 1: Inno Setup (рекомендуется)**
```powershell
# 1. Скачать и установить: https://jrsoftware.org/isinfo.php
# 2. Выполнить:
"C:\Program Files (x86)\Inno Setup 6\ISCC.exe" build_scripts\create_msi.iss
```

**Вариант 2: WiX Toolset**
```powershell
# 1. Скачать: https://wixtoolset.org/
# 2. Установить WiX Toolset
# 3. Выполнить:
candle.exe build_scripts\installer.wxs
light.exe installer.wixobj -o dist\TerminalCommandsGuide.msi
```

**Вариант 3: Advanced Installer (GUI)**
- Скачать: https://www.advancedinstaller.com/
- Создать новый проект, добавить EXE файл
- Собрать MSI через графический интерфейс

---

## 🐧 Linux

### Требования:
- Python 3.8+
- PyInstaller
- dpkg-deb (для .deb)
- `python3-customtkinter`, `clipboard`, `pyperclip` будут подтянуты при установке пакета через `postinst`, если их нет в системе

### Сборка:
```bash
cd terminal_commands_app

# 1. Установка зависимостей
pip install -r requirements.txt

# 2. Сборка EXE
pyinstaller --onefile --windowed --name "TerminalCommandsGuide" main.py

# 3. Создание .deb пакета
chmod +x build_scripts/build_linux.sh
./build_scripts/build_linux.sh
```

### Результат:
- `dist/TerminalCommandsGuide` - исполняемый файл
- `dist/terminal-commands-guide_2.0_amd64.deb` - DEB пакет

### Установка DEB:
```bash
sudo dpkg -i dist/terminal-commands-guide_2.0_amd64.deb
```

---

## 🍎 macOS

### Требования:
- Python 3.8+
- PyInstaller
- Xcode Command Line Tools
- hdiutil (встроен в macOS)

### Сборка:
```bash
cd terminal_commands_app

# 1. Установка зависимостей
pip install -r requirements.txt

# 2. Сборка APP
pyinstaller --onefile --windowed --name "TerminalCommandsGuide" main.py

# 3. Создание .dmg
chmod +x build_scripts/build_macos.sh
./build_scripts/build_macos.sh
```

### Результат:
- `dist/TerminalCommandsGuide.app` - приложение
- `dist/TerminalCommandsGuide.dmg` - DMG образ

---

## 📊 Сравнение форматов

| Платформа | Формат | Размер | Преимущества |
|-----------|--------|--------|--------------|
| **Windows** | EXE | 26 MB | Портативный, не требует установки |
| **Windows** | MSI | ~30 MB | Стандартный установщик, реестр |
| **Windows** | ZIP | 26 MB | Архив, можно распаковать куда угодно |
| **Linux** | DEB | ~28 MB | Интеграция с системой, apt |
| **Linux** | Binary | 25 MB | Портативный бинарник |
| **macOS** | DMG | ~30 MB | Стандартный формат macOS |
| **macOS** | APP | 25 MB | Приложение в.bundle формате |

---

## 🔧 Автоматическая сборка

### Windows:
```powershell
.\build_scripts\build_windows_simple.ps1
```

### Linux:
```bash
./build_scripts/build_linux.sh --auto
```

Автономная сборка Linux:
```bash
./build_scripts/build_linux.sh --static
```

### macOS:
```bash
./build_scripts/build_macos.sh
```

### Все платформы (универсальный):
```bash
python build_scripts/build_all.py
```

---

## 📝 Примечания

1. **Размер файлов**: EXE/APP файлы содержат Python interpreter, поэтому размер ~25-30 MB
2. **Антивирусы**: PyInstaller EXE могут ложно детектироваться - добавьте в исключения
3. **Тестирование**: Проверяйте собранные файлы на чистой системе
4. **Подписывание**: Для распространения подпишите код сертификатом

---

## 🐛 Troubleshooting

### Windows: "API-MS-WIN DLL not found"
```powershell
# Переустановите Visual C++ Redistributable
# https://aka.ms/vs/17/release/vc_redist.x64.exe
```

### Linux: "pyinstaller not found"
```bash
pip3 install pyinstaller
export PATH=$PATH:~/.local/bin
```

### macOS: "Developer cannot be verified"
```bash
# System Preferences → Security & Privacy → Open Anyway
# Или:
xattr -cr /Applications/TerminalCommandsGuide.app
```

---

## 📞 Support

- GitHub Issues: [создайте issue]
- Email: support@terminalcommands.app
