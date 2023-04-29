Введение
--------
В этом репозитории представлены автотесты для тестирования продуктов Ростелеком (компоненты системы), UI часть, выполненные на основании: 
1) Требований заказчика “[Требования_SSO_для_тестирования_last.doc](https://docs.google.com/document/d/18F9cmg5wYkZt1mkXFSoCD4HSCRUF87Cf/edit?usp=share_link&ouid=102802765291179425980&rtpof=true&sd=true)”
2) Задания от учебной платформы Skillfactory “[SF_instruction.pdf](https://drive.google.com/file/d/136pleOBIMdf5tvUWVF7Rv6c1xO0-2bQB/view?usp=share_link) ” 
3) Тест-кейсов “[Тест-кейсы.xlsx](https://docs.google.com/spreadsheets/d/1kO4JYphCtGEIM6NQjoO_Xi5rAneaQF2e/edit?usp=share_link&ouid=102802765291179425980&rtpof=true&sd=true)”

Проанализированы требования в файле “[Требования_мои_комменты.doc](https://docs.google.com/spreadsheets/d/1kO4JYphCtGEIM6NQjoO_Xi5rAneaQF2e/edit?usp=share_link&ouid=102802765291179425980&rtpof=true&sd=true)”, оформлены баг-репорты “[Баг-репорты.xlsx](https://docs.google.com/spreadsheets/d/1k9kh_1XGtubH96kDWQehckSvAixRNIDq/edit?usp=share_link&ouid=102802765291179425980&rtpof=true&sd=true)”. 
Все перечисленные документы находятся в папке *docs*.<br> 
Итого по подсчету pytest – 463 тест-проверки (collected 463 items).

Тестируемые компоненты (продукты Ростелеком):
---------------------------------------------
1. ЕЛК Web - https://lk.rt.ru/,
2. Онлайм Web - https://my.rt.ru/,
3. Старт Web - https://start.rt.ru/,
4. Умный дом Web - https://lk.smarthome.rt.ru/,
5. Ключ Web - https://key.rt.ru/.

Технологии
----------
Использован язык программирования Python3, паттерн PageObject, библиотеки PyTest + Selenium. Для реализации паттерна PageObject взят фреймворк Smart Page Object, репозиторий по [ссылке](https://github.com/TimurNurlygayanov/ui-tests-example), автор Тимур Нурлыгаянов. Автотесты сгруппированы по типу и представлены в отдельных файлах для удобства использования.

Файлы:
------
- *conftest.py* – содержит весь необходимый код для «захвата» упавших тест-кейсов и создания скриншотов страницы в случае падения (скриншоты не работают, вопросы к Тимуру).
- *constants.py* – содержит константы использованные в автотестах.
- *pages/base.py* – содержит паттерн PageObject – основные действия над веб-страницей: вход по заданному URL, перемещение назад-вперед, вверх-вниз и т.д.
- *pages/elements.py* – содержит класс-помощник для определения и работы с веб-элементами на веб-страницах.
- *tests/test_rtc_form_elements.py* – содержит тест-кейсы для тестирования наличия и содержания элементов форм: авторизация по паролю, авторизация по коду, восстановление пароля и регистрация.
- *tests/test_rtc_register_email.py* – содержит тест-кейсы для тестирования элемента формы регистрации поля ввода email – проверки валидации.
- *tests/test_rtc_register_name.py* – содержит тест-кейсы для тестирования элементов формы регистрации полей ввода имени и фамилии – проверки валидации.
- *tests/test_rtc_register_passw.py* – содержит тест-кейсы для тестирования элемента формы регистрации полей ввода пароля – проверки валидации.
- *tests/test_rtc_register_phone.py* – содержит тест-кейсы для тестирования элемента формы регистрации для поля ввода номера телефона – проверки валидации.
- *tests/test_rtc_signin_auth.py* – содержит тест-кейсы для тестирования сценариев авторизации по паролю с использованием email или логина.
- *tests/test_rtc_cookies.py* – содержит тест-кейсы для тестирования сценария «вход по куки (cookies)».

Как запускать тесты:
--------------------
1) Установить все зависимости:
В командной строке терминала (bash) набрать и выполнить: 
`pip3 install -r requirements.txt`
2) Скачать Selenium [WebDriver](https://chromedriver.chromium.org/downloads) - выбрать совместимую версию с вашим браузером Chrome.
3) Запуск тестов:
```
python3 -m pytest -v --driver Chrome --driver-path ~/chrome tests/*
или
pytest -v --driver Chrome --driver-path ~/chrome tests/*
Пример:
pytest -v --driver Chrome --driver-path D:/Downloads/chromedriver_win32/chromedriver.exe tests/ tests/test_rtc_signin_auth.py.py
или
pytest -v -k test --driver Chrome --driver-path D:/Downloads/chromedriver_win32/chromedriver.exe tests/ 
```
---
☝️ Пароли (валидные) спрятаны в файл .env (не выложен здесь). <BR>
Создать в директории проекта файл .env, в него записать: <BR> 
```
VALID_EMAIL = 'your valid email'
VALID_LOGIN = 'your account'
VALID_PASSWORD = 'your valid password'
```
Техники тест-дизайна:
---------------------
1) Тесты разрабатывались методом чёрного ящика – анализ требований системы без знания внутренней структуры – все тесты. 
2) Тесты разрабатывались для функционального тестирования – проверка основного функционала компонентов системы: регистрация нового пользователя, аутентификация, восстановление доступа.
3) Применялось позитивное и негативное тестирование – ввод в поля формы регистрации нового пользователя, авторизация по паролю с использованием email или логина, наличие и описание элементов формы.
4) Техника разбиения на классы эквивалентности и анализ граничных значений – создание тестовых данных для ввода в поля формы регистрации нового пользователя: имя, фамилия, email, номер телефона, пароль.
5) Техника причина-следствие или таблица решений или таблица альтернатив – таблицы представленны в требованиях к системе - отражают комбинации входных данных (логин, телефон, пароль, ФИО, одноразовый код), продукты (ЕЛК, Онлайм, Старт, Умный дом и Ключ) и действия-следствия: аутентификация, регистрация и авторизация.
6) Техника исследовательского тестирования (на основании пользовательского опыта) – описание/название элементов форм (табы, слоганы, надписи, кнопки, поля ввода), расположение элементов, авторизация в системе. В связи с этим не создавал баг-репорты касающихся не полному соответствию названий элементов на сайте требованиям.
7) Техника предугадывания ошибки – применялась для проверки таблиц решений – часть решений может выполнятся или не выполняться, например, там где не должно быть авторегистрации, но она при этом есть.

Инструменты тест-дизайна:
-------------------------
1) Чит-листы – как вспомогательный инструмент для тестирования полей ввода в форме регистрации нового пользователя “ Чит-лист.xlsx”. Получили применения для параметризации с использованием фикстур PyTest.
2) Сервис [Random string generator](http://www.unit-conversion.info/texttools/random-string-generator/) – для создания тестовых входных данных (ФИО, логин, пароль).
3) [SEO-АНАЛИЗ ТЕКСТА](https://text.ru/seo) - для подсчета количества символов.

Другие инструменты:
-------------------
1) 1secmail.com – сервис для создания временной почты.
2) ChroPath – плагин для Google Chrome - инструмент для поиска элементов на странице.
3) Bug Magnet – плагин для Google Chrome – инструмент-помощник по исследовательскому тестированию для Chrome. Добавляет валидные/не валидные и граничные значения в контекстное меню (щелчок правой кнопкой мыши) для полей ввода.
4) PyCharm – интегрированная среда разработки на Python.

Что не получилось:
------------------
Не получилось выполнить тестирование входа при помощи куки “test_rtc_cookies.py”. 
Решал задачу через считывание куки в объект web_driver Selenium и дальнейшей записи в двоичный файл. Далее считывал из файла, и добавлял в атрибут web_driver:
page_new._web_driver.add_cookie(cookie)
Именно в этом месте возникала ошибка: 
```
“
>       raise exception_class(message, screen, stacktrace)
E       selenium.common.exceptions.InvalidCookieDomainException: Message: invalid cookie domain: Cookie 'domain' mismatch
”
```
Как пытался решить:
-------------------
1) В начале, успешную авторизацию и запись куки были в одном методе “test_write_cookies”, а считывание куки и вход по ней в другом “test_sign_in_by_cookies”. Далее для удобства решения проблемы перенес всё в один метод “test_write_cookies”. 
2) Перед считыванием и записью куки внёс задержку до 20 сек.
3) Пытался добавлять куку в web_driver не построчно:
    for cookie in get_cookies:
        page_new._web_driver.add_cookie(cookie)

а целиком:
   page_new._web_driver.add_cookie(get_cookies)

4) Распечатывал в консоль терминала и анализировал считанную куку:
print(f"'{get_cookies}'\n")
и
    for cookie in get_cookies:
        print(f"{cookie}")

Попытки не увенчались успехом. 

Был бы признателен за помощь в решении этого «проблемного» вопроса.
-------------------------------------------------------------------
