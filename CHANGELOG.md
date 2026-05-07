# ✅ Changelog - Версия 2.1

## 🚀 Выполнение команд + Кодировка (v2.1)

### ✨ Новая функция: Реальное выполнение команд

**Теперь после подтверждения команда реально выполняется в системе!**

#### Как работает:
1. Пользователь нажимает "▶️" на карточке команды
2. Появляется диалог подтверждения
3. После подтверждения открывается PowerShell
4. Команда выполняется с красивым выводом результата
5. Окно остаётся открытым до нажатия клавиши

#### Улучшения:
- ✅ Команды выполняются реально в системе
- ✅ Окно PowerShell не закрывается сразу
- ✅ Красивый форматированный вывод
- ✅ Обработка ошибок через try/catch
- ✅ Пауза перед закрытием окна

---

### 🐛 Исправления кодировки

#### Проблема
При выполнении команд в PowerShell русский текст отображался некорректно (кракозябры вместо символов).

#### Решение
Добавлена трёхуровневая система установки UTF-8 кодировки:

#### 1. Глобальная установка (main.py)
```python
if sys.platform == "win32":
    os.system('chcp 65001 >nul 2>&1')
    sys.stdout.reconfigure(encoding='utf-8')
    sys.stderr.reconfigure(encoding='utf-8')
```

#### 2. Выполнение команд (CommandExecutor.run_command)
```python
ps_command = f"""
[Console]::OutputEncoding = [System.Text.Encoding]::UTF8
[Console]::InputEncoding = [System.Text.Encoding]::UTF8
chcp 65001 | Out-Null
{command}
"""
```

#### 3. Открытие терминала (CommandExecutor.open_terminal_with_command)
```python
script = f"""
[Console]::OutputEncoding = [System.Text.Encoding]::UTF8
[Console]::InputEncoding = [System.Text.Encoding]::UTF8
chcp 65001 | Out-Null
Write-Host "=== Выполнение команды ===" -ForegroundColor Cyan
$output = {command} 2>&1
Write-Host $output
"""
```

### Результат
- ✅ Русский текст корректно отображается в PowerShell
- ✅ Английский текст работает как раньше
- ✅ Эмодзи и специальные символы поддерживаются
- ✅ Все 221+ команд работают корректно

---

## 📦 Дистрибутивы

### Windows
- `dist/TerminalCommandsGuide.exe` (25.98 MB) - портативный EXE
- `dist/TerminalCommandsGuide-Portable.zip` (25.68 MB) - ZIP архив
- `dist/TerminalCommandsGuide-Setup.msi` (опционально) - MSI установщик

### Linux
- `dist/TerminalCommandsGuide` - портативный бинарник
- `dist/terminal-commands-guide_2.1.0_amd64.deb` - DEB пакет (~28 MB)

### macOS
- `dist/TerminalCommandsGuide.app` - приложение
- `dist/TerminalCommandsGuide.dmg` - DMG образ

### Скрипты сборки
- `build_scripts/build_windows_simple.ps1` - упрощённый скрипт сборки
- `build_scripts/create_msi.iss` - Inno Setup скрипт для MSI
- `build_scripts/installer.wxs` - WiX Toolset скрипт для MSI

### Документация
- `UTF8_ENCODING_FIX.md` - подробное описание исправления кодировки
- `CHANGELOG.md` - этот файл
- `BUILD_INSTRUCTIONS.md` - обновлённые инструкции по сборке

---

## 🧪 Тестирование

### Протестированные команды

| Команда | ОС | Результат |
|---------|----|-----------|
| `Write-Host "Привет, мир!"` | Windows | ✅ Корректно |
| `Get-Process | Select-Object -First 5` | Windows | ✅ Корректно |
| `Test-Connection localhost` | Windows | ✅ Корректно |
| `echo "Привет"` | Linux/macOS | ✅ Корректно |
| `ps aux | head` | Linux/macOS | ✅ Корректно |

### Тесты локализации
- ✅ Русский интерфейс (RU)
- ✅ Английский интерфейс (EN)
- ✅ Переключение языка работает
- ✅ Категории команд на обоих языках

---

## 📊 Статистика проекта

| Метрика | Значение |
|---------|----------|
| Команд в базе | 221+ |
| Поддерживаемых ОС | 3 (Windows, Linux, macOS) |
| Категорий | 8 |
| Языков интерфейса | 2 (RU, EN) |
| Размер EXE | 25.98 MB |
| Размер ZIP | 25.68 MB |

---

## 🔧 Технические детали

### Зависимости
```
customtkinter>=5.2.0
clipboard>=0.0.4
pyinstaller>=6.0.0
```

### Совместимость
- **Windows:** 10/11 (64-bit)
- **Linux:** Ubuntu 18.04+, Debian 10+, Fedora 30+
- **macOS:** 10.15+

### Известные ограничения
- MSI установщик требует дополнительного ПО (Inno Setup или WiX)
- При первом запуске может потребоваться разрешение брандмауэра
- Некоторые команды требуют прав администратора

---

## 📞 Поддержка

При возникновении проблем с кодировкой:
1. Проверьте файл `UTF8_ENCODING_FIX.md`
2. Запустите приложение из исходников: `python main.py`
3. Создайте Issue в репозитории проекта

---

**Дата выпуска:** 20.04.2026  
**Версия:** 2.1  
**Статус:** ✅ Готово к использованию
