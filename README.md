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


### **Алгоритм работы программы:**
**1.	Аутентификация и регистрация:**

- Пользователь заходит на сайт и видит страницу входа/регистрации.
- При регистрации система создает учетную запись, сохраняет зашифрованные пароли и назначает роли (например, "пользователь" или "администратор").
- При входе пользователю генерируется JWT-токен или сессия для последующих запросов.

**2.	Главная страница:**

- После успешного входа пользователя перенаправляют на главную страницу, где отображается список задач и ваш прогресс.
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

# Визуальная модель проекта: 
![alt text](https://github.com/yulterra/archsecurity/blob/main/Слайд1.JPG)

![alt text](https://github.com/yulterra/archsecurity/blob/main/Слайд2.JPG)


# Безопасность 

## **Постановка задачи**
Задача заключается в проектировании архитектуры безопасности веб-приложения "Задачник" таким образом, чтобы использование приложения можно было считать безопасным.

## Цели и Предположения Безопасности (ЦПБ)
**Цели:**
 - обеспечить защиту персональных данных;
 - обеспечить контроль доступа;
 - обеспечить безопасность аутентификации и авторизации;
 - защитить сайт от угроз и атак;
 - защита инфраструктуры;
 - выделение модуля резервного копирования;
 - обеспечить безопасную разработку по ГОСТам;
 - контроль уязвимостей;
 - избежать хранения лишней информации (избыточность);
 - хранить пароли в зашифрованном виде;
 - защитить сайт от посещения ботов;
 - распределение баз данных.



# Негативный сценарий 

## **1. Обеспечение защиты персональных данных**

Злоумышленник может попытаться получить доступ к персональным данным следующими методами:

- **Фишинг:**

  Сценарий атаки: Создание поддельного сайта, имитирующего страницу входа, и отправка фишинговых писем пользователям.
  
  Цель атаки: Кража учетных данных для последующего несанкционированного доступа.
  
- **Социальная инженерия:**

  Сценарий атаки: Обманом убедить пользователей раскрыть свои учетные данные.
  
  Цель атаки: Доступ к персональной информации через учетную запись пользователя.

- **Перехват данных:**
 
  Сценарий атаки: Использование атаки "человек посередине" (MITM) для перехвата данных, передаваемых между клиентом и сервером.
 
  Цель атаки: Получение доступа к передаваемым персональным данным.
  
- **Криптографические сбои:**

  Сценарий атаки:
   - Перехват каких-либо данных в открытом виде. Это касается таких протоколов, как HTTP, SMTP, FTP, а также обновлений TLS, таких как STARTTLS.
   - Обнаружение каких-либо старых или слабых криптографические алгоритмов, протоколов по умолчанию или в устаревший код
   - Атака на криптографические ключи по умолчанию, атака на управление ключами или их ротации

## **2. Обеспечение контроля доступа**

Сцениарий атаки с использованием ошибок безопасности: 

   - Нарушение принципа наименьших привилегий или запрета по умолчанию, когда доступ должен предоставляться только для определённых возможностей, ролей или пользователей, но доступен всем.
   - Обход проверок контроля доступа путём изменения URL-адреса (подмена параметров или принудительный просмотр), внутреннего состояния приложения или HTML-страницы, а также с помощью инструмента для атак, изменяющего запросы API.
   - Разрешение на просмотр или редактирование чужой учётной записи путём предоставления её уникального идентификатора (незащищённые прямые ссылки на объекты)
   - Доступ к API с отсутствующими элементами управления доступом для ПУБЛИКАЦИИ, РАЗМЕЩЕНИЯ и УДАЛЕНИЯ.
   - Повышение привилегий. Работа в качестве пользователя без входа в систему или в качестве администратора при входе в систему в качестве пользователя.
   - Манипуляции с метаданными, такие как воспроизведение или изменение токена управления доступом JSON Web Token (JWT), а также манипулирование файлами cookie или скрытыми полями для повышения привилегий или злоупотребления при аннулировании JWT.
   - Неправильная настройка CORS позволяет получить доступ к API из неавторизованных/ненадёжных источников.
   - Принудительно переходить на аутентифицированные страницы в качестве неаутентифицированного пользователя или на привилегированные страницы в качестве стандартного пользователя.
     
## **3. Обеспечение безопасности аутентификации и авторизации**

Злоумышленник может пытаться обойти системы аутентификации и авторизации для получения несанкционированного доступа:

- **Подбор слабого пароля:**

  Сценарий атаки: Подбор слабых или стандартных паролей для получения доступа к учетной записи пользователя.

  Цель атаки: Несанкционированный доступ к данным.
  
- **Кража сессионных токенов:**
  
  Сценарий атаки: Захват сессионных токенов через XSS-атаку для использования учетной записи пользователя.
  
  Цель атаки: Использование украденных сессионных данных для доступа к системе.
 
## **4. Защита от угроз и атак**

- **SQL-инъекция:**

     Сценарий атаки: Внедрение вредоносных запросов через поля ввода данных.
     
     Цель атаки: Получение доступа к базе данных или изменение данных.
     
     Приложение уязвимо для атаки, когда:
   
   - Приложение не проверяет, не фильтрует и не очищает данные, предоставленные пользователем.
   - Динамические запросы или непараметризованные вызовы без учёта контекста используются непосредственно в интерпретаторе.
   - В параметрах поиска объектно-реляционного отображения (ORM) используются вредоносные данные для извлечения дополнительных конфиденциальных записей.
   - Вредные данные используются напрямую или объединяются. SQL-запрос или команда содержат структуру и вредоносные данные в динамических запросах, командах или хранимых процедура
  
- **XSS-атака:**

  Сценарий атаки: Внедрение вредоносного скрипта через поля ввода, который выполняется на стороне пользователя.

  Цель атаки: Кража сессионных данных или выполнение несанкционированных действий от имени пользователя.
  
- **DDoS-атака:**

  Сценарий атаки: Генерация большого объема запросов с целью перегрузки сервера.
  
  Цель атаки: прекратить нормальное функционирование сайта 

## **5. Защита инфраструктуры**

- **Атака на серверную инфраструктуру:**
  
  Сценарий атаки: Использование известных уязвимостей серверного ПО для получения доступа к серверу.
  
  Цель атаки: Получение доступа к данным или контроль над сервером.

## **6. Хранение паролей в зашифрованном виде**

- **Атака на хэшированные пароли:**

  Сценарий атаки: Доступ к базе данных и попытка расшифровки хэшированных паролей с использованием радуговых таблиц или перебора.

  Цель атаки: Расшифровка паролей для доступа к учетным записям.

## **7. Защита от посещения ботов**

- **Анализ или атака с помощью ботов:**

  Сценарий атаки: Использование автоматизированных ботов для создания массовых запросов или действий в системе.

  Цель атаки: Перегрузка системы, создание хаоса или злоупотребление функционалом сайта.

   

# Позитивный сценарий

## **1. Обеспечение защиты персональных данных**

Решения:

   1)	Классифицируйте данные, обрабатываемые, сохраняемые или передаваемые приложением. Определите, какие данные являются конфиденциальными в соответствии с законами о конфиденциальности, нормативными требованиями или потребностями бизнеса.
   
   2) Не храните конфиденциальные данные без необходимости. Удаляйте их как можно скорее или используйте токенизацию или даже усечение данных, соответствующие требованиям PCI DSS. Данные, которые не хранятся, не могут быть украдены.
   
   3) Убедитесь, что все конфиденциальные данные зашифрованы в состоянии покоя.
   
   4) Убедитесь, что используются современные и надёжные стандартные алгоритмы, протоколы и ключи; применяйте надлежащее управление ключами.
   
   5) Шифруйте все передаваемые данные с помощью безопасных протоколов, таких как TLS с шифрованием с сохранением конфиденциальности (FS), приоритетом шифрования на стороне сервера и безопасными параметрами. Обеспечьте шифрование с помощью таких директив, как HTTP Strict Transport Security (HSTS).
   
   6)	Регулярная проверка активности учетных записей на предмет подозрительных действий.

## **2. Обеспечение контроля доступа**

Решение: 

   1) По умолчанию доступ закрыт, за исключением общедоступных ресурсов.

   2) Внедрите механизмы контроля доступа один раз и используйте их во всём приложении, в том числе минимизируя использование межсайтового обмена ресурсами (CORS).

   3) Средства контроля доступа к моделям должны обеспечивать владение записями, а не предполагать, что пользователь может создавать, читать, обновлять или удалять любые записи.

   4) Модели предметной области должны обеспечивать соблюдение требований к ограничению бизнеса уникальных приложений.

   5) Отключите отображение каталогов веб-сервера и убедитесь, что метаданные файлов (например, .git) и резервные копии не отображаются в корневом каталоге.

   6) Регистрируйте сбои в системе контроля доступа и при необходимости предупреждайте администраторов (например, о повторяющихся сбоях).

   7) Ограничьте скорость API и доступ к контроллеру, чтобы минимизировать ущерб от автоматизированных атак.

   8) Идентификаторы сеансов с отслеживанием состояния должны быть аннулированы на сервере после выхода из системы. Токены JWT без отслеживания состояния должны иметь короткий срок действия, чтобы свести к минимуму возможности злоумышленника. Для токенов JWT с длительным сроком действия настоятельно рекомендуется следовать стандартам OAuth для отзыва доступа.
## **3. Обеспечение безопасности аутентификации и авторизации**

Решения:

   1)	Установка минимальной длины и сложности пароля (комбинация букв, цифр, символов).
   
   2)	Внедрение обязательных требований к паролю (например, наличие заглавных букв и спецсимволов).
   
   3)	Ограничение количества неудачных попыток ввода пароля (с блокировкой учетной записи после нескольких попыток).
   
   4)	Использование безопасных методов хранения паролей (например, bcrypt или Argon2 для хэширования).
   
   5)	Защита сессий с использованием Secure и HttpOnly cookies, чтобы предотвратить кражу сессионных данных.
   
   6)	Ограничение доступа к ресурсам приложения с использованием ролевой модели доступа (Role-Based Access Control, RBAC).
   
   7)	Регулярная проверка токенов доступа на предмет их срока действия и целостности.
   
   8) Защита обратной связи при вводе аутентификационной информации.
 

## **4. Защита от угроз и атак**

Решения:

   1)	Применение параметризованных запросов для защиты от SQL-инъекций.
      Для предотвращения внедрения требуется хранить данные отдельно от команд и запросов:
   
         - Предпочтительным вариантом является использование безопасного API, которое позволяет полностью отказаться от интерпретатора, предоставляет параметризованный интерфейс или позволяет перейти на инструменты объектно-реляционного отображения (ORM).
   Примечание: даже при использовании параметров хранимые процедуры все равно могут привести к SQL-инъекции, если PL/SQL или T-SQL объединяют запросы и данные или выполняют вредоносные данные с помощью EXECUTE IMMEDIATE или exec().
   
         - Используйте позитивную проверку ввода на стороне сервера. Это не является полной защитой, поскольку во многих приложениях требуются специальные символы, например в текстовых областях или API для мобильных приложений.
   
         - Для любых оставшихся динамических запросов экранируйте специальные символы, используя специальный синтаксис экранирования для этого интерпретатора.
   Примечание: структуры SQL, такие как имена таблиц, имена столбцов и т. д., не могут быть экранированы, поэтому имена структур, предоставляемые пользователем, опасны. Это распространённая проблема в программах для создания отчётов.
   
         - Используйте LIMIT и другие средства управления SQL в запросах, чтобы предотвратить массовое раскрытие записей в случае внедрения SQL-кода.
   
   3)	Корректность и проверка через средства безопасности, межсетевой экран (экранирование) всех пользовательских вводов для предотвращения XSS-атак.
   
   4)	Внедрение механизма защиты от CSRF (Cross-Site Request Forgery) для защиты критических действий пользователей.
   
   5)	Использование Content Security Policy (CSP) для ограничения загрузки неподтвержденных скриптов.
   
   6)	Ограничение объема запросов (rate limiting) для защиты от DDoS-атак и чрезмерных действий пользователей.
   
-- ограничить количество уровней подзадач и подзадач

 
## **5. Защита инфраструктуры**

Решения:

   1)	Регулярное обновление и патчинг всех используемых системных компонентов и ПО.
   
   2)	Настройка брандмауэров и средств мониторинга для отслеживания подозрительных сетевых активностей.
   
   3)	Разграничение прав доступа на уровне серверов и базы данных, минимизация привилегий.
   
   4)	Использование резервного копирования данных для восстановления системы в случае атак или сбоев.
   
   5)	Разделение приложений и баз данных по принципу микросервисов для снижения риска компрометации всей системы.


## **6. Хранение паролей в зашифрованном виде**

Решения:

1)	Использование современных алгоритмов хэширования паролей (bcrypt, Argon2) с солью.

2)	Регулярная ротация ключей шифрования для хранения паролей и других конфиденциальных данных.

3)	Хранение данных в базе данных в зашифрованном виде с использованием алгоритмов AES-256.

 
## **7. Защита от посещения ботов**

Решения:

1.	Внедрение CAPTCHA для проверки пользователей при выполнении критических действий (регистрация, создание задач).

2.	Ограничение скорости запросов с одного IP-адреса (rate limiting) для предотвращения злоупотреблений.

3.	Использование технологий анализа поведения (например, отслеживание аномальной активности), чтобы выявить автоматизированные действия ботов.

# Схема взаимодействия модулей

![image](https://github.com/user-attachments/assets/dbebf054-12fe-4913-8551-5280dc2c8355)

   
![image](https://github.com/user-attachments/assets/ab79ee94-baf5-4954-adfd-fb1240f2c572)

![image](https://github.com/user-attachments/assets/dbc06e8a-5836-4a9e-b805-cafe94e01726)

