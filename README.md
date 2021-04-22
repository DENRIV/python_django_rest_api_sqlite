# python_django_rest_api_sqlite GET,POST,PUT,DELETE,PATCH : hero 

GET — The most common option, returns some data from the API based on the endpoint you visit and any parameters you provide

POST — Creates a new record that gets appended to the database

PUT — Looks for a record at the given URI you provide. If it exists, update the existing record. If not, create a new record

DELETE — Deletes the record at the given URI

PATCH — Update individual fields of a record


python manage.py migrate

Create Super User

python manage.py createsuperuser

user : (leave blank to use <win.user>) : 

pass : ***


python manage.py runserver

localhost:8000

localhost:8000/admin

from django.db import models

class Hero(models.Model):

    name = models.CharField(max_length=60)
    
    alias = models.CharField(max_length=60)

    def __str__(self):
    
        return self.name

python manage.py makemigrations

  pip install djangorestframework

# mysite/settings.py:

INSTALLED_APPS = [

    # ...,
    
    'rest_framework',
    
]

# myapi/serializers.py

from rest_framework import serializers

from .models import Hero

class HeroSerializer(serializers.HyperlinkedModelSerializer):

    class Meta:
    
        model = Hero
        
        fields = ('name', 'alias')

# myapi/views.py

from rest_framework import viewsets

from .serializers import HeroSerializer

from .models import Hero

class HeroViewSet(viewsets.ModelViewSet):

    queryset = Hero.objects.all().order_by('name')
    
    serializer_class = HeroSerializer

# mysite/urls.py

from django.contrib import admin

from django.urls import path, include

urlpatterns = [

    path('admin/', admin.site.urls),
    
    path('', include('myapi.urls')),
    
 ]

# myapi/urls.py:

from django.urls import include, path

from rest_framework import routers

from . import views

router = routers.DefaultRouter()

router.register(r'heroes', views.HeroViewSet)

# Wire up our API using automatic URL routing.

# Additionally, we include login URLs for the browsable API.

urlpatterns = [

    path('', include(router.urls)),
    
    path('api-auth/', include('rest_framework.urls', namespace='rest_framework'))
    
]

# [TEST]

python manage.py runserver

localhost:8000

http://localhost:8000/heroes/

# input : 3 heroes 

http://127.0.0.1:8000/heroes/1/ 

http://127.0.0.1:8000/heroes/2/ 

http://127.0.0.1:8000/heroes/3/
 
http://127.0.0.1:8000/heroes/4/ 

# detail": "Not found."

# myapi/serializers.py

class HeroSerializer(serializers.HyperlinkedModelSerializer):

    class Meta:
    
        model = Hero
        
        fields = ('id','name', 'alias')

# Send a POST request

# send JSON to the API and it will create a new record in the database.

# Click in [POST] (down,right) & Click in [Raw data]

{

 "name":"Captain A",
 
 "alias":"Steve R."
 
}





