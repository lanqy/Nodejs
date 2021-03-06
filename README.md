# nodejs
nodejs example

#### expressjs + mysql + swig template engine

### project structure

#####------- public
#####----------------css
#####---------------------bootstrap.css
#####----------------js
#####--------------------jquery-1.11.3.js
#####----------------images
#####----------------------
#####------- views
#####--------------common.html
#####--------------index.html
#####--------------users.html
	
------- app.js


------------------app.js
##### js
``` javascript
var express = require('express');
var path = require('path');
var mysql = require('mysql');
var cons = require('consolidate');
var bodyParser = require('body-parser');
var md5 = require('md5');
var app = express();

var connection = mysql.createConnection({
  host     : 'localhost',
  user     : 'root',
  password : '123456',
  database : 'cms'
});

app.use(bodyParser.json());
// in latest body-parser use like below.
app.use(bodyParser.urlencoded({ extended: true }));

// assign the swig engine to .html files
app.engine('html', cons.swig);

// set .html as the default extension
app.set('view engine', 'html');
app.set('views', path.join(__dirname, 'views'));
app.use(express.static(path.join(__dirname, 'public')));

app.get('/', function(req, res){
  res.render('index', {
    title: 'Consolidate.js'
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

app.post('/post', function(req, res){
  connection.query('insert into users SET add_time="' + new Date().getTime() + '", name="' + req.body.name + '", password="' + md5(req.body.password) + '"', function(err, rows){
    res.render('user', {
		title: 'insert',
		name: req.body.name,
		password: req.body.password
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

