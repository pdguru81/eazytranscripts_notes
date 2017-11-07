## Welcome to EazyTranscripts :fire::fire:

Please head over to [url coming soon] for an idea of what this application currently looks like. To run a local instance, you will need to:
1. install docker and docker-compose on your machine and ensure docker is running.
2. run 'docker pull pdguru81/eazytranscripts' to get an image of the EazyTranscripts application. Please note that the file is ove 800MB in size and as such might take sometime to download.
3. create a docker-compose file using the template I have defined here below. I stronly recommend this approach as the system relies on the existence of envioronment variables that must be defined in these files.
4. Run 'docker-compose up' to get the application running.

Sample content of the docker-compose` file.
This gets an EazyTranscript image to run in a container that uses
environment variabes that have been set  in a <Filename>.env file also described below.

```
version: '3'
services:
  web:
    image: pdguru81/eazytranscripts:latest
    ports:
     - "5000:5000"
    env_file:
     - eazytranscripts.env
```

Required Environment Variables.
This should be set in a <filename>.env file that should exist in the same folder as 
the docker-compose.yml file for simplicity.

```
SECERET_KEY='YOUR OWN SECRET KEY. A LONG STRING OF RANDOM CHARACTERS'
CHARGE_STUDENTS='set to True or False in order to use payment api'
SEND_EMAILS='set to True or False in order to send emails to users'
MAIL_SERVER='set your orgs mail server. The default is smtp.googlemail.com'
MAIL_PORT=587 'Mail server port. Fixing this at 587 for now.''
MAIL_USE_TLS=True 'This value is fixed to True for now.'
MAIL_USERNAME='set your email username for the account used for sending emails'
MAIL_PASSWORD='set password for the account used for sending emails'
MAIL_DEFAULT_SENDER='Actual email address for sending emails.'
USE_DATABASE= 'True or False to use the database service you have installed using docker-compose'
DATABASE_PASSWORD='password of the database system to be used'
DATABASE_USER='The database user to be used with this application'
HOST='The application host name or ip.'
DOCKER_PORT=5000 #must match the value used in docker compose
EXPOSED_PORT=5000 #must match the value used in docker compose
HTTP_SCHEME="http"
EAZY_TRANSCRIPT_ENVIRONMENT="Application environment. Could be set to `local` or `prod`"
```

