# Архитектура безопасности
## **Введение:**
При работе над WEB-сайтами нужно думать о безопасности не только определенного участка кода или отдельной части, а обо всей системе в целом. Безопасность – это права доступа, операционная система, журналирование, мониторинг, обработка ошибок, тестирование и т.д.

## **Архитектура проекта.**
Название: Задачник

Бэкэнд: ASP.NET

Фронтенд: Bootstrap

Модель: MVC

БД: PosgreSQL


## Компоненты:
1.	Аутентификация и авторизация: ASP.NET Identity, JWT, OAuth 2.0.
2.	Главная страница: TaskController, AJAX, SchedulerController.
3.	Профиль пользователя: UserProfileController, статистика, достижения.
4.	Список задач (CRUD): TaskManagerService, файлы, дедлайны.
5.	Расписание: SchedulerService, AJAX, drag-and-drop.
6.	Модель данных: Users, Tasks, Schedules, Files, AuditLogs.
7.	Резервное копирование: BackupService, шифрование.
8.	Защита от атак: reCAPTCHA, Rate Limiting, Cloudflare.

![alt text](https://github.com/yulterra/archsecurity/blob/main/Слайд1.JPG)

### **Алгоритм работы программы:**
**1.	Аутентификация и регистрация:**

- Пользователь заходит на сайт и видит страницу входа/регистрации.
- При регистрации система создает учетную запись, сохраняет зашифрованные пароли и назначает роли (например, "пользователь" или "администратор").
- При входе пользователю генерируется JWT-токен или сессия для последующих запросов.

**2.	Главная страница:**

- После успешного входа пользователя перенаправляют на главную страницу, где отображается список задач и расписание.
- Здесь пользователь видит предстоящие задачи с дедлайнами, выполненные задачи, а также может переключаться между различными видами (например, календарь, список).

**3.	Профиль пользователя:**

- Пользователь может перейти на страницу профиля, где находятся его персональные данные, статистика выполненных задач, достижения и другая информация.
- Есть возможность редактировать данные (например, менять аватар, имя, email и т.д.).

**4.	Работа с задачами:**

- На странице списка задач или расписания пользователь может:
   - Создать новую задачу с заголовком, описанием, приложенными файлами и сроком выполнения.
   - Редактировать или удалить существующую задачу.
   - Отметить задачу как выполненную или переделать дедлайны.
   - Добавить подзадачу
- Все задачи сохраняются в базе данных PostgreSQL с привязкой к профилю пользователя.

**5.	Управление расписанием:**

- В разделе расписания пользователь может:
   - Добавить задачу на определенную дату или временной интервал.
   - Редактировать расписание задач с возможностью перемещать их по календарю
   - Удалить задачи из расписания или открепить их от определенной даты.
   - Все изменения в расписании автоматически синхронизируются с базой данных.

**6.	Работа с файлами:**

- При создании или редактировании задачи пользователь может загружать файлы, которые хранятся в безопасном хранилище с проверкой на вирусы и контролем типа/размера файла.

**7.	Уведомления и оповещения:**

- Приложение отправляет уведомления пользователю о предстоящих дедлайнах или просроченных задачах.
- Уведомления могут быть реализованы через email и  push-уведомления на фронтенде.

**8.	Резервное копирование и восстановление:**

- Приложение регулярно создает резервные копии базы данных и файлов, чтобы гарантировать восстановление данных в случае сбоев. (ежедневно)
- Данные шифруются и хранятся в безопасных удаленных хранилищах.

**9.	Завершение сессии:**

- После завершения работы пользователь может выйти из системы, что завершает сессию (или токен JWT), очищая все активные данные с клиентской стороны.


# Безопасность 

## **Постановка задачи**
Задача заключается в проектировании архитектуры безопасности веб-приложения "Задачник" таким образом, чтобы использование приложения можно было считать безопасным.

## Цели и Предположения Безопасности (ЦПБ)
**Цели:**
 - обеспечить защиту персональных данных;
 - обеспечить безопасность аутентификации и авторизации;
 - защитить сайт от угроз и атак;
 - защита инфраструктуры;
 - выделение модуля резервного копирования;
 - обеспечить безопасную разработку по ГОСТам;
 - контроль уязвимостей;
 - избежать хранения лишней информации (избыточность);
 - хранить пароли в зашифрованном виде ();
 - защитить сайт от посещения ботов;
 - распределение баз данных.



# Негативный сценарий 

## **1. Обеспечение защиты персональных данных**

Злоумышленник может попытаться получить доступ к персональным данным следующими методами:

**- Фишинг:**

  Сценарий атаки: Создание поддельного сайта, имитирующего страницу входа, и отправка фишинговых писем пользователям.
  
  Цель атаки: Кража учетных данных для последующего несанкционированного доступа.
  
**- Социальная инженерия:**

  Сценарий атаки: Обманом убедить пользователей раскрыть свои учетные данные.
  
  Цель атаки: Доступ к персональной информации через учетную запись пользователя.

**- Перехват данных:**
 
  Сценарий атаки: Использование атаки "человек посередине" (MITM) для перехвата данных, передаваемых между клиентом и сервером.
 
  Цель атаки: Получение доступа к передаваемым персональным данным.
 
## **2. Обеспечение безопасности аутентификации и авторизации**

Злоумышленник может пытаться обойти системы аутентификации и авторизации для получения несанкционированного доступа:

**- Подбор слабого пароля:**

  Сценарий атаки: Подбор слабых или стандартных паролей для получения доступа к учетной записи пользователя.

  Цель атаки: Неавторизованный доступ к данным.
  
**- Кража сессионных токенов:**
  
  Сценарий атаки: Захват сессионных токенов через XSS-атаку для использования учетной записи пользователя.
  
  Цель атаки: Использование украденных сессионных данных для доступа к системе.
 
## **3. Защита от угроз и атак**

**- SQL-инъекция:**

  Сценарий атаки: Внедрение вредоносных SQL-запросов через поля ввода данных.
  
  Цель атаки: Получение доступа к базе данных или изменение данных.
  
**- XSS-атака:**

  Сценарий атаки: Внедрение вредоносного скрипта через поля ввода, который выполняется на стороне пользователя.

  Цель атаки: Кража сессионных данных или выполнение несанкционированных действий от имени пользователя.
  
**- DDoS-атака:**

  Сценарий атаки: Генерация большого объема запросов с целью перегрузки сервера.
  
  Цель атаки: прекратить нормальное функционирование сайта 

## **4. Защита инфраструктуры**

**- Атака на серверную инфраструктуру:**
  
  Сценарий атаки: Использование известных уязвимостей серверного ПО для получения доступа к серверу.
  
  Цель атаки: Получение доступа к данным или контроль над сервером.

## **5. Хранение паролей в зашифрованном виде**

**- Атака на хэшированные пароли:**

  Сценарий атаки: Доступ к базе данных и попытка расшифровки хэшированных паролей с использованием радуговых таблиц или перебора.

  Цель атаки: Расшифровка паролей для доступа к учетным записям.

## **6. Защита от посещения ботов**

**- Боты для автоматизации задач:**

  Сценарий атаки: Использование автоматизированных ботов для создания массовых запросов или действий в системе.

  Цель атаки: Перегрузка системы, создание хаоса или злоупотребление функционалом сайта.

   

# Позитивный сценарий

## **1. Обеспечение защиты персональных данных**

Решения:

1)	Шифрование всех передаваемых и хранимых персональных данных с использованием TLS и AES.

2)	Регулярная проверка активности учетных записей на предмет подозрительных действий.
 
## **2. Обеспечение безопасности аутентификации и авторизации**

Решения:

1)	Установка минимальной длины и сложности пароля (комбинация букв, цифр, символов).

2)	Внедрение обязательных требований к паролю (например, наличие заглавных букв и спецсимволов).

3)	Ограничение количества неудачных попыток ввода пароля (с блокировкой учетной записи после нескольких попыток).

4)	Использование безопасных методов хранения паролей (например, bcrypt или Argon2 для хэширования).

5)	Защита сессий с использованием Secure и HttpOnly cookies, чтобы предотвратить кражу сессионных данных.

6)	Ограничение доступа к ресурсам приложения с использованием ролевой модели доступа (Role-Based Access Control, RBAC).

7)	Регулярная проверка токенов доступа на предмет их срока действия и целостности.
 

## **3. Защита от угроз и атак**

Решения:

1)	Применение параметризованных запросов для защиты от SQL-инъекций.

2)	Корректность и проверка через средства безопасности, межсетевой экран (экранирование) всех пользовательских вводов для предотвращения XSS-атак.

3)	Внедрение механизма защиты от CSRF (Cross-Site Request Forgery) для защиты критических действий пользователей.

4)	Использование Content Security Policy (CSP) для ограничения загрузки неподтвержденных скриптов.

5)	Ограничение объема запросов (rate limiting) для защиты от DDoS-атак и чрезмерных действий пользователей.

  - ограничить количество уровней подзадач и подзадач

 
## **4. Защита инфраструктуры**

Решения:

1)	Регулярное обновление и патчинг всех используемых системных компонентов и ПО.

2)	Настройка брандмауэров и средств мониторинга для отслеживания подозрительных сетевых активностей.

3)	Разграничение прав доступа на уровне серверов и базы данных, минимизация привилегий.

4)	Использование резервного копирования данных для восстановления системы в случае атак или сбоев.

5)	Разделение приложений и баз данных по принципу микросервисов для снижения риска компрометации всей системы.


## **5. Хранение паролей в зашифрованном виде**

Решения:

1)	Использование современных алгоритмов хэширования паролей (bcrypt, Argon2) с солью.

2)	Регулярная ротация ключей шифрования для хранения паролей и других конфиденциальных данных.

3)	Хранение данных в базе данных в зашифрованном виде с использованием алгоритмов AES-256.

 
## **6. Защита от посещения ботов**

Решения:

1.	Внедрение CAPTCHA для проверки пользователей при выполнении критических действий (регистрация, создание задач).

2.	Ограничение скорости запросов с одного IP-адреса (rate limiting) для предотвращения злоупотреблений.

3.	Использование технологий анализа поведения (например, отслеживание аномальной активности), чтобы выявить автоматизированные действия ботов.

   
