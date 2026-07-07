# Инструкция по установке и настройке

## Системные требования
- Windows 10/11 (Home/Pro) или macOS/Linux
- Docker Desktop с WSL 2 (для Windows)
- Node.js 18+ и npm
- Git

---

## 1. Установка Docker Desktop
### Для Windows
1. Скачай Docker Desktop с [официального сайта](https://www.docker.com/products/docker-desktop/).
2. Установи и запусти.
3. Убедись, что в настройках включена опция **"Use WSL 2 based engine"** (Settings → General).

**Если при запуске Docker появляется ошибка "Virtualization support not detected":**
> 1. Нажми `Ctrl + Shift + Esc`, чтобы открыть Диспетчер задач.
> 2. Перейди на вкладку **"Производительность"**.
> 3. В правом нижнем углу найди строку **"Виртуализация:"**.
>    - Если там написано **"Включена"** — отлично, пропускай шаг с BIOS.
>    - Если там **"Отключена"**, то, скорее всего, проблема в настройках BIOS. В этом случае:
>      1. Перезагрузи компьютер и войди в BIOS/UEFI (обычно клавиши `F2`, `Del`, `Esc` во время загрузки).
>      2. Найди настройки процессора (разделы `Advanced` или `CPU Configuration`).
>      3. Включи опцию:
>         - Для Intel: `Intel Virtualization Technology` (VT-x)
>         - Для AMD: `SVM Mode` (AMD-V)
>      4. Сохрани настройки и перезагрузись.
> 4. Включи компоненты Windows:
>    - Нажми `Win + R` → введи `optionalfeatures` → Enter.
>    - Поставь галочки напротив:
>      - `Virtual Machine Platform` (Платформа виртуальных машин)
>      - `Windows Subsystem for Linux` (Подсистема Windows для Linux)
>      - `Windows Hypervisor Platform` (Платформа гипервизора Windows)
> 5. Перезагрузи компьютер.
> 6. Если ошибка осталась:
>    - установи ядро WSL 2 вручную: [ссылка](https://learn.microsoft.com/ru-ru/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package)
>    - выполни: `wsl --set-default-version 2`
>    - перезагрузи компьютер

### Для macOS
1. Скачай Docker Desktop с [официального сайта](https://www.docker.com/products/docker-desktop/).
2. Установи и запусти.
3. Docker работает "из коробки", дополнительных настроек не требует.

### Для Linux
Установи Docker и Docker Compose через пакетный менеджер:
```bash
sudo apt update
sudo apt install docker.io docker-compose
sudo systemctl start docker
sudo systemctl enable docker
```

---

## 2. Установка Node.js и npm
1. Скачай и установи Node.js с [официального сайта](https://nodejs.org/) (рекомендуется LTS-версия).
2. Проверь установку в командной строке:
```bash
node --version   # должно показать v18.x.x или выше
npm --version    # должно показать версию npm
```

---

## 3. Клонирование репозитория
```bash
git clone https://github.com/EgorDanilovv/qwen-code-gigachat-case5.git
cd qwen-code-gigachat-case5
```

---

## 4. Настройка прокси gpt2giga
### 4.1. Создай файл `.env`
Скопируй пример конфигурации:
```bash
# Windows
copy .env.example .env

# Linux/macOS
cp .env.example .env
```

### 4.2. Отредактируй `.env`
Открой файл `.env` и впиши свой ключ авторизации

> **Где взять GIGACHAT_CREDENTIALS?**
> - Зарегистрируйся на [developers.sber.ru](https://developers.sber.ru)
> - Создай проект и получи ключ

### 4.3. Запусти прокси
```bash
docker compose up -d
```

### 4.4. Проверь, что прокси работает
Открой браузер и перейди по адресу: `http://localhost:8090`
- Если видишь ответ (даже ошибку 404) — прокси работает.
- Если страница не загружается — проверь логи: `docker compose logs`

---

## 5. Установка и настройка Qwen Code
### 5.1. Установи Qwen Code глобально
```bash
npm install -g qwen-code
```

### 5.2. При первом запуске:
1. Выбери **Custom Provider**.
2. Укажи `http://localhost:8090` как базовый URL.
3. API-ключ можно пропустить (просто нажми Enter)
4. Введи `GigaChat-2-Max` как имя модели.
5. Выбери эту модель для работы.

---

## 6. Проверка работоспособности
Задай любой вопрос модели, например:
```
"Напиши функцию для сложения двух чисел на JavaScript"
```

Если ты видишь осмысленный ответ — всё работает! 🎉