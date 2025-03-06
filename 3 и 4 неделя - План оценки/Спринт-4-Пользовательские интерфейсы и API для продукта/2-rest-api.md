# REST API

## Клиенты REST API

Клиенты REST API:
- сервис "План оценки"
- сервис “Реестр компетенций"
- система кадров
- системы по управлению набором
- сервис "План индивидуального развития"
- SSO GNIVC (single sign-on) 

## Роли

- специалист по обучению;
- сотрудник;
- эксперт.

## Запросы и ответы
### 1. Авторизация в сервисе через SSO GNIVC

**Пример запроса:** <br>
```json
POST /oauth2/token

{
  "login" : "lubitelpiva",
  "password" : "ilovepivo",
  "response_type=token"
  }
```
**Пример ответа:** <br>
```json
{
 "token_type:" "bearer",
 "access_token":"ACCESS_TOKEN",
  "expires_in": 3600,
  "refresh_token": "NEW_REFRESH_TOKEN"
}
```
**Запрос доступен для ролей:** специалист по обучению, сотрудник, эксперт.

### 2. Получение списка компетенций

**Пример запроса:**  
```json
GET /competencies
Authorization: Bearer ACCESS_TOKEN
```

**Пример ответа:**  

```json
{
  "competencies": [
    {
      "competence_id": 1,
      "competence_name": "SQL"
    },
    {
      "competence_id": 2,
      "competence_name": "Java Programming"
    }
  ]
}
```
**Запрос доступен для ролей:** специалист по обучению, эксперт.
### 3. Формирование плана оценки
**Пример запроса:**   
```json
POST /assessment-plans
Authorization: Bearer ACCESS_TOKEN
Content-Type: application/json
{
  "employee_id": 123,
  "applied position" : "Аnalyst"
  "level_id": 1,
  "assessment_format": "test",
  "scheduled_date": "2023-10-01T10:00:00Z"
  "Expert_id”:”12334"
}
```
**Пример ответа:**  
```json
{
  "assessment_plan_id": 789,
  "competencies": [
    {
      "competence_id": 1, 
      "competence_name": "Знание SQL", 
      "grade": 2
    },
    {
      "competence_id": 2, 
      "competence_name": "Знание UML", 
      "grade": 1
    }
  ],
  "message": "План оценки успешно создан."
}
```
**Запрос доступен для ролей:** специалист по обучению.

### 4. Получение истории оценок сотрудника
**Пример запроса:**  
```json
GET /assessments/history/123
Authorization: Bearer ACCESS_TOKEN
```
**Пример ответа:**  
```json
{
  "assessments": [
    {
      "Assessment_id": 1,
      "employee_id": 123,
      "competence_id": 2,
      "finish_date": "2023-03-01T10:00:00Z",
      "scored_points": 85,
       "status": "passed"
    }
  ]
}
```
**Запрос доступен для ролей:** специалист по обучению, сотрудник, эксперт.
### 5. Получение уведомлений для пользователя
**Пример запроса:**  
```json
GET /notifications/
Authorization: Bearer ACCESS_TOKEN
```
**Пример ответа:** 
```json
{
  "notifications": [
    {
      "notification_id": 1,
      "message": "Вам назначена оценка по компетенции SQL на 2023-10-01.",
      "date": "2023-09-25T14:00:00Z"
    }
  ]
}
```
**Запрос доступен для ролей:** специалист по обучению, сотрудник, эксперт.
### 6. Обновление сроков компетенции
**Пример запроса:**  
```json
PATCH /assessment-plans/{assessment_plan_id}
Authorization: Bearer ACCESS_TOKEN
Content-Type: application/json

{
  "new_due_date": "2023-11-01T15:00:00Z"
}
```
**Пример ответа:**  
```json
{
  "message": "Сроки оценки компетенции успешно обновлены."
}
```
**Запрос доступен для ролей:** специалист по обучению.  
### 7. Получение деталей компетенции
**Пример запроса:** 
```json
GET /competencies/{competence_id}
Authorization: Bearer ACCESS_TOKEN
```
**Пример ответа:**  
```json
{
  "competence_id": 1,
  "competence_name": "SQL",
  "employee_id": 123,
  "level_id": 1
}
```
**Запрос доступен для ролей:** специалист по обучению, эксперт.
### 8. Получение списка сотрудников
**Пример запроса:** 
```json
GET /employees
Authorization: Bearer ACCESS_TOKEN
```
**Пример ответа:**  
```json
{
  "employees": [
    {
      "employee_id": 123,
      "name": "Иван",
      "surname": "Иванов",
      "age": 30,
      "current_position": "Разработчик",
      "applied_position": "Старший разработчик",
      "tenure": 5,
      "average_level": "Senior"
    }
  ]
}
```
**Запрос доступен для ролей:** специалист по обучению.
### 9. Добавление новой компетенции сотруднику
**Пример запроса:** 
```json
POST /competencies
Authorization: Bearer ACCESS_TOKEN
Content-Type: application/json
{
  "competence_name": "Python",
  "employee_id": 123,
  "level_id": 1
}
```
**Пример ответа:**  
```json
{
  "competence_id": 3,
  "message": "Компетенция успешно добавлена."
}
```
**Запрос доступен для ролей:** специалист по обучению.
### 10. Обновление информации о сотруднике
**Пример запроса:** 
```json
PUT /employees/{employee_id}
Authorization: Bearer ACCESS_TOKEN
Content-Type: application/json
{
  "name": "Иван",
  "surname": "Иванов",
  "age": 31,
  "current_position": "Ведущий разработчик"
}
```
**Пример ответа:**  
```json
{
  "message": "Информация о сотруднике успешно обновлена."
}
```
**Запрос доступен для ролей:**  специалист по обучению.
### 11. Получение уровней компетенций
**Пример запроса:** 
```json
GET /levels
Authorization: Bearer ACCESS_TOKEN
```
**Пример ответа:** 
```json
{
  "levels": [
    {
      "level_id": 1,
      "level_name": "Beginner",
      "points": 10
    },
    {
      "level_id": 2,
      "level_name": "Intermediate",
      "points": 20
    },
    {
      "level_id": 3,
      "level_name": "Advanced",
      "points": 30
    }
  ]
}
```
**Запрос доступен для ролей:** специалист по обучению, эксперт.  
