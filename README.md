
Архитектура проекта.
Название: Задачник
Бэкэнд: ASP.NET
Фронтенд: Bootstrap
Модель: MVC
БД: PosgreSQL

Алгоритм работы:
1. Пользователь 
2. Страница авторизации и регистрации
3. Главная страница (список задач, расписание)
4. Профиль пользователя (персональные данные, статистика, достижения)
5. Форма для задачи (создать, редактировать, удалить)
6. Форма для списка задач
7. Форма для материалов
8. Форма для расписания (создать, редактировать, удалить, добавить задачу, удалить задачу)

Безопасность 

Постановка задачи
Задача заключается в проектировании архитектуры безопасности веб-приложения "Задачник" таким образом, чтобы использование приложения можно было считать безопасным.

Цели и Предположения Безопасности (ЦПБ)
Цели:
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


Описание целей:
1. Обеспечение защиты персональных данных.
   Перехват персональных данных может быть осуществлен следующим образом:
     1) Ненадежный пароль
        Решение:
        - установка минимальной длины пароля;
        - установка обяазтельных требований к паролю;
        - ограничение неуспешных попыток доступа к программному изделию;
        - защита обратной связи при вводе аутентификационной информации.
      2) Фишинг
      3) Социальная инженерия
      4) Перехват данных
   
   

   
