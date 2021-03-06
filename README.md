## FOIA Hub

A consolidated FOIA (Freedom of Information Act) request hub, and resource
center.

* Receive and store a FOIA request, via a form or API.
* Send received FOIA requests on to the specified FOIA office.
* Send requestors a notice after the opening of a request, along with contact information for the FOIA office that is expected to handle the request.
* Send requestors a notice after the closing of a request, with the nature of the response and the necessary contact information to follow up on or appeal.

Some related repos:

* [foia](https://github.com/18F/foia) - Discussion forum and miscellaneous resources and scrapers.
* [foia-search](https://github.com/18F/foia-search) - Full text search API over FOIA requests and responses.
* (_Retired_) [foia-design](https://github.com/18F/foia-design) - original Jekyll-based HTML prototype.

## Setup

This is a Django app that uses [Postgres](http://www.postgresql.org/), and depends on [Python 3](https://docs.python.org/3/).

**Installing Python 3**: You may wish to use
[pyenv](https://github.com/yyuu/pyenv) to manage downloading Python 3. Or, you
can install `python3` through [Homebrew](http://brew.sh) (OS X) or `apt-get`
(Ubuntu).

**Installing Postgres**: You can `brew install postgres` (OS X) or `apt-get install postgresql` (Ubuntu).

The instructions below assume you use [virtualenv](http://virtualenv.readthedocs.org/en/latest/) [virtualenvwrapper](http://virtualenvwrapper.readthedocs.org/en/latest/) to manage dependencies.

### Project setup

Create an environment to install Python dependencies, with virtualenvwrapper.

```bash
mkvirtualenv --python=/path/to/python3 foia-hub
```

Note: You don't need to explicitly specify the Python version, especially if
you use pyenv + virtualenvwrapper. Running mkvirtualenv in that scenario will
'freeze' the currently active version of Python. 

Pull down the repo:

```bash
git clone https://github.com/18F/foia-hub
```

Install project requirements.

```bash
pip install -r requirements.txt
```

Note: On Ubuntu, I had to run the following before psycopg2 installed correctly:

```bash
sudo apt-get install libpq-dev python3-dev
```

Set your Django settings module.

```bash
export DJANGO_SETTINGS_MODULE=foia_hub.settings.dev
```


### Database set up

Create a `foia` database in Postgres:

```bash
createdb foia
```

If you you get a `could not connect to server` error, then add `export PGHOST=localhost` to your bash profile:

Next, create user `foia` with password <<PASSWORD>>:

```bash
psql -d foia -c "CREATE USER foia WITH PASSWORD '<<PASSWORD>>';"
```

where <<PASSWORD>> is a password of your choosing. 

Initialize your database schema:

```bash
django-admin.py syncdb
```

Finally, launch the server locally:

```
django-admin.py runserver
```

The site should be running at [`http://localhost:8000`](http://localhost:8000).

### Loading Data

#### Loading Agency FOIA Contact Data

Agency contact data is stored in another repository as YAML files. 

Clone the repository: 

```bash
git clone git@github.com:18F/foia.git
```

Then run the data loading script:

```bash
cd foia_hub/foia_hub/scripts
python load_agency_contacts.py <<path to foia repository>>/foia/contacts/data/
```

Note that the data repository is your local clone of:
[https://github.com/18F/foia/tree/master/contacts/data](https://github.com/18F/foia/tree/master/contacts/data])

Now if you access: [http://localhost:8000/api/agency/](http://localhost:8000/api/agency/]), you'll the list of agencies in JSON format. 


## Public domain

This project is in the worldwide [public domain](LICENSE.md). As stated in [CONTRIBUTING](CONTRIBUTING.md):

> This project is in the public domain within the United States, and copyright
> and related rights in the work worldwide are waived through the [CC0 1.0
> Universal public domain
> dedication](https://creativecommons.org/publicdomain/zero/1.0/).
>
> All contributions to this project will be released under the CC0 dedication.
> By submitting a pull request, you are agreeing to comply with this waiver of
> copyright interest.
