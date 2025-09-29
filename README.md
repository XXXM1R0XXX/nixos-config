# Минимальная конфигурация nix-darwin

Простая и чистая конфигурация nix-darwin для macOS с минимальным набором пакетов и приложений.

## Установка с нуля на голом macOS

### 1. Установка Nix

Установите Nix используя официальный установщик:

```bash
curl --proto '=https' --tlsv1.2 -sSf -L https://install.determinate.systems/nix | sh -s -- install
```

После установки перезагрузите терминал или выполните:

```bash
source /nix/var/nix/profiles/default/etc/profile.d/nix-daemon.sh
```

### 2. Установка Homebrew

Установите Homebrew для GUI приложений:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### 3. Клонирование конфигурации

Склонируйте этот репозиторий:

```bash
git clone https://github.com/XXXM1R0XXX/nixos-config.git
cd nixos-config
```

### 4. Настройка конфигурации

Отредактируйте `flake.nix` и замените плейсхолдеры:

```bash
# Замените эти значения на ваши собственные
__USERNAME__     # ваше имя пользователя macOS
__USEREMAIL__    # ваш email
__HOSTNAME__     # имя вашего компьютера  
__SYSTEM__       # aarch64-darwin для Apple Silicon или x86_64-darwin для Intel
```

Также обновите `hostname` в файле `Justfile`:

```bash
# В Justfile замените:
hostname := "your-hostname"  # на имя вашего хоста
```

### 5. Установка just

Установите `just` для упрощения команд:

```bash
nix-shell -p just
```

### 6. Применение конфигурации

Примените конфигурацию:

```bash
just darwin
```

При первом запуске может потребоваться ввести пароль администратора.

## Что включено в минимальную конфигурацию

### Системные пакеты (через Nix):
- neovim - текстовый редактор
- git - система контроля версий  
- just - утилита для запуска команд

### Пользовательские пакеты (через home-manager):
- zip/unzip - архиваторы
- ripgrep - поиск по файлам
- jq - обработка JSON
- fzf - нечеткий поиск
- file, which, tree - базовые утилиты
- eza - современная замена ls

### GUI приложения (через Homebrew):
- Firefox - веб-браузер
- Visual Studio Code - редактор кода
- Xcode - среда разработки (из App Store)

### Утилиты командной строки (через Homebrew):
- wget - загрузчик файлов
- curl - HTTP клиент

## Полезные команды

```bash
# Посмотреть все доступные команды
just

# Обновить все пакеты
just up

# Очистить старые версии
just clean
just gc

# Форматировать nix файлы
just fmt
```

## Структура конфигурации

```
.
├── flake.nix           # основной файл конфигурации
├── Justfile           # команды для управления
├── modules/           # модули nix-darwin
│   ├── apps.nix       # приложения и пакеты
│   ├── host-users.nix # настройка хоста и пользователей
│   ├── nix-core.nix   # базовые настройки nix
│   └── system.nix     # настройки системы macOS
└── home/              # конфигурация home-manager
    ├── default.nix    # точка входа
    ├── core.nix       # базовые пакеты пользователя
    ├── git.nix        # настройки git
    ├── shell.nix      # настройки shell
    └── starship.nix   # настройки prompt
```

## Настройка под себя

Вы можете легко расширить конфигурацию:

1. Добавить системные пакеты в `modules/apps.nix`
2. Добавить пользовательские пакеты в `home/core.nix`  
3. Добавить GUI приложения в секцию `homebrew.casks` в `modules/apps.nix`
4. Изменить системные настройки в `modules/system.nix`

После изменений выполните `just darwin` для применения конфигурации.