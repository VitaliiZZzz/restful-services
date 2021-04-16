# RESTful services (express/restify)

## Що таке REST?

REST розшифровується як REpresentational State Transfer.

Відмінною особливістю сервісів REST є те, що вони дозволяють найкращим чином використовувати протокол HTTP.

## Коротко про HTTP

HTTP - це протокол, що дозволяє виконувати операції над різноманітними ресурсами (create, read, update, delete - CRUD)

Ресурс - це узагальнене поняття, тобто все, що ви хочете показати зовнішньому світові через вашу програму. Наприклад:
* Конкретна сутність (entity)
* Конкретна задача
* Список задач

Кожен ресурс має свій ідентифікатор (URI - універсальний ідентифікатор ресурсів). Наприклад:
* /books
* /students/Ivan
* /cars/Mercedes
* /trucks/MAN/1

## Запит-відповідь в HTTP

Для того, щоб з ресурсами виконати CRUD-дії необхідно їх знайти за URI та передати їм вхідні параметри. Для цього використовується запит. Щоб отримати результат CRUD-дій, використовуються відповіді.

![image1](https://github.com/VitaliiZZzz/restful-services/blob/master/doc/images/request_response.png)

Запити та відповіді для зручності будуються за певним шаблоном.

### Будова запиту в HTTP

HTTP визначає наступну структуру запиту (request):

```
GET /wiki/HTTP HTTP/1.1
Host: uk.wikipedia.org
User-Agent: firefox/5.0 (Linux; Debian 5.0.8; en-US; rv:1.8.1.7) Gecko/20070914 Firefox/2.0.0.7
Connection: close
(пустий рядок)
```

* рядок запиту (request line) - визначає тип повідомлення
* заголовки запиту (header fields) - характеризують тіло повідомлення, параметри передачі та інші відомості
* тіло повідомлення (body) - необов'язкове

### Будова відповіді в HTTP

HTTP визначає наступну структуру відповідного повідомлення (response):

```
HTTP/1.1 200 OK
Server: Apache
Content-Language: uk
Content-Type: text/html; charset=utf-8
Content-Length: 1234
(пустий рядок)
```

* рядок стану (status line), що включає код стану та повідомлення про причини
* поля заголовка відповіді (header fields)
* додаткове тіло повідомлення (body)

### Типи HTTP-запиту

Тип, який міститься в HTTP-запиті, вказує на необхідну операцію з обраним ресурсом. (Дані типи лише вказують, а не виконують дії)
```
GET /movies/3
POST /movies
PUT /movies/2
DELETE /movies/1
```
* GET: отримати детальну інформацію про ресурс (Read)
* POST: створити новий ресурс (Create)
* PUT: оновити існуючий ресурс (Update)
* DELETE: видалити ресурс (Delete)

## REST та його зв'язок з HTTP

Представлення (representation) – спосіб ілюстрації чи опису об’єкта. Наприклад, за допомогою атрибутів, за допомогою математичної моделі.

Стан (state) – ілюстрація чи опис об’єкта в зафіксований момент часу. Наприклад, конкретні значення атрибутів.

REST (Representation State Transfer) - це архітектурний стиль, що полягає в передачі стану представлення за допомогою HTTP. Увага: REST - це лише задумка, ідея, теорія.

REST API - це технічна реалізація архітектурного стилю REST.

RESTful service - це веб-сервіс, побудований на REST.

| Ознака | HTTP | REST |
| :--------: | :---: | :----: |
| Відправник / отримувач запитів | Браузер | Мобільний додаток, Десктопний додаток, Вебсайт і т.д. |
| Ідентифікація ресурсів | Через URI | Через URI |
| Характер ресурсів | HTML-, CSS-, Javascript- файли | JSON, XML, інші формати даних (“pure-data”) |
| Дієслова запиту до CRUD-операцій | GET, POST, PUT, DELETE | GET, POST, PUT, DELETE |
| Швидкість завантаження | Нижча | Вища |

З вищевказаного зрозуміло, що RESTful сервіси є досить гнучкими, тому їх тематика є різноманітною.

### Принципи стилю REST

* Ідентифікує ресурси як HTTP.
* Використовує дієслова, вже визначені протоколом HTTP, для доступу до CRUD-операцій.
* Формат в середині запиту-відповіді - будь-який (JSON, XML, тощо).
* Тематика сервісу - будь-яка, оскільки REST є досить гнучким.
* Найголовніше в сервісі - ресурси.

## Express.js vs Restify.js

### Express

Express.js - вільне і відкрите програмне забезпечення під ліцензією MIT, фреймворк веб-додатків для Node.js, є backend'ом в стеці MEAN. Автор TJ Holowaychuk.

### Restify

Restify.js - вільне і відкрите програмне забезпечення під ліцензією MIT. Цей фреймворк оптимізований для створення семантично коректних веб-служб RESTful, готових до масштабного використання. Автор Mark Cavage.

### Порівняння

|            | Плюси | Мінуси |
| :--------: | :---: | :----: |
| Express.js | 1. Низький поріг входження, Express - це практично стандарт Node.js-додатку. | 1. Всі ресурси необхідно створювати вручну і, в результаті, з'явиться багато повторного коду або гірше - власна бібліотека. |
|            | 2. Повна кастомізація | 2. Кожен ресурс вимагає тестування або простої перевірки на 500-ту помилку. |
|            | 3. Розвинена спільнота | 3. Рефакторинг стане болючим, тому що буде необхідно правити все і всюди. |
|            | 4. Потужна документація | 4. Нема стандартного підходу, потрібно шукати свій. |
|            |  | 5. Швидкість роботи |
| Restify.js | 1. Підтримка DTrace, якщо API працює на платформі, яка його підтримує. | Мінуси такі ж як і у Express (окрім швидкості роботи) - багато зайвої праці. |
|            | 2. Немає такого зайвого функціоналу, як шаблони і рендеринг. |  |
|            | 3. Обмеження частоти запитів (throttling). |  |
|            | 4. Підтримка SPDY |  |

![image2](https://github.com/VitaliiZZzz/restful-services/blob/master/doc/images/json_handings.png)

### Порівняння REST APIs

Фреймворки _Express.js_ та _Restify.js_ мають майже однаковий синтаксис, але різну логіку роботи “під капотом”.

***Створення сервісу (Application)***

Сервісу прийнято встановлювати ім’я “app”. Для сервісу прийнято вказувати одночасно і динамічний, і статичний порти. Пріоритет надається динамічному порту, але, якщо його немає, то використовується статичний порт.

_Express.js:_
```
const express = require("express");
const port = process.env.PORT || 8080
const app = express();
```

_Restify.js:_
```
const restify = require('restify');
const port = process.env.PORT || 8080;
const app = restify.createServer();
```

***Додавання проміжного обробника (Middleware)***

_Express.js:_
```
app.use(express.json()); // обробник JSON формату в тілі запитів-відповідей
app.use(logger); // логування
```

_Restify.js:_
```
app.use(restify.plugins.bodyParser()); // обробник JSON формату в тілі запитів-відповідей
app.use(logger); // логування
```

***Додавання GET кінцевих вершин(GET Endpoints)***

Express.js дозволяє вказувати статус відповіді та надсилати відповідь за допомогою одного конвеєра функцій, а Restify.js – ні.

_Express.js:_
```
app.get('/', (req, res) => {
    res.send("Hello World!");
});

app.get('/api/books', (req, res) => {
    res.send(books);
});

app.get('/api/books/:id', (req, res) => {
    const book = books.find(c => c.id === parseInt(req.params.id));
    if (!book) res.status(404).send('The book with given id is not found');
    res.send(book);
});
```

_Restify.js:_
```
app.get('/', (req, res) => {
	res.send('Hello World');
});

app.get('/api/books', (req, res) => {
	res.send(books);
});

app.get('/api/books/:id', (req, res) => {
	const book = books.find(b => b.id === parseInt(req.params.id));
	if (!book) {
		res.status(404);
		res.send('The book with given id is not found');
	}else
	res.send(book);
});
```

***Додавання POST кінцевих вершин(POST Endpoints)***

_Express.js:_
```
app.post('/api/books', (req, res) => {
    if (!req.body.name || req.body.name.length < 3) {
        res.status(400).send("You should give book name")
        return;
    }
    const book = {
        id: books.length + 1,
        name: req.body.name
    };
    books.push(book);
    res.send(book);
});
```

_Restify.js:_
```
app.post('/api/books', (req, res) => {
	if (!req.body.name || req.body.name.length < 3){
		// 400 Bad Request
		res.status(400);
		res.send('Name is required and should be minimum characters');
		return;
	}
	const book = {
		id: books.length + 1,
		name: req.body.name 
	}
	books.push(book);
	res.send(book);
});
```

***Додавання PUT кінцевих вершин(PUT Endpoints)***

_Express.js:_
```
app.put('/api/books/:id', (req, res) => {
    const book = books.find(c => c.id === parseInt(req.params.id));
    if (!book) res.status(404).send('The book with given id is not found');
    if (!req.body.name) res.status(400).send("You should give book name");
    book.name = req.body.name;
    res.send(book);
    });
```

_Restify.js:_
```
app.put('/api/books/:id', (req, res) => {
	const book = books.find(b => b.id === parseInt(req.params.id));
	if (!book) {
		res.status(404);
		res.send('The book with given id is not found');
		return;
	};
	if (!req.body.name){
		// 400 Bad Request
		res.status(400);
		res.send('You should give book name');
		return;
	}else{
		book.name = req.body.name;
		res.send(book);
	}
});
```

***Додавання DELETE кінцевих вершин(DELETE Endpoints)***

_Express.js:_
```
app.delete('/api/books/:id', (req, res) => {
    const book = books.find(c => c.id === parseInt(req.params.id));
    if (!book) res.status(404).send('The book with given id is not found');

    const index = books.indexOf(book);
    books.splice(index, 1);
    res.send(book);
});
```

_Restify.js:_
```
app.del('/api/books/:id', (req, res) => {
	const book = books.find(b => b.id === parseInt(req.params.id));
	if (!book) {
		res.status(404);
		res.send('The book with given id is not found');
		return;
	};
	const index = books.indexOf(book);
	books.splice(index, 1);
	res.send(book);
});
```

***Створення роутів (Routes)***

Створення роутів для Express.js відбувається трохи легше, ніж для Restify.js.

_Express.js:_
```
app
    .route('/api/books')
    .get((req, res) => {res.send(books)})
    .post((req, res) => {
    if (!req.body.name || req.body.name.length < 3) {
        res.status(400).send("You should give book name")
        return;
    }
    const book = {
        id: books.length + 1,
        name: req.body.name
    };
    books.push(book);
    res.send(book)});
```

_Restify.js:_
```
routerInstance.group('/api/books', function(router){
	router.get('/', (req, res) => {
		res.send(books);
	});
	router.post('/', (req, res) => {
	if (!req.body.name || req.body.name.length < 3){
		// 400 Bad Request
		res.status(400);
		res.send('Name is required and should be minimum characters');
		return;
	}
	const book = {
		id: books.length + 1,
		name: req.body.name 
	}
	books.push(book);
	res.send(book);
	});
routerInstance.applyRoutes(app);
```

***Запуск сервісу (Listening)***

_Express.js:_
```
app.listen(port, () => console.log(`Listening on port ${port}...`))
```

_Restify.js:_
```
app.listen(port, () => {
	console.log(`Listening port ${port}...`);
});
```
