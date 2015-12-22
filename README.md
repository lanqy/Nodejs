# nodejs
nodejs example

#### expressjs + mysql + swig template engine

### project structure

------- public
-----------|----css
 	  	|---bootstrap.css
-----------|----js
----------------|---jquery-1.11.3.js
-----------|----images
------------------|---
------- views
----------|---common.html
----------|---index.html
----------|---users.html
	
------- app.js


------------------app.js
##### js
``` javascript
var express = require('express');
var path = require('path');
var mysql = require('mysql');
var cons = require('consolidate');
var app = express();

var connection = mysql.createConnection({
  host     : 'localhost',
  user     : 'root',
  password : '123456',
  database : 'cms'
});

// assign the swig engine to .html files
app.engine('html', cons.swig);

// set .html as the default extension
app.set('view engine', 'html');
app.set('views', path.join(__dirname, 'views'));
app.use(express.static(path.join(__dirname, 'public')));

app.get('/', function(req, res){
  res.render('index', {
    title: 'welcome to expressjs'
  });
});

app.get('/users', function(req, res){
  connection.query('SELECT * FROM users', function(err, rows){
    res.render('users', {
		title: 'Users',
		users : rows
	});
  });
});

app.listen(3000);
console.log('Express server listening on port 3000');
```
##### html
------------------ views/index.html
``` html
<!doctype html>
<html>
	<head>
		<title>{{title}}</title>
	</head>
	
	<body>
		<h1>{{ title }}</h1>
	</body>
</html>
```
------------------ views/users.html
``` html
<!doctype html>
<html>
	<head>
		<meta charset="utf-8"/>
		<title>{{title}}</title>
	</head>
	<body>
		<h1>{{ title }}</h1>
		<ul>
		{% for user in users %}
		  <li{% if loop.first %} class="first"{% endif %}>
			{{ user.name }}
		  </li>
		{% endfor %}
		</ul>
	</body>
</html>
```

