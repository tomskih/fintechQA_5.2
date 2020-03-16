# Задание 5.2. Instagram
## Business-critical функционал
Основное назначение Instagram - размещение и публикация визуального контента. Пользователи делятся материалами, их задача - получение максимального отклика на свои публикации. Пользователи и компании зарабатывают на рекламе, Instagram тоже зарабатывает на рекламе.
В связи с этим, можно выделить следующий  business-critical функционал:
- **Регистрация аккаунта** (личного, бизнес) - т.к. сервисом нельзя пользоваться без авторизации
- **Авторизация** - причина та же, что и в предыдущем пункте
- **Размещение контента**
- **Просмотр контента** - работа с лентой
## Тест-кейсы
Рассмотрим набор тест-кейсов, позволяющих убедиться в работоспособаности business-critical функционала. 
Полный набор атомарных кейсов, упорядоченный в **логическом** порядке, размещен по [ссылке](https://docs.google.com/spreadsheets/d/1-Qdef1ccjIZrnPtbbA8mRNIQ6ewks3AvhT7rfgTvfPA/edit?usp=sharing). Каждый лист содержит тест-кейсы по одному из выделенных функционалов.
В последней колонке расставлены приоритеты, по ним можно выполнить сортировку
![](https://raw.githubusercontent.com/tomskih/fintechQA_5.2/master/%D1%81%D0%BE%D1%80%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%BA%D0%B0%20%D0%BF%D0%BE%20%D0%BF%D1%80%D0%B8%D0%BE%D1%80%D0%B8%D1%82%D0%B5%D1%82%D1%83.png)
Некоторые кейсы можно рассматривать только в совокупности, поэтому привожу наименования полных сценариев в порядке уменьшения приоритета
1. Регистрация
-- Регистрация по номеру телефона с вводом корретных данных (1.1 + 1.3 + 1.7 + 1.11) 
-- Регистрация по эл. адресу с вводом корретных данных (1.2 + 1.5)
-- Повторный запрос кода подтверждения (1.10)
-- Регистрация по номеру телефона и email с вводом некорретных данных на разных шагах(1.4, 1.6, 1.3 + 1.8, 1.3 + 1.5/1.7 + 1.12, 1.3 + 1.5/1.7 + 1.11 + 1.14)
-- Изменение номера телефона для регистрации
2. Авторизация
-- Авторизация по логину и паролю с вводом корретных данных (2.1 + 2.2)
-- Повторная авторизация с вводом корректных данных (2.5 + 2.6)
-- Авторизация по логину и паролю с вводом некорретных данных (2.1 + 2.3, 2.1 + 2.4)
3. Создание контента
-- Создание простой публикации с выбором фото из предложенных (3.1 + 3.3 + 3.7)
-- Создание сториз (3.8)
-- Создание простой публикации с выбором фото из проводника/камеры (3.1 + 3.3, 3.1 + 3.4)
-- Создание публикации с фильтрами/редактированием (3.2/3.3/3.4 + 3.5, 3.2/3.3/3.4 + 3.6)
4. Просмотр контента

## Архитектура проекта
Рассмотрим ситуацию, когда пользователь уже зарегистрирован и авторизован - в этом случае мы сможем выстроить единый интерфейс, с которым связаны 3 самых критичных тест-кейса. В этом случае наиболее критичными являются:
-- Создание простой публикации с выбором фото из предложенных
-- Создание сториз
-- Просмотр ленты
Работаем с интерфейсом главной страницы - именно с него доступны перечисленные функции.
![](https://raw.githubusercontent.com/tomskih/fintechQA_5.2/master/page_example.jpg)





## Сценарий проверок
## Выбор инструментов
