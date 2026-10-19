# DocPerfect v1.0.0 — Ubuntu 22.04 / 24.04 x86_64

**Назначение:** собрать Linux-версию из тех же исходников, что Windows, и проверить её перед публикацией. Фактическая работоспособность сборки должна подтверждаться журналами CI и испытанием на компьютере пользователя.

## Зависимости сборки

- Ubuntu **22.04 x86_64** (базовая сборочная система с glibc 2.35), Python **3.12**, `python3.12 -m venv` и `pip`.
- `dpkg-deb`, `tar`, доступ в интернет для установки Python-зависимостей на этапе сборки.
- Библиотеки Qt/X11: `libxcb-cursor0`, `libxkbcommon-x11-0`, `libgl1`, `libegl1`, `libdbus-1-3`, `libfontconfig1`; при отсутствии остальных модулей Qt установщик apt подтянет необходимые зависимости.

Исходники поддерживают Python 3.12+. **Python не требуется конечному пользователю .deb или portable**, потому что приложение упаковано PyInstaller.

## Linux сборка и проверка

```bash
export QT_QPA_PLATFORM=offscreen
bash scripts/build_ubuntu.sh
bash scripts/test_build_ubuntu.sh
bash scripts/package_release_ubuntu.sh
```

Три независимых бинарника находятся в `dist/DocPerfect/`: `DocPerfect` (GUI), `DocPerfect-CLI`, `DocPerfect-MCP`. Последний использует **stdio**, а не локальный веб-сервер; запускать его надо из совместимого MCP-клиента.

Итоговые файлы:

- `dist/release/v1.0.0/docperfect_1.0.0_amd64.deb` — обычная установка приложения в систему с меню приложений.
- `dist/release/v1.0.0/DocPerfect_1.0.0_Ubuntu_amd64_Portable.tar.gz` — распаковываемая версия.
- `dist/release/v1.0.0/SHA256SUMS_Ubuntu.txt` — контрольные суммы двух файлов.

## Установка из .deb (обычным пользователем)

```bash
sudo apt install ./docperfect_1.0.0_amd64.deb
docperfect                     # графическое окно
docperfect-cli --help          # консольная справка
# MCP должен запускать ИИ-клиент, не запускайте его вручную в обычном терминале.
```

Для MCP в настройках агента укажите `"command": "/usr/bin/docperfect-mcp"` и ограничьте папки через `DOCPERFECT_ALLOWED_ROOTS`, запись оставить `DOCPERFECT_AGENT_WRITES=0` до явного решения владельца данных. См. [AGENTS.md](AGENTS.md).

## Portable без установки

```bash
mkdir -p ~/Applications
tar -xzf DocPerfect_1.0.0_Ubuntu_amd64_Portable.tar.gz -C ~/Applications
~/Applications/DocPerfect/DocPerfect
~/Applications/DocPerfect/DocPerfect-CLI --help
```

Для MCP используйте `~/Applications/DocPerfect/DocPerfect-MCP` (в клиенте лучше абсолютный путь). Если при запуске появляется сообщение об отсутствии `libxcb-cursor.so.0` или `libxkbcommon-x11.so.0`, установите системные библиотеки Qt через `apt`. Не пытайтесь запускать Windows EXE через Wine — у DocPerfect есть нативная Linux-сборка.

## Файлы и удаление

- Настройки пользователя: `${XDG_DATA_HOME:-~/.local/share}/docperfect`.
- Удаление программы: `sudo apt remove docperfect`. Пользовательские DOCX и конфигурацию в домашнем каталоге установщик не удаляет.
- При работе MCP с облачным агентом фрагменты документа могут быть отправлены провайдеру этой модели. Решение об их передаче принимает пользователь/агент.

## Проверка совместимости

GitHub Actions собирает **на Ubuntu 22.04**, затем устанавливает именно собранный `.deb` на **Ubuntu 24.04** и проверяет запуск GUI, CLI, MCP, удаление и сохранность файла пользователя. Эту проверку следует дождаться перед публикацией релиза. Текущий файл исходников не является доказательством, что CI уже запущен.

## Ограничения

- Пакеты подготовлены для **amd64/x86_64**, не ARM64.
- Предпросмотр содержимого — структурный, не пиксельно-точный рендеринг Microsoft Word.
- Некоторые сложные функции DOCX и OOXML не входят в анализ; важные документы проверяйте в офисном редакторе.
- Сборки не имеют цифровой подписи издателя.
