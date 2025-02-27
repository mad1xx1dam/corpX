# CorporationX

**CorporationX** — микросервисное веб-приложение для управления стартапами и их социальной активностью. Система состоит из нескольких независимых сервисов, каждый из которых отвечает за свою функциональность, и общей инфраструктуры (`infra`) для баз данных и хранилищ.

## Сервисы и репозитории

1. **user_service**: Управление пользователями (персональные данные, менторство и другой функционал)
   - Репозиторий: [https://github.com/mad1xx1dam/user_service](https://github.com/mad1xx1dam/user_service)  
   - Порт: `8080`
2. **project_service**: Управление проектами (стажировки, стадии проектов, подпроекты)
   - Репозиторий: [https://github.com/mad1xx1dam/project_service](https://github.com/mad1xx1dam/project_service)  
   - Порт: `8082`
3. **post_service**: Управление постами
   - Репозиторий: [https://github.com/mad1xx1dam/post_service](https://github.com/mad1xx1dam/post_service)  
   - Порт: `8081`
4. **analytics_service**: Аналитика активности (отслеживание количества действий пользователей)
   - Репозиторий: [https://github.com/mad1xx1dam/analytics_service](https://github.com/mad1xx1dam/analytics_service)  
   - Порт: `8086`
5. **achievement_service**: Управление достижениями (за различные действия в системе)
   - Репозиторий: [https://github.com/mad1xx1dam/achievement_service](https://github.com/mad1xx1dam/achievement_service)  
   - Порт: `8085`
6. **payment_service**: Платежи  
   - Репозиторий: [https://github.com/mad1xx1dam/payment_service](https://github.com/mad1xx1dam/payment_service)  
   - Порт: `9080`
7. **notification_service**: Уведомления (telegram, sms voyage, email)
   - Репозиторий: [https://github.com/mad1xx1dam/notification_service](https://github.com/mad1xx1dam/notification_service)  
   - Порт: `8083`
8. **account_service**: Управление счетами (накопительные и т.п.)  
   - Репозиторий: [https://github.com/mad1xx1dam/account_service](https://github.com/mad1xx1dam/account_service)  
   - Порт: `8090`
9. **url_shortener_service**: Сокращение URL (длинная ссылка --> короткая ссылка)
   - Репозиторий: [https://github.com/mad1xx1dam/url_shortener_service](https://github.com/mad1xx1dam/url_shortener_service)  
   - Порт: `8084`
10. **infra**: Инфраструктура (PostgreSQL, Redis, Minio и др.)  
    - Репозиторий: [https://github.com/mad1xx1dam/infra](https://github.com/mad1xx1dam/infra)

## Требования

Для запуска сервисов без IDE нужен минимальный набор инструментов:

- **Docker**: Для контейнеризации и запуска сервисов.
  - Установка:  
    - Windows/macOS: [Docker Desktop](https://www.docker.com/products/docker-desktop/)  
    - Linux: `sudo apt install docker.io docker-compose`
  - Проверить: `docker --version`
- **Gradle**: Для сборки JAR-файлов (используется Gradle Wrapper, установка не требуется).
  - Проверить наличие: `ls gradlew` (Linux/macOS) или `dir gradlew.bat` (Windows)
- **Git**: Для клонирования репозиториев.
  - Установка:  
    - Linux: `sudo apt install git`  
    - Windows/macOS: [Git](https://git-scm.com/downloads)  
  - Проверить: `git --version`

**Примечание**: Java 17 автоматически загружается Gradle Wrapper’ом, но для ускорения можно установить вручную:
- Linux: `sudo apt install openjdk-17-jdk`
- Windows/macOS: [AdoptOpenJDK](https://adoptium.net/)

## Запуск без IDE

### 1. Поднять инфраструктуру
```bash
git clone https://github.com/mad1xx1dam/infra.git
cd infra
docker compose up -d
```

### 2. Склонировать и запустить нужный сервис
```bash
git clone https://github.com/mad1xx1dam/user_service.git
cd user_service
./gradlew build -x test  # Linux/macOS
gradlew.bat build -x test  # Windows
docker compose up -d
```

### 3. Проверка работы
```bash
curl -X GET -H "x-user-id: 1" http://localhost:<port>/api/v1/<endpoint>
```
Замените `<port>` на порт сервиса (см. список выше) и `<endpoint>` на нужный путь (например, `users/1`).

## Запуск с IDE (IntelliJ IDEA)

### 1. Поднять инфраструктуру
```bash
git clone https://github.com/mad1xx1dam/infra.git
cd infra
docker compose up -d
```

### 2. Склонировать и открыть нужный сервис
```bash
git clone https://github.com/mad1xx1dam/<service>.git
cd <service>
```
- Открыть в **IntelliJ IDEA**: `File > Open > <папка сервиса>` (импорт как Gradle-проект).
- Собрать без тестов:
  ```bash
  ./gradlew build -x test
  ```
- Запустить сервис:
  ```bash
  docker compose up -d
  ```

### 3. Проверка работы
В Postman: `http://localhost:<port>/api/v1/<endpoint>` с заголовком `x-user-id: 1`.

## Дополнительно

### Остановка всех контейнеров
```bash
docker compose down
```

### Просмотр логов
```bash
docker compose logs app
```

