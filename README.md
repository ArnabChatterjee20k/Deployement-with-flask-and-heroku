# Steps for deploying flask app with heroku,postgres and git

## Python setup
> pip psycopg2  gunicorn

> pip freeze > requirements.txt

> touch Procfile

> Inside the procfile add this command <b>web: gunicorn app:app</b>

## Repo setup
> git add .
> git commit -m "message"

## heroku 
> <b>heroku login -i</b> for loggin in the terminal itself
> To log in browser heroku login
> heroku create appname

## instance a database
> heroku addons:create heroku-postgresql:hobby-dev --app appname

## get the database URI
> heroku config --app appname
> Make sure that the uri is beginning with <b>postgresql://</b>
> Actually the heroku provide us the config starting with postgres not with postgresql.

<p>You will get DATABASE_URL as key and URI as the value . Copy this and set it in the system environment variable.</p>

# setting the uri from the environment in the app.py
> app.config['SQLALCHEMY_DATABASE_URI'] = os.environ.get("DATABASE_URI")
<li>We are not using DATABASE_URI as we cant overwrite the DATABASE_URL value and it is needed as flask-sqlalchemy supports uri beginning with <b>postgresql not postgres</b> </li>
<p>But here is a problem so follow the steps below</p>
<p>We are using environment variable as it is secure and the uri is already present in the heroku environment.</p>

## Correcting database uri in the environment variable from terminal
<li>Since the environment variable is having the uri starting with postgres not with postgresql so correct that by using terminal:- </li>

> heroku config:set DATABASE_URI=postgresql://

<li>We will see that we are not able to update so make a new key and add the updated uri starting with postgreql </li>

# Pushing the code
> If you want to get the url then use <b>heroku git:remote -a appname</b> else the next step.
> git push heroku master

## adding the table to the remote database
> U can use terminal or the dashboard in the heroku website
> heroku run python
```
    from app import db
    db.create_all()
    exit()
```
