Deployment on Railway
====================

.. index:: Railway

Script
------

Run these commands to deploy the project to Railway:

.. code-block:: bash
    railway init --name $PROJECT_SLUG 

    railway link --project $PROJECT_SLUG  # select workspace manually
    railway add -s $PROJECT_SLUG # hit enter to skip env
    
    railway add -d postgres
    railway add -d redis
    
    railway variables --set DJANGO_DEBUG=true
    railway variables --set DJANGO_SETTINGS_MODULE=config.settings.production
    railway variables --set DJANGO_SECRET_KEY="$(openssl rand -base64 64)"
    
    # Generating a 32 character-long random string without any of the visually similar characters "IOl01":
    railway variables --set DJANGO_ADMIN_URL="$(openssl rand -base64 4096 | tr -dc 'A-HJ-NP-Za-km-z2-9' | head -c 32)/"

    # Assign with AWS_ACCESS_KEY_ID
    railway variables --set DJANGO_AWS_ACCESS_KEY_ID=".."

    # Assign with AWS_SECRET_ACCESS_KEY
    railway variables --set DJANGO_AWS_SECRET_ACCESS_KEY=".."
    
    # Assign with AWS_STORAGE_BUCKET_NAME
    railway variables --set DJANGO_AWS_STORAGE_BUCKET_NAME=".."

    # Set your mail service API key
    railway variables --set SENDGRID_API_KEY=".."
    railway variables --set DEFAULT_FROM_EMAIL=".."

    # Set the following variables to the values of the corresponding environment variables in your local environment
    railway variables --set DJANGO_DEBUG=true
    railway variables --set DJANGO_SETTINGS_MODULE=config.settings.production
    railway variables --set DJANGO_SECRET_KEY="$(openssl rand -base64 64)"
    railway variables --set DJANGO_ADMIN_URL="admin"
    # railway variables --set DJANGO_ADMIN_URL="$(openssl rand -base64 4096 | tr -dc 'A-HJ-NP-Za-km-z2-9' | head -c 32)/"

    railway domain
    # alternatively:
    # railway domain api.yourcustomdomain.com
    railway variables --set DJANGO_ALLOWED_HOSTS="$(railway variables --json | python3 -c 'import sys, json; print(json.load(sys.stdin).get("RAILWAY_STATIC_URL"))')"

    railway variables --set DATABASE_URL="$(railway variables -s Postgres --json | python3 -c 'import sys, json; print(json.load(sys.stdin).get("DATABASE_URL"))')"
    railway variables --set REDIS_URL="$(railway variables -s Redis --json | python3 -c 'import sys, json; print(json.load(sys.stdin).get("REDIS_URL"))')"

    railway open
    # configure the github repository on the web interface
    # when deployed, open the app
    open "https://$(railway variables --json | python3 -c 'import sys, json; print(json.load(sys.stdin).get("RAILWAY_STATIC_URL"))')"

    railway ssh
    /opt/venv/bin/python manage.py createsuperuser
    /opt/venv/bin/python manage.py check --deploy

    railway open

