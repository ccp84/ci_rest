#  Code Institute Rest Modules

## Setup django
* install django
`python -m pip install 'django<4' gunicorn`
* start new project
`django admin startproject NAME .` *DONT FORGET THE .*
* make env.py and gitignore files
* `python -m pip freeze > requirements.txt` (do any time you use pip - check for stuff you dont need)

## Setup AWS https://testdriven.io/blog/storing-django-static-and-media-files-on-amazon-s3/
* Install django-storages and boto3 to make AWS S3 storage work 
* Add storages to installed apps
```
INSTALLED_APPS = [
'django.contrib.admin',
'django.contrib.auth',
'django.contrib.contenttypes',
'django.contrib.sessions',
'django.contrib.messages',
'django.contrib.staticfiles',
'upload',
'storages',
]
```
* Static files settings
```
USE_S3 = os.getenv('USE_S3') == 'TRUE'

if USE_S3: # aws settings
AWS_ACCESS_KEY_ID = os.getenv('AWS_ACCESS_KEY_ID')
AWS_SECRET_ACCESS_KEY = os.getenv('AWS_SECRET_ACCESS_KEY')
AWS_STORAGE_BUCKET_NAME = os.getenv('AWS_STORAGE_BUCKET_NAME')
AWS_DEFAULT_ACL = 'public-read'
AWS_S3_CUSTOM_DOMAIN = f'{AWS_STORAGE_BUCKET_NAME}.s3.amazonaws.com'
AWS_S3_OBJECT_PARAMETERS = {'CacheControl': 'max-age=86400'} # s3 static settings
AWS_LOCATION = 'static'
STATIC_URL = f'https://{AWS_S3_CUSTOM_DOMAIN}/{AWS_LOCATION}/'
STATICFILES_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'
else:
STATIC_URL = '/staticfiles/'
STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')

STATICFILES_DIRS = (os.path.join(BASE_DIR, 'static'),)

MEDIA_URL = '/mediafiles/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'mediafiles')
```
* Add to env
```
USE_S3 = TRUE
AWS_ACCESS_KEY_ID = 
AWS_SECRET_ACCESS_KEY = 
AWS_STORAGE_BUCKET_NAME = 
```