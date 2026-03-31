# AUTH

## POST /api/v1/auth/register

### Кто может успешно выполнить запрос
- Любой пользователь

### Нужен access token
- Нет

### Request
```json
{
  "name": "Alexey",
  "email": "example@mail.com",
  "password": "example"
}
```

Обязательные поля:
- name
- email
- password

### Response

#### 201 Created
```json
{
  "id": 1,
  "name": "Alexey",
  "accessToken": "example",
  "refreshToken": "example"
}
```

#### 409 Conflict
```json
{
  "message": "Email уже занят"
}
```

#### 422 Unprocessable Entity
```json
{
  "message": "Ошибка валидации"
}
```

## POST /api/v1/auth/login

### Кто может успешно выполнить запрос
- Любой пользователь с корректными учетными данными зарегистрированного аккаунта

### Нужен access token
- Нет

### Request
```json
{
  "email": "example@mail.com",
  "password": "example"
}
```

Обязательные поля:
- email
- password

### Response

#### 200 OK
```json
{
  "id": 1,
  "name": "Alexey",
  "accessToken": "example",
  "refreshToken": "example"
}
```

#### 401 Unauthorized
```json
{
  "message": "Неверный email или пароль"
}
```

#### 422 Unprocessable Entity
```json
{
  "message": "Ошибка валидации"
}
```

## POST /api/v1/auth/refresh

### Кто может успешно выполнить запрос
- Пользователь с действительным refresh token

### Нужен access token
- Нет

### Request
```json
{
  "refreshToken": "example"
}
```

Обязательные поля:
- refreshToken

### Response

#### 200 OK
```json
{
  "accessToken": "example",
  "refreshToken": "example"
}
```

#### 401 Unauthorized
```json
{
  "message": "Недействительный refresh token"
}
```

#### 422 Unprocessable Entity
```json
{
  "message": "Ошибка валидации"
}
```

## POST /api/v1/auth/logout

### Кто может успешно выполнить запрос
- Авторизованный пользователь с действительным refresh token

### Нужен access token
- Да

### Request
```json
{
  "refreshToken": "example"
}
```

Обязательные поля:
- refreshToken

### Response

#### 204 No Content

#### 401 Unauthorized
```json
{
  "message": "Пользователь не авторизован"
}
```

#### 422 Unprocessable Entity
```json
{
  "message": "Ошибка валидации"
}
```

# USERS

## GET /api/v1/users/me

### Кто может успешно выполнить запрос
- Авторизованный пользователь

### Нужен access token
- Да

### Request
- Нет

### Response

#### 200 OK
```json
{
  "id": 1,
  "name": "Alexey",
  "email": "example@mail.com"
}
```

#### 401 Unauthorized
```json
{
  "message": "Пользователь не авторизован"
}
```

# INVITATIONS

## GET /api/v1/invitations/{invitationCode}

### Кто может успешно выполнить запрос
- Любой пользователь, открывший действительную ссылку-приглашение

### Нужен access token
- Нет

### Path params
- invitationCode

### Request
- Нет

### Response

#### 200 OK
```json
{
  "eventId": 10,
  "title": "Trip",
  "eventDate": "2026-04-10"
}
```

#### 404 Not Found
```json
{
  "message": "Недействительная ссылка-приглашение"
}
```

## GET /api/v1/invitations/{invitationCode}/participants

### Кто может успешно выполнить запрос
- Любой пользователь, открывший действительную ссылку-приглашение

### Нужен access token
- Нет

### Path params
- invitationCode

### Request
- Нет

### Response

#### 200 OK
```json
{
  "items": [
    {
      "id": 101,
      "name": "Alexey",
      "isBound": true
    },
    {
      "id": 102,
      "name": "Sasha"
    }
  ]
}
```

#### 404 Not Found
```json
{
  "message": "Недействительная ссылка-приглашение"
}
```

## GET /api/v1/invitations/{invitationCode}/expenses

### Кто может успешно выполнить запрос
- Любой пользователь, открывший действительную ссылку-приглашение

### Нужен access token
- Нет

### Path params
- invitationCode

### Request
- Нет

### Response

#### 200 OK
```json
{
  "items": [
    {
      "id": 501,
      "title": "Products",
      "amount": 5400,
      "expenseDate": "2026-04-11",
      "payerParticipantId": 101,
      "payerParticipantName": "Alexey",
      "createdByParticipantId": 101,
      "createdByParticipantName": "Alexey"
    }
  ]
}
```

#### 404 Not Found
```json
{
  "message": "Недействительная ссылка-приглашение"
}
```

## GET /api/v1/invitations/{invitationCode}/expenses/{expenseId}

### Кто может успешно выполнить запрос
- Любой пользователь, открывший действительную ссылку-приглашение

### Нужен access token
- Нет

### Path params
- invitationCode
- expenseId

### Request
- Нет

### Response

#### 200 OK
```json
{
  "id": 501,
  "title": "Products",
  "amount": 5400,
  "expenseDate": "2026-04-11",
  "payerParticipantId": 101,
  "payerParticipantName": "Alexey",
  "createdByParticipantId": 101,
  "createdByParticipantName": "Alexey",
  "shares": [
    {
      "participantId": 101,
      "participantName": "Alexey",
      "amount": 2700,
      "description": "За продукты"
    },
    {
      "participantId": 102,
      "participantName": "Sasha",
      "amount": 2700,
      "description": "За продукты"
    }
  ]
}
```

#### 404 Not Found
```json
{
  "message": "Трата не найдена или ссылка-приглашение недействительна"
}
```

## POST /api/v1/invitations/{invitationCode}/participants/bind

### Кто может успешно выполнить запрос
- Авторизованный пользователь, открывший событие по действительной ссылке-приглашению и еще не привязанный к слоту этого события

### Нужен access token
- Да

### Path params
- invitationCode

### Request
```json
{
  "participantId": 102
}
```

Обязательные поля:
- participantId

### Response

#### 200 OK
```json
{
  "id": 102,
  "name": "Sasha"
}
```

#### 401 Unauthorized
```json
{
  "message": "Пользователь не авторизован"
}
```

#### 404 Not Found
```json
{
  "message": "Недействительная ссылка-приглашение"
}
```

#### 404 Not Found
```json
{
  "message": "Слот не найден"
}
```

#### 409 Conflict
```json
{
  "message": "Слот уже занят или пользователь уже привязан к слоту этого события"
}
```

#### 422 Unprocessable Entity
```json
{
  "message": "Ошибка валидации"
}
```

## POST /api/v1/invitations/{invitationCode}/participants/self

### Кто может успешно выполнить запрос
- Авторизованный пользователь, открывший событие по действительной ссылке-приглашению и еще не привязанный к слоту этого события

### Нужен access token
- Да

### Path params
- invitationCode

### Request
```json
{
  "name": "Alexey"
}
```

Обязательные поля:
- name

### Response

#### 201 Created
```json
{
  "id": 103,
  "name": "Alexey"
}
```

#### 401 Unauthorized
```json
{
  "message": "Пользователь не авторизован"
}
```

#### 404 Not Found
```json
{
  "message": "Недействительная ссылка-приглашение"
}
```

#### 409 Conflict
```json
{
  "message": "Пользователь уже привязан к слоту этого события"
}
```

#### 422 Unprocessable Entity
```json
{
  "message": "Ошибка валидации"
}
```

# EVENTS

## GET /api/v1/events/my

### Кто может успешно выполнить запрос
- Авторизованный пользователь

### Нужен access token
- Да

### Что возвращает
- Все события, доступные пользователю:
    - события, где пользователь является создателем
    - события, где пользователь является привязанным участником

### Request
- Нет

### Response

#### 200 OK
```json
{
  "items": [
    {
      "id": 10,
      "title": "Trip",
      "eventDate": "2026-04-10"
    }
  ]
}
```

#### 401 Unauthorized
```json
{
  "message": "Пользователь не авторизован"
}
```

## POST /api/v1/events

### Кто может успешно выполнить запрос
- Авторизованный пользователь

### Нужен access token
- Да

### Request
```json
{
  "title": "Trip",
  "eventDate": "2026-04-10"
}
```

Обязательные поля:
- title
- eventDate

### Response

#### 201 Created
```json
{
  "id": 10,
  "title": "Trip",
  "eventDate": "2026-04-10",
  "invitationCode": "ABC123",
  "invitationLink": "https://app/invite/ABC123"
}
```

#### 401 Unauthorized
```json
{
  "message": "Пользователь не авторизован"
}
```

#### 422 Unprocessable Entity
```json
{
  "message": "Ошибка валидации"
}
```

## GET /api/v1/events/{eventId}

### Кто может успешно выполнить запрос
- Создатель события
- Привязанный участник события

### Нужен access token
- Да

### Path params
- eventId

### Request
- Нет

### Response

#### 200 OK
```json
{
  "id": 10,
  "title": "Trip",
  "eventDate": "2026-04-10",
  "invitationCode": "ABC123",
  "invitationLink": "https://app/invite/ABC123"
}
```

#### 401 Unauthorized
```json
{
  "message": "Пользователь не авторизован"
}
```

#### 403 Forbidden
```json
{
  "message": "Нет доступа к событию"
}
```

#### 404 Not Found
```json
{
  "message": "Событие не найдено"
}
```

## POST /api/v1/events/{eventId}/invitation-link/regenerate

### Кто может успешно выполнить запрос
- Создатель события

### Нужен access token
- Да

### Path params
- eventId

### Request
- Нет

### Response

#### 200 OK
```json
{
  "invitationCode": "XYZ999",
  "invitationLink": "https://app/invite/XYZ999"
}
```

#### 401 Unauthorized
```json
{
  "message": "Пользователь не авторизован"
}
```

#### 403 Forbidden
```json
{
  "message": "Только создатель события может заменить ссылку-приглашение"
}
```

#### 404 Not Found
```json
{
  "message": "Событие не найдено"
}
```

## DELETE /api/v1/events/{eventId}

### Кто может успешно выполнить запрос
- Создатель события

### Нужен access token
- Да

### Path params
- eventId

### Request
- Нет

### Response

#### 204 No Content

#### 401 Unauthorized
```json
{
  "message": "Пользователь не авторизован"
}
```

#### 403 Forbidden
```json
{
  "message": "Только создатель события может удалить событие"
}
```

#### 404 Not Found
```json
{
  "message": "Событие не найдено"
}
```

# PARTICIPANTS

## GET /api/v1/events/{eventId}/participants

### Кто может успешно выполнить запрос
- Создатель события
- Привязанный участник события

### Нужен access token
- Да

### Path params
- eventId

### Request
- Нет

### Response

#### 200 OK
```json
{
  "items": [
    {
      "id": 101,
      "name": "Alexey",
      "linkedUserId": 1
    },
    {
      "id": 102,
      "name": "Sasha",
      "linkedUserId": null
    }
  ]
}
```

#### 401 Unauthorized
```json
{
  "message": "Пользователь не авторизован"
}
```

#### 403 Forbidden
```json
{
  "message": "Нет доступа к событию"
}
```

#### 404 Not Found
```json
{
  "message": "Событие не найдено"
}
```

## POST /api/v1/events/{eventId}/participants/free

### Кто может успешно выполнить запрос
- Привязанный участник события

### Нужен access token
- Да

### Path params
- eventId

### Request
```json
{
  "name": "Dima"
}
```

Обязательные поля:
- name

### Response

#### 201 Created
```json
{
  "id": 104,
  "name": "Dima",
  "linkedUserId": null
}
```

#### 401 Unauthorized
```json
{
  "message": "Пользователь не авторизован"
}
```

#### 403 Forbidden
```json
{
  "message": "Только привязанный участник события может создать свободный слот"
}
```

#### 404 Not Found
```json
{
  "message": "Событие не найдено"
}
```

#### 422 Unprocessable Entity
```json
{
  "message": "Ошибка валидации"
}
```

## POST /api/v1/events/{eventId}/participants/{participantId}/unbind

### Кто может успешно выполнить запрос
- Привязанный участник события, отвязывающийся от своего слота

### Нужен access token
- Да

### Path params
- eventId
- participantId

### Request
- Нет

### Response

#### 200 OK
```json
{
  "id": 103,
  "name": "Alexey",
  "linkedUserId": null
}
```

#### 401 Unauthorized
```json
{
  "message": "Пользователь не авторизован"
}
```

#### 403 Forbidden
```json
{
  "message": "Нельзя отвязаться от этого слота"
}
```

#### 404 Not Found
```json
{
  "message": "Слот не найден"
}
```

## DELETE /api/v1/events/{eventId}/participants/{participantId}

### Кто может успешно выполнить запрос
- Привязанный участник события

### Нужен access token
- Да

### Path params
- eventId
- participantId

### Request
- Нет

### Response

#### 204 No Content

#### 401 Unauthorized
```json
{
  "message": "Пользователь не авторизован"
}
```

#### 403 Forbidden
```json
{
  "message": "Только привязанный участник события может удалить свободный слот"
}
```

#### 404 Not Found
```json
{
  "message": "Слот не найден"
}
```

#### 409 Conflict
```json
{
  "message": "Нельзя удалить занятый слот или слот, участвующий в тратах"
}
```

# EXPENSES

## GET /api/v1/events/{eventId}/expenses

### Кто может успешно выполнить запрос
- Создатель события
- Привязанный участник события

### Нужен access token
- Да

### Path params
- eventId

### Request
- Нет

### Response

#### 200 OK
```json
{
  "items": [
    {
      "id": 501,
      "title": "Products",
      "amount": 5400,
      "expenseDate": "2026-04-11",
      "payerParticipantId": 101,
      "payerParticipantName": "Alexey",
      "createdByParticipantId": 101,
      "createdByParticipantName": "Alexey"
    }
  ]
}
```

#### 401 Unauthorized
```json
{
  "message": "Пользователь не авторизован"
}
```

#### 403 Forbidden
```json
{
  "message": "Нет доступа к событию"
}
```

#### 404 Not Found
```json
{
  "message": "Событие не найдено"
}
```

## GET /api/v1/events/{eventId}/expenses/{expenseId}

### Кто может успешно выполнить запрос
- Создатель события
- Привязанный участник события

### Нужен access token
- Да

### Path params
- eventId
- expenseId

### Request
- Нет

### Response

#### 200 OK
```json
{
  "id": 501,
  "title": "Products",
  "amount": 5400,
  "expenseDate": "2026-04-11",
  "payerParticipantId": 101,
  "payerParticipantName": "Alexey",
  "createdByParticipantId": 101,
  "createdByParticipantName": "Alexey",
  "shares": [
    {
      "participantId": 101,
      "participantName": "Alexey",
      "amount": 2700,
      "description": "За продукты"
    },
    {
      "participantId": 102,
      "participantName": "Sasha",
      "amount": 2700,
      "description": "За продукты"
    }
  ]
}
```

#### 401 Unauthorized
```json
{
  "message": "Пользователь не авторизован"
}
```

#### 403 Forbidden
```json
{
  "message": "Нет доступа к событию"
}
```

#### 404 Not Found
```json
{
  "message": "Трата не найдена"
}
```

## POST /api/v1/events/{eventId}/expenses

### Кто может успешно выполнить запрос
- Привязанный участник события

### Нужен access token
- Да

### Path params
- eventId

### Request
```json
{
  "title": "Products",
  "amount": 5400,
  "expenseDate": "2026-04-11",
  "payerParticipantId": 101,
  "shares": [
    {
      "participantId": 101,
      "amount": 2700,
      "description": "За продукты"
    },
    {
      "participantId": 102,
      "amount": 2700,
      "description": "За продукты"
    }
  ]
}
```

Обязательные поля:
- title
- amount
- expenseDate
- payerParticipantId
- shares

Дополнительно:
- `shares` должен быть непустым массивом
- `payerParticipantId` должен относиться к участнику этого события
- каждый `participantId` в `shares` должен относиться к участнику этого события
- сумма всех долей в `shares` должна быть равна `amount`

### Response

#### 201 Created
```json
{
  "id": 501,
  "title": "Products",
  "amount": 5400,
  "expenseDate": "2026-04-11",
  "payerParticipantId": 101,
  "payerParticipantName": "Alexey",
  "createdByParticipantId": 101,
  "createdByParticipantName": "Alexey",
  "shares": [
    {
      "participantId": 101,
      "participantName": "Alexey",
      "amount": 2700,
      "description": "За продукты"
    },
    {
      "participantId": 102,
      "participantName": "Sasha",
      "amount": 2700,
      "description": "За продукты"
    }
  ]
}
```

#### 401 Unauthorized
```json
{
  "message": "Пользователь не авторизован"
}
```

#### 403 Forbidden
```json
{
  "message": "Только привязанный участник события может создать трату"
}
```

#### 404 Not Found
```json
{
  "message": "Событие не найдено"
}
```

#### 422 Unprocessable Entity
```json
{
  "message": "Плательщик не относится к этому событию"
}
```

#### 422 Unprocessable Entity
```json
{
  "message": "Один или несколько участников долей не относятся к этому событию"
}
```

#### 422 Unprocessable Entity
```json
{
  "message": "Сумма долей не равна сумме траты"
}
```

#### 422 Unprocessable Entity
```json
{
  "message": "У траты должна быть хотя бы одна доля"
}
```

#### 422 Unprocessable Entity
```json
{
  "message": "Ошибка валидации"
}
```

## DELETE /api/v1/events/{eventId}/expenses/{expenseId}

### Кто может успешно выполнить запрос
- Создатель траты
- Создатель события

### Нужен access token
- Да

### Path params
- eventId
- expenseId

### Request
- Нет

### Response

#### 204 No Content

#### 401 Unauthorized
```json
{
  "message": "Пользователь не авторизован"
}
```

#### 403 Forbidden
```json
{
  "message": "Удалить трату может только ее создатель или создатель события"
}
```

#### 404 Not Found
```json
{
  "message": "Трата не найдена"
}
```

# BALANCES

## GET /api/v1/events/{eventId}/balances/me

### Кто может успешно выполнить запрос
- Привязанный участник события

### Нужен access token
- Да

### Path params
- eventId

### Request
- Нет

### Response

#### 200 OK
```json
{
  "owedToMe": [
    {
      "participantId": 102,
      "participantName": "Sasha",
      "amount": 2700
    }
  ],
  "iOwe": [
    {
      "participantId": 103,
      "participantName": "Dima",
      "amount": 900
    }
  ],
  "totalOwedToMe": 2700,
  "totalIOwe": 900,
  "netBalance": 1800
}
```

#### 401 Unauthorized
```json
{
  "message": "Пользователь не авторизован"
}
```

#### 403 Forbidden
```json
{
  "message": "Персональный расчет долгов доступен только привязанному участнику события"
}
```

#### 404 Not Found
```json
{
  "message": "Событие не найдено"
}
```