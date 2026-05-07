# 🔧 Исправление кодировки UTF-8

## Проблема
При выполнении команд в PowerShell русский текст отображался некорректно (кракозябры вместо символов).

## Решение

### 1. Глобальная установка UTF-8 в main.py
```python
if sys.platform == "win32":
    os.system('chcp 65001 >nul 2>&1')
    sys.stdout.reconfigure(encoding='utf-8')
    sys.stderr.reconfigure(encoding='utf-8')
```

### 2. Кодировка в CommandExecutor.run_command()
```python
ps_command = f"""
[Console]::OutputEncoding = [System.Text.Encoding]::UTF8
[Console]::InputEncoding = [System.Text.Encoding]::UTF8
chcp 65001 | Out-Null
{command}
"""
```

### 3. Кодировка в CommandExecutor.open_terminal_with_command()
```python
script = f"""
# Устанавливаем кодировку UTF-8
[Console]::OutputEncoding = [System.Text.Encoding]::UTF8
[Console]::InputEncoding = [System.Text.Encoding]::UTF8
chcp 65001 | Out-Null

Write-Host "=== Выполнение команды ===" -ForegroundColor Cyan
$output = {command} 2>&1
Write-Host "`n=== Результат ===" -ForegroundColor Green
Write-Host $output
"""
```

## Что делает каждое исправление

| Исправление | Описание |
|-------------|----------|
| `chcp 65001` | Устанавливает кодовую страницу UTF-8 в консоли Windows |
| `[Console]::OutputEncoding` | Устанавливает кодировку вывода для PowerShell |
| `[Console]::InputEncoding` | Устанавливает кодировку ввода для PowerShell |
| `sys.stdout.reconfigure()` | Перекодирует stdout Python в UTF-8 |
| `encoding='utf-8'` | Указывает кодировку для subprocess.Popen |

## Тестирование

### Тест 1: Команда с русским текстом
```powershell
Write-Host "Привет, мир!"
```
**Ожидаемый результат:** "Привет, мир!" отображается корректно

### Тест 2: Системная информация
```powershell
Get-ComputerInfo | Select-Object WindowsProductName, WindowsVersion
```
**Ожидаемый результат:** Русские символы в названии системы

### Тест 3: Файлы с русскими именами
```powershell
Get-ChildItem | Where-Object Name -like "*русский*"
```
**Ожидаемый результат:** Файлы с русскими именами отображаются корректно

## Совместимость

| Версия Windows | Кодировка по умолчанию | После исправления |
|----------------|------------------------|-------------------|
| Windows 10 | CP866 (кириллица) | UTF-8 (65001) |
| Windows 11 | CP866 или UTF-8 | UTF-8 (65001) |
| Windows 8.1 | CP866 | UTF-8 (65001) |

## Примечания

1. **Производительность**: Установка UTF-8 не влияет на производительность
2. **Совместимость**: Все команды работают как раньше + корректный русский текст
3. **Linux/macOS**: Не требуется, там UTF-8 по умолчанию

## Если проблемы остались

### Проблема: Всё ещё видны кракозябры
**Решение**: Проверьте шрифт в консоли PowerShell
```powershell
# Установите шрифт Consolas или Lucida Console
# Правый клик на заголовке окна → Свойства → Шрифт
```

### Проблема: Некоторые команды не работают
**Решение**: Возможно команда использует специфичную кодировку
```python
# Попробуйте выполнить без установки кодировки
process = subprocess.Popen(
    ["powershell", "-Command", command],
    encoding='cp866'  # Старая кодировка
)
```

## Дополнительные ресурсы

- [PowerShell Encoding Documentation](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_character_encoding)
- [Windows Code Pages](https://docs.microsoft.com/en-us/windows/win32/intl/code-page-identifiers)
- [Python sys.stdout.reconfigure](https://docs.python.org/3/library/io.html#io.TextIOWrapper.reconfigure)
