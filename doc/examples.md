# RESTful services. Examples

[Повернутись на головну](https://github.com/VitaliiZZzz/restful-services/blob/master/README.md)

### Порівняння REST APIs побудованих за допомогою _Express.js_ та _Restify.js_

Код на _Express.js_ створював: Коренюк А.О.

Код на _Restify.js_ створював: Гутов В.В.

Фреймворки _Express.js_ та _Restify.js_ мають майже однаковий синтаксис, але різну логіку роботи “під капотом”. Для прикладу будемо використовувати таку "pure-data":
```
books = [
    {id: 1, name: 'book1'},
    {id: 2, name: 'book2'},
    {id: 3, name: 'book3'}
]
```

***Створення сервісу (Application)***

Сервісу прийнято встановлювати ім’я “app”. Для сервісу прийнято вказувати одночасно і порт, що задається сервером, і порт, що задається власне сервісом. Пріоритет надається порту, що задається сервером, але, якщо його немає, то використовується порт, що задається сервісом.

_Express.js:_
```
const express = require("express");
const port = process.env.PORT || 8080 // правило вибору порту
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

Express.js дозволяє вказувати статус відповіді та надсилати відповідь за допомогою одного конвеєра функцій, а Restify.js – ні. Крім того, якщо вказати декілька res.send() підряд, то Express.js виконає один res.send(), проігнорувавши інші, а Restify.js - видасть повідомлення про помилку:

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
    if (!book) res.status(404).send('The book with given id is not found'); // один конвеєр
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
		res.send('The book with given id is not found'); // без конвеєра
	}else
	res.send(book);
});
```

***Додавання POST кінцевих вершин(POST Endpoints)***

_Express.js:_
```
app.post('/api/books', (req, res) => {
    if (!req.body.name || req.body.name.length < 3) {
        res.status(400).send("You should give book name") // один конвеєр
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
		res.send('Name is required and should be minimum characters'); // без конвеєра
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
    if (!req.body.name) res.status(400).send("You should give book name"); // один конвеєр
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
		return; // вказано для того, щоб не виконувати наступний res.send()
	};
	if (!req.body.name){
		// 400 Bad Request
		res.status(400);
		res.send('You should give book name'); // без конвеєра
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
    if (!book) res.status(404).send('The book with given id is not found'); // один конвеєр

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
		res.send('The book with given id is not found'); // без конвеєра
		return;
	};
	const index = books.indexOf(book);
	books.splice(index, 1);
	res.send(book);
});
```

***Створення роутів (Routes)***

Створення роутів для Express.js відбувається трохи простіше, ніж для Restify.js.

_Express.js:_
```
app
    .route('/api/books')
    .get((req, res) => {res.send(books)})
    .post((req, res) => {
    if (!req.body.name || req.body.name.length < 3) {
        res.status(400).send("You should give book name") // один конвейєр
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
