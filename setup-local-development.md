# Set up a local development environment

This guide will cover setting up a peregrine development environment.

TODO: The cloud settings are still to be determined.

## Set up Working Directory

Clone the repo locally.

```console
git clone https://github.com/uc-cdis/peregrine.git
```

Navigate to the cloned repository directory.

## Set up Python 3

The environment was tested with python 3.8 on WSL1.  You can use `bash` to install python 3 if it's not already available.

```console
sudo apt-get update
sudo apt-get install python3
```

### Set up a Virtual Environment

Set up a virtual environment for use with this project using `bash`:

```console
python3 -m venv py38-venv
. py38-venv/bin/activate
```

### Update Python Dependencies

You may need to reinstall graphene, so uninstall first:

```console
python3 -m pip uninstall graphene
```

Make sure to add in the both the dev-requirements and requirements.txt.

```console
python3 -m pip install -r requirements.txt
python3 -m pip install -r dev-requirements.txt
```

## Set up local Postgresql DB for testing

You can use a local postgresql for testing purposes.

### Set up local Postgresql DB on WSL

You can use `bash` to install postgres:

```console
sudo apt install postgresql-client-common
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sudo apt-get update
sudo apt-get install postgresql-12
```

Make sure the cluster is started:

```console
sudo pg_ctlcluster 12 main start
```

### Set up local Postgresql DB on Mac

If you're on mac, you can install postgres using brew:

```console
brew install postgres
```

### Set up DB and users for testing

You'll need to connect to the postgresql and add test users and databases.

#### Connect to Postgresql on WSL

Connect to the local postgresql server

```console
sudo -i -u postgres
psql
```

#### Connect to Postgresql on Mac

If you're on a mac, use the following to connect to postgres:

```console
brew services start postgres
psql postgres
```

#### Set up users and databases in psql

Initialize the database and users within the psql console:

```console
CREATE USER test WITH PASSWORD "test";
CREATE USER postgres WITH PASSWORD "test";
ALTER USER test WITH PASSWORD 'test';
ALTER USER postgres WITH PASSWORD 'test';
```

> You may need to use single quotes instead of double quotes depending on your shell environment.

Use `\q` to exit the psql shell.

## Create Private and Public Keys

Make sure you're in the [.\peregrine](https://github.com/uc-cdis/peregrine/) directory.
You can use bash and openssl to create a private and public key:

```console
mkdir -p tests/resources/keys;
cd tests/resources/keys;
openssl genrsa -out test_private_key.pem 2048;
openssl rsa -in test_private_key.pem -pubout -out test_public_key.pem; cd -
```

You can confirm the existence of the keys:

```console
ls ./tests/resources/keys
cat ./tests/resources/keys/test_private_key.pem
cat ./tests/resources/keys/test_public_key.pem
```

## Setup Unit Testing

At this point you should have the Python dependencies installed.  Be sure to check for `pytest` in the listed packages using:

```console
python3 -m pip freeze
```

Navigate to the [.\peregrine](https://github.com/uc-cdis/peregrine/) directory.

```console
python3 -m pytest ./tests 
```