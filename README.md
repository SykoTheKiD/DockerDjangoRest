## Django REST Application in Docker with Continuous Integration
### Includes
* Python 2.X and Python 3.X
* PostgreSQL
* Unicorn
* Nginx
* Travis CI Integration

### Setup
1. In the `docker-compose.yml` file in the **app** section under **command** enter your Django project name
2. In the `.env` file enter your Django project's secret key under **secret key**
3. Place the entire contents of your Django project inside the `app/` folder
4. In the `Dockerfile` inside the `app/` directory if you are using **Python 3** uncomment the `FROM python:3-onbuild` command and comment out the `FROM python:2-onbuild` we are using the **onbuild** version so it will automatically load the **requirements.txt** file
5. If you have a `requirements.txt` file append the contents of the `requirements.txt` in this project to yours.
6. In your `settings.py` file replace your 
    * **SECRET KEY** with `os.environ['SECRET_KEY']`
    * **DEBUG** with `True if os.getenv('DEBUG') == 'true' else False`


7. In the **DATABASES** section of `settings.py` replace your
    * **NAME** with `os.environ['DB_NAME']`
    * **USER** with `os.environ['DB_USER']`
    * **PASSWORD** with `os.environ['DB_PASS']`
    * **HOST** with `os.environ['DB_HOST']`
    * **PORT** with `os.environ['DB_PORT']`

You have made your Django project get the database and secret key data from the environment variables which are defined in the `.env` file from earlier.

8. Add `'*'`as one of your hosts in **ALLOWED_HOSTS** (don't know if this is needed, had to do this to get it to work on my machine)

### Travis CI Setup
1. Insert your Django project **SECRET_KEY** between the quotes under the **env** section
2. Enter a name for your database under **DB_NAME** and the same name inside the `psql` command under **before_script** 

### Docker Deployment
1. Run `docker-compose build`
2. After building run `docker-compose up`
3. In your browser go to your docker machine's IP address and you should see your app
4. If the CSS isn't loading run `docker-compose run app /usr/local/bin/python manage.py collectstatic` and then reload
