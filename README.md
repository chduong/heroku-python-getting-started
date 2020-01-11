# Python: Getting Started

A barebones Django app, which can easily be deployed to Heroku.

This application supports the [Getting Started with Python on Heroku](https://devcenter.heroku.com/articles/getting-started-with-python) article - check it out.

## Prepare the app
`$ git clone https://github.com/heroku/python-getting-started.git` <directory to save to, if not specified it goes to where ever you are cd>
`$ cd python-getting-started`

## Deploy the app
`$ heroku create`
`$ git push heroku master`

## Check app instance
`$ heroku ps:scale web=1`

## Open app
`$ heroku open`

## View logs
`heroku logs --tail`
- Control + C to stop viewing logs

## Define a Procfile
Example file looks like:

`web: gunicorn gettingstarted.wsgi --log-file -`

This declares a single process type, web, and the command needed to run it. The name web is important here. It declares that this process type will be attached to the HTTP routing stack of Heroku, and receive web traffic when deployed.

Procfiles can contain additional process types. For example, you might declare one for a background worker process that processes items off of a queue.

Microsoft Windows version file: Procfile.windows

Has content:

`web: python manage.py runserver 0.0.0.0:5000`

## Scale the app
Check the number of dynos running using:

`$ heroku ps`

Scale down:

`$ heroku ps:scale web=0`

Scale up from 0:

`$ heroku ps:scale web=1`

## Declare app dependencies
This requires Postgres installed to properly work.

`$ pip install -r requirements.txt`

Check pip's feature list:

`$ pip list`

## Run the app locally
The app is almost ready to start locally. Django uses local assets, so first, you’ll need to run collectstatic:

`$ python manage.py collectstatic`

Respond with “yes”.

Now start your application locally using `heroku local`, which was installed as part of the Heroku CLI.

If you’re on Microsoft Windows system, run this:

`heroku local web -f Procfile.windows`

If you’re on a Unix system, just use the default Procfile by running:

`heroku local web`

Your local web server will then start up:

[OKAY] Loaded ENV .env File as KEY=VALUE Format
2:28:11 PM web.1 |  [2018-10-12 14:28:11 -0500] [18712] [INFO] Starting gunicorn 19.9.0
2:28:11 PM web.1 |  [2018-10-12 14:28:11 -0500] [18712] [INFO] Listening at: http://0.0.0.0:5000 (18712)
2:28:11 PM web.1 |  [2018-10-12 14:28:11 -0500] [18712] [INFO] Using worker: sync
2:28:11 PM web.1 |  [2018-10-12 14:28:11 -0500] [18715] [INFO] Booting worker with pid: 18715

Just like Heroku, `heroku local` examines the `Procfile` to determine what to run.

Open `http://localhost:5000` with your web browser. You should see your app running locally.

To stop the app from running locally, go back to your terminal window and press Ctrl+C to exit.

## Push local changes

`$ pip install requests`

And then add it to your `requirements.txt` file:

`django` <br>
`gunicorn` <br>
`django-heroku` <br>
`requests` <br>

Modify hello/views.py so that it imports the requests module at the start:

`import requests`

Now modify the `index` method to make use of the module. Try replacing the current `index` method with the following code:

def index(request): <br>
    r = requests.get('http://httpbin.org/status/418') <br>
    print(r.text) <br>
    return HttpResponse('<pre>' + r.text + '</pre>') <br>
    
Now test locally:

`heroku local`

Visit your application at http://localhost:5000. You should now see the output of fetching http://httpbin.org/status/418, which is a lovely teapot:

    -=[ teapot ]=-

       _...._
     .'  _ _ `.
    | ."` ^ `". _,
    \_;`"---"`|//
      |       ;/
      \_     _/
        `"""`
Now deploy. Almost every deploy to Heroku follows this same pattern. First, add the modified files to the local git repository:

`git add .`

Now commit the changes to the repository:

`git commit -m "Demo"`

Now deploy, just as you did previously:

`git push heroku master`

Finally, check that everything is working:

`heroku open`

## Running Locally V2

Make sure you have Python 3.7 [installed locally](http://install.python-guide.org). To push to Heroku, you'll need to install the [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli), as well as [Postgres](https://devcenter.heroku.com/articles/heroku-postgresql#local-setup).

```sh
$ git clone https://github.com/heroku/python-getting-started.git
$ cd python-getting-started

$ python3 -m venv getting-started
$ pip install -r requirements.txt

$ createdb python_getting_started

$ python manage.py migrate
$ python manage.py collectstatic

$ heroku local
```

Your app should now be running on [localhost:5000](http://localhost:5000/).

## Deploying to Heroku

```sh
$ heroku create
$ git push heroku master

$ heroku run python manage.py migrate
$ heroku open
```
or

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy)

## Documentation

For more information about using Python on Heroku, see these Dev Center articles:

- [Python on Heroku](https://devcenter.heroku.com/categories/python)
