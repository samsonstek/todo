# todo

This is a todo web application made by using flask and Flask-SQLAlchemy.

# what does it do?

It lists to do items

# About Flask
Installation

To install Flask: 
```bash
pip3 install flask
```

To use it in python script
```bash
from flask import Flask, render_template
```

# About Flask-SQLAlchemy
Installation 

To install Flask-SQLAlchemy: 
```bash
pip3 install flask-sqlalchemy
```
To use it in python script
```bash
from flask_sqlalchemy import SQLAlchemy
```
## Steps followed in creating this web application

### 1. Create and Run an application in Flask
- Created a flask application by the name app.py
- imported the Flask class
```python
from flask import Flask
```
- instantiate our application
```python
app = Flask(__name__)
```
By specifying __name__ it creates an application that gets named after the name of our file, which in our case is app.
- Tell our Flask application which endpoint to listen to for connections, We want to listen to the homepage route
```python
@app.route(‘/’)
```
- Create a route handler named index and that returns a template HTML file.
```python
def index():
     render_template(‘index.html’)
```
Flask offers a method called render template which specifies an HTML file to render to the user wherever a user visits this route.


- Create a folder named “templates”

- Create a file “index.html” inside “templates” folder
By default Flask looks for templates in a folder called templates

-Define the html document
Our goal is to have the following but we want the data to come from our server instead of being embedded in our HTML.
```python
<html>
	<head>
		<title>Todo App</title>
	</head>
	<body>
		<ul>
			<li> Todo 1</li>
			<li> Todo 2</li>
			<li> Todo 3</li>
			<li> Todo 4</li>
		</ul>
	</body>
</html>
```
- Pass in variables that we want to use in the template
```python
def index():
	return render_template('index.html', data=[{
	        'description' : 'Todo 1'
}, {
	        'description' : 'Todo 2'
}, {
	        'description' : 'Todo 3'
}])

```
- In the html, we use a Jinja For loop

```python
<html>
	<head>
		<title>Todo App</title>
	</head>
	<body>
	       <ul>
		{% for d in data}
		<li>{{ d.description }}</li>
		{% endfor %}
	     </ul>
	</body>
</html>
```
* Flask processes HTML templates using templating engine called Jinja which helps render HTML files to the user.

- Run Flask application which will launch a Development server on 127.0.0.1 and port 5000
```python
FLASK_RUN=app.py flask run
```
To enable Live reload(restart server and capture latest change), we can alternatively use the following and enable debug mode
```python
FLASK_RUN=app.py FLASK_DEBUG flask run
```

### 2. Incorporating Flask-SQLAlchemy and talk to a real database
- Import SQLAlchemy
```python
from flask_sqlalchemy import SQLAlchemy
```
- Link an instance of a database to interact with in SQLAlchemy to our Flask application
```python
db = SQLAlchemy(app)
```
db is an instance of the database
- Connect to our database from our Flask application by setting a configuration variable which is set on app.config
- Set a configuration variable( Flask-SQLAlchemy  expects a configuration variable ‘SQLALCHEMY_DATABASE_URI’) and equals to a string Flask-SQLAlchemy will understand to establish a connection
```python
app.config[‘SQLALCHEMY_DATABASE_URI’] = ‘the dialect://username:password@host:port//database_name’
```
In our case: 
```python
app.config[‘SQLALCHEMY_DATABASE_URI’] = ‘the dialect://postgresql:postgres@localhost:5432//example’
```
#### So We have created a ‘Todo’ Application, Configured a connection from SQLAlchemy to our Flask Application

### 3. Create a Class and define attributes
```python
class Todo(db.Model):
     __tablename__ = ‘todos’
     id = db.Column(db.Integer, primary_key = True)
     description = db.Column(db.String(), nullable = False)
```
- Modify our data to include data that comes from our database

```python
def index():
	return render_template('index.html', data=Todo.query.all())
```


#### This shows a model we use inside our web application

### 4 Create the tables in our database
- To detect the models, we use a method called db.create_all()
```python
db.create_all()
```
### 5 Insert a record to the created table
- Import the created Todo and db class on the terminal
```python
from flask app import Todo, db
```
- create a variable
```python
todo = Todo(description = ‘Todo 1’)
```
- Add that variable
```python
db.session.add(todo)
```
-Commit the added record to the database
```python
db.session.commit()
``` 

### 6 Define a dander repr method on the class defined for debugging SQLAlchemy objects(optional) 
```python
def __repr__(self):
    return f’<Todo ID: {self.id} {self.description}>’

