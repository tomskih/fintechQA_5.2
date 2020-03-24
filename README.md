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
    * Регистрация по номеру телефона с вводом корретных данных (1.1 + 1.3 + 1.7 + 1.11) 
    * Регистрация по эл. адресу с вводом корретных данных (1.2 + 1.5)
    * Повторный запрос кода подтверждения (1.10)
    * Регистрация по номеру телефона и email с вводом некорретных данных на разных шагах(1.4, 1.6, 1.3 + 1.8, 1.3 + 1.5/1.7 + 1.12, 1.3 + 1.5/1.7 + 1.11 + 1.14)
    * Изменение номера телефона для регистрации
2. Авторизация
    * Авторизация по логину и паролю с вводом корретных данных (2.1 + 2.2)
    * Повторная авторизация с вводом корректных данных (2.5 + 2.6)
    * Авторизация по логину и паролю с вводом некорретных данных (2.1 + 2.3, 2.1 + 2.4)
3. Создание контента
    * Создание простой публикации с выбором фото из предложенных (3.1 + 3.3 + 3.7)
    * Создание сториз (3.8)
    * Создание простой публикации с выбором фото из проводника/камеры (3.1 + 3.3, 3.1 + 3.4)
    * Создание публикации с фильтрами/редактированием (3.2/3.3/3.4 + 3.5, 3.2/3.3/3.4 + 3.6)
4. Просмотр контента

## Архитектура проекта
Рассмотрим ситуацию, когда пользователь имеет учетную запись. Он должен авторизоваться, иметь возможность создать публикацию и просмотреть ленту - это и будут 3 самых критичных тест-кейса.
* Авторизация по логину и паролю с вводом корретных данных (2.1 + 2.2)
* Создание простой публикации с выбором фото из предложенных (3.1 + 3.3 + 3.7)
* Просмотр контента

Работаем с интерфейсом мобильной браузерной версии на ОС Android. Пример страницы представлен ниже.
![](https://raw.githubusercontent.com/tomskih/fintechQA_5.2/master/page_example.jpg)

Схематично архитектура проекта выглядит так
![](https://raw.githubusercontent.com/tomskih/fintechQA_5.2/master/PageObjectPatternModel.png)

## Сценарий проверок
### Описание классов (страниц, блоков страницы)
#### Страница авторизации по логину-паролю
```java
public class LoginPage { 
	String url = "https://www.instagram.com/accounts/login"
	Select langSelect
	Link forgotPassLink, signUpLink
	Input loginInput, passInput
	Button loginButton
	
}
```
#### Главная страница авторизованного пользователя
```java
public class MainPage { 
	String url = "https://www.instagram.com"
	
	public class Header {   //хедер
		Link mainLink
		Button newStoryButton, inboxButton
	
	}

	public class Stories {  //блок со сторизами
		Button newStoryButton, viewStoryButton
	
	}

	public class Tape {     //лента
		Element post
		GroupElement suggestions

	}

	public class Footer {     // футер
		Button homeButton, searchButton, newPublishButton, activityButton, profileButton
	
	}
}
```
#### Страница выбора фото при создании публикации
```java
public class SelectPhotoPage {
	Button backButton, okButton
	Element imageElement
}
```
#### Страница оформления публикации
```java
public class NewPhotoPostPage{
	Button cancelButton, okButton
	Element imageEditElement
	Tab editTabs
	
}
```

### Тесты
```java
@Test
public void validLoginPageWithLoginPass(){ //2.1 Корректность работы страницы авторизации - первый вход
	LoginPage.open(url)
	LoginPage.findWebElement(loginInput, passInput, loginButton)
}

@Test
public void validLoginPageWithLoginPass(){ //2.2 Авторизация - корректные данные
	LoginPage.open(url)
	loginInput = "9088888888"
	passInput = "12nnn43ER"
	loginButton.tapButton()
	Assert.assertTrue(!LoginPage.isError());

}
```

## Выбор инструментов
Для начала разберемся, какие тесты нам нужны.
**Unit**, **integration** - безусловно, как обеспечение бесперебойной работы ядра системы.
**API** - тоже нужны, т.к. есть мобильное приложение.
**UI** - необходимы, интерфейс здесь важен.

Нужно либо комплексное решение, которое позволит покрыть тестами проект от и до, либо набор совместимых решений, дополняющих друг друга.
Проект развивается, значит решение должно быть рассчитано на (возможно) повышающуюся сложность, а также поддерживаться и дорабатываться разработчиками, чтобы в случае встраивания новых технологий не пришлось резко менять инструментарий для разработки тестов.
Ну и ещё один немаловажный (для меня, как для начинающего) критерий - хорошая документированность и/или активное сообщество, т.к. очень сложно начинать изучать автоматизацию с какого-то нишевого и малоизвестного решения. А для того, чтобы при желании можно было без больших сложностей сменить один из компонентов, желательно чтобы все они имели широкие возможности интеграции.
Я выбирала между TestCafe, JUnit/TestNG + Selenium - здесь Java/JS, который сейчас и изучаем. Плюс отчеты Allure.

### TestCafe 
Интересное решение для функциональных тестов: кроссбраузерное, быстро работающее, внешне достаточно простое в использовании. Опасение вызывает написание и поддержка сложных сценариев. Если их не будет, то это вполне подходящее решение, как мне кажется. 
### Selenium WebDriver
Имитация работы браузера, широкое распространение, активная доработка, простое использование. Да, медленный за счет низкоуровневой абстракции. Но для него есть множество расширений с более высоким уровнем абстракции, удобными "плюшками". Также большим плюсом являются возможности интеграции: WebDriver можно использовать совместно с огромным количеством других инструментов. Думаю, что именно с него стоило бы начать изучение автоматизации. 
Единственное, думаю это был бы не чистый WebDriver, а **Selenide**.
### JUnit/TestNG 
2 похожих фреймворка, в которых меня привлек понятный Java-синтаксис, использование аннотаций, простое подключение к IntelliJ IDEA.
TestNG показался интереснее за счет группировки и запуска групп тестов в xml - это нагляднее, поддержка зависимых тестов (при той же регистрации это полезно)
### Allure Report
Лёгкий, гибкий инструмент для построения отчетов. Можно использовать с Jenkins. Можно подключить практически к чему угодно - это тоже большой плюс.
Показывает выполнение тесткейса по шагам - наглядно.
Ну и удобная ролевая политика - пользователь из группы может просматривать только те данные, которые нужны именно ему.
