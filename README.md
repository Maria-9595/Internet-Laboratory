# REST API для работы с пользователями на Symfony 6.2

## Установка

```bash
composer install
```

Настройте подключение к базе данных в файле `.env`:

```
DATABASE_URL="mysql://db_user:db_password@127.0.0.1:3306/db_name?serverVersion=8.0"
```

Настройте JWT:

```bash
mkdir -p config/jwt
openssl genrsa -out config/jwt/private.pem -aes256 4096
openssl rsa -pubout -in config/jwt/private.pem -out config/jwt/public.pem
```

Добавьте пароль для JWT в `.env`:

```
JWT_PASSPHRASE=your_passphrase_here
```

Выполните миграции:

```bash
php bin/console doctrine:database:create
php bin/console doctrine:migrations:migrate
```

Запустите сервер:

```bash
symfony server:start
```

## API Endpoints

### Создание пользователя

```
POST /api/users
```

Запрос:
```json
{
  "email": "test@test.ru",
  "plainPassword": "password123",
  "name": "Maria Yureva"
}
```

Ответ:
```json
{
  "id": 1,
  "email": "test@test.ru",
  "name": "Maria Yureva",
  "roles": ["ROLE_USER"]
}
```

### Авторизация пользователя

```
POST /api/login
```

Запрос:
```json
{
  "email": "test@test.ru",
  "password": "password123"
}
```

Ответ:
```json
{
  "token": "...",
  "user": {
    "id": 1,
    "email": "test@test.ru",
    "name": "Maria Yureva",
    "roles": ["ROLE_USER"]
  }
}
```

### Получение информации о пользователе

```
GET /api/users/{id}
```

Заголовки:
```
Authorization: Bearer {token}
```

Ответ:
```json
{
  "id": 1,
  "email": "test@test.ru",
  "name": "Maria Yureva",
  "roles": ["ROLE_USER"]
}
```

### Получение информации о текущем пользователе

```
GET /api/users/me
```

Заголовки:
```
Authorization: Bearer {token}
```

Ответ:
```json
{
  "id": 1,
  "email": "test@test.ru",
  "name": "Maria Yureva",
  "roles": ["ROLE_USER"]
}
```

### Обновление пользователя

```
PUT /api/users/{id}
```

Заголовки:
```
Authorization: Bearer {token}
```

Запрос:
```json
{
  "email": "test1@test.ru",
  "name": "Manuna Yureva",
  "plainPassword": "password321"
}
```

Ответ:
```json
{
  "id": 1,
  "email": "test1@test.ru",
  "name": "Manuna Yureva",
  "roles": ["ROLE_USER"]
}
```

### Удаление пользователя

```
DELETE /api/users/{id}
```

Заголовки:
```
Authorization: Bearer {token}
```

Ответ: 204 No Content
