# Казус 1 - Copy Paste

### 📌 Бизнес информация и анализ

Компания обслужваща поръчки до близко и далече, е решила да вкара ред в книжата си. Фирмата се е насочила към имплементацията на сложен процес по трансформиране на данни и тяхното облагородавяне. Към момента те разполагат с огромен екселски файл, съдържащ над 5000 записа за техните транзакции направени през 2023 година. Желанието на клиентите ни е да се разработи процес по взимане на данните от CSV файла и обработката му в Snowflake

### Преди да започнете
Свалете [CSV файла от ТУК](https://fakecompanydata.s3.eu-central-1.amazonaws.com/ecommerce_orders.csv), които съдържа актуална информация за поръчките, покупките и продажбите извършени в компанията. Имайте предивд че файла съдържа много грешки - които на този етап можете да игнорирате.

Вашата задача е да:
1. Направете си нова база данни, наречена ECOMERSE_DB.
2. Копирайте данните от файл, в Snowflake таблица с име по избор.

За да изпълните задачата трябва да прочетете по подробно как се работи с COPY командата в Snowflake

### 💡 Изисквания към заданието

След като разполагаме с данните в Snowflake - ще установим че част от тях са грешни или непълни, клиентите ни имат нужда да нанесете следните промени в данните:

#### 🔥Коригиране на липсващи данни.

Част то записите имат липсващи стойности за Клиент, Адрес на доставка и метод за плащане
- Ако Адреса за доставка липсва, но статуса е Delivered - прехвърлете записа към отделна таблица, която да съдържа само и единствено такива доставки, готови за ревю, **td_for_review**
- Ако в записа липсва данни за клиента Customer_id , то тогава този запис трябва да бъде прехвърлен към таблица **td_suspisios_records**
- Ако липсва информация за платежния метод, коригирайте със стойност по подразбиране **Unknown**

#### 🔥Невалиден формат на данните

Някой от записите имат грешен формат, спрямо другите данни - ще откриете сами за какво говорим. Всеки запис с невалиден формат трябва да се прехвърли към таблица **td_invalid_date_format** , а финалните записи да се коригират, така че да бъдат в правилния формат.

#### 🔥Отрицателни или нулеви стойности за количество и цена
Изтриите всички редове, които съдържат невалидни стойности за цена и количество поръчана стока. Тези стойности винаги трябва да бъдат положителни. Прехвърлете заподозрените записи в специфична таблица, с име по ваш избор.

#### 🔥Невалидна отстъпка
Ако отстъпката е по малка от нула или по голяма от 50% то тя е невалидна, всички отстъпки над този диапазон, трябва да се трансформират във валидните си граници:
- за отрицателни стойности -- това е 0
- за стойности над 50% -- това е 50%

#### 🔥Неправилно калкулирана цена
Някой записи имат грешно калкулирана крайна цена, Цената се определя по формулата:
- количество стока * цена като се взима предвид и отстъпката

#### 🔥Неконсистентни статуси на поръчките
Някой записи са маркирани като ДОСТАВЕНИ, но в тях не фигурира адрес за доставк. Всички такива записи трябва да променят своя статус на **"Pending"**

#### 🔥Дуплицирани редове
Ако намерите повтарящи се записи, трябва да ги изтриете. 

Резултата от нашата работа е финална таблица **td_clean_records** съдържаща, корекции по описаните правила. Целта ни е да разработим необходимите SQL скриптове, които изпълнени последователно ще ни дадът резултат описан в заданието.

### 🚀 Предаване на заданието
Създейте ново, Github репозитори, **use-case-1-snowflake**. В него създайте единствена папка sql, в която да качите вашата версия на кода.
