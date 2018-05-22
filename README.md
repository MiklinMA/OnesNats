# OnesNats

![Diagram](https://raw.githubusercontent.com/MiklinMA/DriverNats/master/Diagram.png)

Внешняя компонента для использования сервера очередей сообщений [NATS](https://github.com/nats-io/gnatsd).

В основе лежит библиотека враппер для интерфейса компонент, использующих технологию NativeAPI, для платформы 1С:Предприятия 8.x.

## Сборка

Для использования компоненты на старых версиях Windows (XP) без установки новых VC, необходимо производить сборку под фреймворк v100.

При открытии проекта в Visual Studio моложе чем 2010 набор инструментов и версию пакета SDK оставить без изменений.

При неободимости установить пакет [Visual Studio 2010 SDK](https://www.microsoft.com/en-us/download/details.aspx?id=2680) или среду Visual Studio Express 2010.

## Использование компоненты

Компонента использует обратный вызов для генерации событий в 1С. 

### Модуль управляемого/обычного приложения

```
перем Компонента экспорт;

Процедура ПриНачалеРаботыСистемы()
#Если клиент тогда
	ИмяКомпоненты = "ЛюбоеИмяКомпоненты";

	Если НЕ ПодключитьВнешнююКомпоненту(ПутьККомпонентеИлиМакетуСНей, ИмяКомпоненты, ТипВнешнейКомпоненты.Native) Тогда
		ВызватьИсключение "Подключить компоненту не удалось!";
	КонецЕсли;

	Компонента = Новый("AddIn." + ИмяКомпоненты + ".OnesNats");
	Компонента.Хост = "localhost"; // host
	Компонента.Порт = 4222;	// port

	Компонента.Подключить(); // connect
	Компонента.Слушать("тема.сообщения.*"); // subscribe
#конецЕсли
КонецПроцедуры

Процедура ОбработкаВнешнегоСобытия(Источник, Событие, Данные)
	Если Источник = "OnesNats" тогда
		// событие == тема подписки
		// данные == данные
	конецЕсли;
КонецПроцедуры
```

### Отправка сообщения с клиента
```
Компонента.Отправить("тема.сообщения.пример1", "данные"); // publish
```

## TODO

- [ ] Реализация request - publish с ожиданием результата.
- [ ] Атрибут connected - для отладки.

## Поддержка

По всем вопросам, можно писать в [почту](mailto:MiklinMA@gmail.com).
