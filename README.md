## Welcome to EazyTranscripts :fire::fire:

Please head over to http://45.55.83.205/ for an idea of what this application currently looks like. To run a local instance, you will need to:
1. install docker and docker-compose on your machine and ensure docker is running.
2. run 'docker pull pdguru81/eazytranscripts' to get an image of the EazyTranscripts application. Please note that the file is ove 800MB in size and as such might take sometime to download.
3. create a docker-compose file using the template I have defined here below. I stronly recommend this approach as the system relies on the existence of envioronment variables that must be defined in these files.
4. Run 'docker-compose up' to get the application running.

Sample content of the docker-compose` file. Please use this to get started. Note that you must:

1) Create an nginx config in the following folders server/conf.d/
2) put the data needed to run this system in some folder and replace /Users/pabel/Desktop/data with that.
This gets an EazyTranscript image to run in a container that uses
environment variabes that have been set  in a <Filename>.env file also described below.

```
version: '3'
services:
  server:
    restart: always
    image: nginx
    volumes:
      - ./server/conf.d:/etc/nginx/conf.d
      - static-assets:/usr/share/nginx/html/
    depends_on:
      - web
    links:
      - web
    ports:
      - "80:80"
  web:
    image: pdguru81/eazytranscripts:latest
    expose:
      - "8000"
    env_file:
     - eazytranscripts.env
    volumes:
      - /Users/pabel/Desktop/data:/data
      - static-assets:/home/root/static
    command: bash -c 'cp /data/**.png /home/root/static/images & gunicorn -c gunicorn.conf.py  -b :8000 -p PIDFILE  start:app --log-level=debug'

volumes:
  static-assets:
```

Required Environment Variables.
This should be set in a <filename>.env file that should exist in the same folder as 
the docker-compose.yml file for simplicity. All the fields are required. DATA_DIRECTORY
below is the folder with all the data needed to run the application. It must also contain
an image of the signature of the current registra at your school and an image of the 
school logo that will be used in the transcript.

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
EAZY_TRANSCRIPT_ENVIRONMENT="Application environment. Could be set to `local` or `prod`"
ENABLE_PAYMENT=True or False
PAYMENT_API_KEY="Flutterwave api key"
PAYMENT_MERCHANT_KEY="Flutterwave api key"
PAYMENT_API_SECRET="Flutterwave api secret"
PAYMENT_API_HOST="URL to the payment host for sending payment requests"
TEST_MODE=True
PAYMENT_MERCHANT_KEY="test"
TRANSCRIPT_COST=15
DATA_DIRECTORY="Folder where the data is"
TEST_MODE=True
PAYMENT_MERCHANT_KEY="test"
ACCREDITATION_ID="institution's id that is added to institution.csv"


```

Local Setup:
============
After having installed docker, export the student records from the databases or persistent storage systems at your institions to .csv files
on the machines where you have installed EazyTranscript. Please ensure that these .csv files are:
1) ReadOnly
2) Exist in priviledged and password protected folders only
3) The academic data are encrypted in the system.

This application expects the admin user to grant it permission to access files in a set of folders only.
The host machine(s) where this applcation is installed must have the following folders and files.

1) students.csv - a file containing student information
2) records.csv - a file containing records of all past and present students at this institution.
3) institutions.csv: a file containing information about the institution.
4) registrar.csv - a file containing information of all past and present registrars
5) course.csv - a file of all the courses ever offered by this institution.

Each of the files mentioned above must exist in one folder and must be mounted to 
the docker container as highlited in the docker-compose.yml file above. Also ensure to add the path to that folder as an envionment variable
that is visibile to EazyTranscript on the host machine. How? Check this tutorial: https://www.youtube.com/watch?v=2c-QWXAx_jI

The structure of the files used by EazyTanscript must have the following headers:
1) students.csv - student_id, student_name, student_phone, student_email, student_address, student_dob, admission_date, prev_school
2) records.csv - student_id, course_id, grade, reg_term, student_degree_program, level, student_year_classification
3) institutions.csv: institution_accreditation_id, institution_name,institution_location
4) registrar.csv - id, name, term
5) courses.csv - course_id, course_description, course_units


