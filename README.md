
> Open the terminal in your favorite code editor and execute the following commands &nbsp;&nbsp;**: &nbsp;⟶**
> ```
> py -m venv env
> ./env/Scripts/activate
> pip install django
> django-admin startproject core .
> pip install djangorestframework
> ```

<br>

> <code>person_api/core/settings.py</code>
> ```
> INSTALLED_APPS = [
>     ...,
>     'rest_framework',
> ]
> ```

<br>

> <code>py manage.py startapp home</code>

<br>

<div>
    <h1>@api_view( )</h1>
</div>

<br>

<code>@api_view()</code> is a kind of **decorator** which converts your existing <ins>django (python) function into an api view function</ins>. <br>
&nbsp;It modifies the function behavior such that it becomes capable of handling all the APIs. <br>
&nbsp;It is used to create **custom endpoints** because of it's control over logic.

<br>

> <code>person_api/home/views.py</code>
> ```
> from rest_framework.decorators import api_view
> from rest_framework.response import Response
>
> @api_view(['GET'])
> def index(request):
>     courses = {
>         'course_name'       : 'Data Engineering',
>         'skills'            : ['Python', 'SQL', 'Django Rest Framework', 'PySpark', 'PowerBI', 'AWS'],
>         'course_provider'   : 'Learning to Earning',
>         'course_instructor' : 'Avinash Kumar Mishra'
>     }
>     return Response(courses)
> ```

<br>

> Create a folder <code>api</code> in the <code>person_api</code> folder <br>
> Then create 2 files &nbsp;&nbsp;&nbsp;➜&nbsp;&nbsp;&nbsp; <code>person_api/api/\_\_init__.py</code> &nbsp;&nbsp;&nbsp;&&nbsp;&nbsp;&nbsp; <code>person_api/api/urls.py</code>

<br>

<div>
    <h1>Routers</h1>
</div>

<br>

> <code>person_api/api/urls.py</code>
> ```
> from home.views import index
> from django.urls import path
>
> urlpatterns = [
>     path('index/', index),
> ]
> ```

<br>

> Now to register these urls in the <ins>**core**</ins> url &nbsp;:
> ```
> from django.contrib import admin
> from django.urls import path, include                       # <include> imported
>
> urlpatterns = [
>     path('api/', include('api.urls')),                      # this line added
>     path('admin/', admin.site.urls),
> ]
> ```

<br>

> Since the Django Rest Framework has some tables like <ins>**session**</ins> , etc. which come along with it's installation &nbsp;: <br>
> <code>py manage.py migrate</code>

<br>

> <code>py manage.py runserver</code> &nbsp;&nbsp;&nbsp;&nbsp;➜&nbsp;&nbsp;&nbsp;&nbsp; http://localhost:8000/api/index/

<br>

> Now try the same in **Postman**. <br>
> It's working as expected for <code>GET</code> operation. <br>
> But what about <code>POST</code> <br>
> Let's try it. <br>
> <code>POST</code> &nbsp;&nbsp;&nbsp;&nbsp;➜&nbsp;&nbsp;&nbsp;&nbsp; http://localhost:8000/api/index/ &nbsp;&nbsp;&nbsp;&nbsp;➜&nbsp;&nbsp;&nbsp;&nbsp; "**405 : Method \"POST\" not allowed.**"

<br>

> To resolve this problem &nbsp;&nbsp;&nbsp;&nbsp;➜&nbsp;&nbsp;&nbsp;&nbsp; <code>person_api/home/views.py</code> &nbsp;**:** <br>
> ```
> @api_view(['GET', 'POST'])
> ```

<br>

> <code>POST</code> &nbsp;&nbsp;&nbsp;&nbsp;➜&nbsp;&nbsp;&nbsp;&nbsp; http://localhost:8000/api/index/ &nbsp;&nbsp;&nbsp;&nbsp;➜&nbsp;&nbsp;&nbsp;&nbsp; No error. &nbsp;Same response as in the case of <code>GET</code>

<br>

> So looking at the response, how to distinguish which method is called <code>GET</code> or <code>POST</code> ? <br>
>
> <code>person_api/home/views.py</code>
> ```
> @api_view(['GET', 'POST'])
> def index(request):
>     courses = {
>         'course_name'       : 'Data Engineering',
>         'skills'            : ['Python', 'SQL', 'Django Rest Framework', 'PySpark', 'PowerBI', 'AWS'],
>         'course_provider'   : 'Learning to Earning',
>         'course_instructor' : 'Avinash Kumar Mishra'
>     }
>     if request.method == 'GET' :
>         print("You hit a GET method")
>     elif request.method == 'POST' :
>         print("You hit a POST method")
>     return Response(courses)
> ```

<br>

> Now hit those urls and check the logs in the **terminal**. <br>
> <code>GET</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;➜&nbsp;&nbsp;&nbsp;&nbsp; http://localhost:8000/api/index/ &nbsp;&nbsp;&nbsp;&nbsp;➜&nbsp;&nbsp;&nbsp;&nbsp; **You &nbsp;hit &nbsp;a &nbsp;GET &nbsp;method** <br>
> <code>POST</code> &nbsp;&nbsp;&nbsp;➜&nbsp;&nbsp;&nbsp;&nbsp; http://localhost:8000/api/index/ &nbsp;&nbsp;&nbsp;&nbsp;➜&nbsp;&nbsp;&nbsp;&nbsp; **You &nbsp;hit &nbsp;a &nbsp;POST &nbsp;method**

<br>

> ⭐ &nbsp;If you pass some data as the **body** in the <code>POST</code> / <code>PUT</code> / <code>PATCH</code> operation, you can catch it using <code>request.data</code> &nbsp;**:**
> ```
> {
>     "name" : "Aman",
>     "age" : 28
> }
> ```
> 
> 
> > <code>person_api/home/views.py</code>
> > ```
> > @api_view(['POST'])
> > def index(request):
> >     ...
> >     elif request.method == 'POST' :
> >         data = request.data
> >         print(data)
> >         print(data['name'])
> >         print(data['age'])
> >     ...
> > ```
> 
>
> You may see this output in the **backend** logs.
> 
>
> > But to show this data in the **frontend** too, you may again need to pass it in the <code>Response()</code> method and return it.
> > ```
> > @api_view(['POST'])
> > def index(request):
> >     ...
> >     elif request.method == 'POST' :
> >         data = request.data
> >         print(data)
> >         return Response(data)
> >     ...
> > ```

<br>

> However, if you want to send some data through <code>GET</code> , you will have to send it as parameter(s) :
>
> > http://localhost:8000/api/index/?search=Aman <br>
> > You can then catch it in the code using <code>request.GET.get('search')</code>
>
> > http://localhost:8000/api/index/?search=Isha%20Jha<br>
> > You can then catch it in the code using <code>request.GET.get('search')</code>

<br>

> <code>person_api/home/views.py</code>
> ```
> @api_view(['GET', 'POST', 'PUT'])
> def index(request):
>     courses = {
>         'course_name'       : 'Data Engineering',
>         'skills'            : ['Python', 'SQL', 'Django Rest Framework', 'PySpark', 'PowerBI', 'AWS'],
>         'course_provider'   : 'Learning to Earning',
>         'course_instructor' : 'Avinash Kumar Mishra'
>     }
>     if request.method == 'GET' :
>         params = request.GET.get('search')              # To catch data (search parameters) passed in GET operation
>         print(params)
>         print("You hit a GET method")
>     elif request.method == 'POST' :
>         data = request.data                             # To catch data (body) passed in POST operation
>         print(data)                                     # { 'name' : data['name'] , 'age' : data['age'] }
>         print("You hit a POST method")
>     elif request.method == 'PUT' :
>         data = request.data                             # To catch data (body) passed in PUT operation
>         print(data)
>         print("You hit a PUT method")
>     return Response(courses)
> ```


<br>

<div>
    <h1>Why Serializer ?</h1>
</div>

<br>

⭐ &nbsp;Database &nbsp;&nbsp;**⟷**&nbsp;&nbsp; <code>Django Query</code> &nbsp;&nbsp;**⟷**&nbsp;&nbsp; **QUERYSET** &nbsp;format &nbsp;&nbsp;**⟷**&nbsp;&nbsp; <code>Serializer</code> &nbsp;&nbsp;**⟷**&nbsp;&nbsp; **JSON** &nbsp;format

<br>

> - If you execute <code>Person.objects.all()</code> in the ORM and check it's datatype, you will get to see that the data is in <code>QuerySet</code> format, which cannot be exposed to the frontend. <br>
>
> - To expose data to the frontend in only JSON format &nbsp;&nbsp;&nbsp;**➜**&nbsp;&nbsp;&nbsp; **QUERYSET** &nbsp;format &nbsp;&nbsp;**⟷**&nbsp;&nbsp; <code>Serializer</code> &nbsp;&nbsp;**⟷**&nbsp;&nbsp; **JSON** &nbsp;format

<br>

> Let's now first create a **model** <code>Person</code> and then it's corresponding **model serializer** <code>PersonSerializer</code> and function based view.
> 
> <br>
>
> > <code>person_api/home/models.py</code>
> > ```
> > from django.db import models
> >
> > class Person(models.Model):
> >     name = models.CharField(max_length=100)
> >     age = models.IntegerField()
> > ```
>
> <br>
>
> > <code>person_api/home/serializers.py</code>
> > ```
> > class PersonSerializer(serializers.ModelSerializer):
> >
> >	    class Meta:
> >	        model = Person
> >	        fields = '__all__'                             # to include all fields of the Person model
> >		# fields = ['name', 'age']                     # to include specific fields of the Person model
> >	        # exclude = ['age']                            # to exclude the specific columns of the Person model
> > ```
>
> <br>
>
> > <code>person_api/core/settings.py</code>
> > ```
> > INSTALLED_APPS = [
> >     ...
> >    'rest_framework',
> >    'home',
> > ]
> > ```
>
> <br>
>
> > <code>py manage.py makemigrations</code> <br>
> > <code>py manage.py migrate</code>
>
> <br>
>
> > <code>person_api/home/views.py</code>
> > ```
> > from home.models import Person
> > from home.serializers import PersonSerializer
> >
> > @api_view(['GET', 'POST', 'PUT', 'PATCH', 'DELETE'])
> > def person(request):                                                              # /api/person/
> >     if request.method == 'GET':
> >         objs = Person.objects.all()
> >         serializer = PersonSerializer(objs, many = True)
> >         return Response(serializer.data)
> >   
> >     elif request.method == 'POST':
> >         data = request.data
> >         serializer = PersonSerializer(data = data)
> >         if serializer.is_valid():
> >             serializer.save()
> >             return Response(serializer.data)
> >         return Response(serializer.errors)
> >     
> >     elif request.method == 'PUT':
> >         data = request.data
> >         obj = Person.objects.get(id = data['id'])
> >         serializer = PersonSerializer(obj, data = data)
> >         if serializer.is_valid():
> >             serializer.save()
> >             return Response(serializer.data)
> >         return Response(serializer.errors)
> >     
> >     elif request.method == 'PATCH':
> >         data = request.data
> >         obj = Person.objects.get(id = data['id'])
> >         serializer = PersonSerializer(obj, data = data, partial = True)
> >         if serializer.is_valid():
> >             serializer.save()
> >             return Response(serializer.data)
> >         return Response(serializer.errors)
> >     else:
> >         data = request.data
> >         obj = Person.objects.get(id = data['id'])
> >         obj.delete()
> >         return Response({'message' : 'person deleted'})
> > ```
>
> <br>
>
> > <code>person_api/api/urls.py</code>
> > ```
> > from home.views import index, person
> > from django.urls import path
> >
> > urlpatterns = [
> >     path('index/', index),
> >     path('person/',person)
> > ]
> > ```

<br>

> > <code>GET</code> &nbsp;&nbsp;http://localhost:8000/api/person/ &nbsp;&nbsp;&nbsp;&nbsp;➜&nbsp;&nbsp;&nbsp;&nbsp; returns empty array <code>[]</code>
> <br>
> 
> > <code>POST</code> &nbsp;&nbsp;http://localhost:8000/api/person/
> > ```
> > {
> >     "name" : "A1",
> >     "age"  : 30
> > }
> > ```
> <br>
> 
> > <code>PUT</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;➜&nbsp;&nbsp;&nbsp;&nbsp; full update &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;➜&nbsp;&nbsp;&nbsp;&nbsp; need to update all the fields <br>
> > <code>PATCH</code> &nbsp;&nbsp;&nbsp;&nbsp;➜&nbsp;&nbsp;&nbsp;&nbsp; partial update &nbsp;&nbsp;&nbsp;&nbsp;➜&nbsp;&nbsp;&nbsp;&nbsp; need to update only the required felds along with the primary key
> <br>
> 
> > <code>DELETE</code> &nbsp;&nbsp;http://localhost:8000/api/person/ &nbsp;&nbsp;&nbsp;&nbsp;➜&nbsp;&nbsp;&nbsp;&nbsp; person deleted
> > ```
> > {
> >	    "id" : 4
> > }
> > ```

<br>

<ins>**Note**</ins> &nbsp;:
> - While creating a person using <code>POST</code> operation, don't pass <code>id</code> in the body because even if you pass, it will not accept. <br>
> Although it will not throw error, but it keeps a track record of the <code>id</code> column and strictly follow the **auto increment**. <br>
> 
> - Try to delete a person with a specific <code>id</code> and then try to create it with the same <code>id</code> <br>It will automatically take <code>id+1</code>

<br>

> However, if you will try to <code>GET</code> a person with a specific <code>id</code> in the URL http://localhost:8000/api/person/4/ , <br>
you will find <code>"Page Not Found"</code> error.

<br>

> Similarly, if you will try to <code>DELETE</code> a person with a specific <code>id</code> in the **URL** , you will find <code>"404 Not Found"</code> error
> <br><br>
>
> > <code>DELETE</code> &nbsp;&nbsp;http://localhost:8000/api/person/4/  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;➜&nbsp;&nbsp;&nbsp;&nbsp; **404 Not Found** : /api/person/4/
> <br>
>
> > <code>DELETE</code> &nbsp;&nbsp;http://localhost:8000/api/person/4/   &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;➜&nbsp;&nbsp;&nbsp;&nbsp; **404 Not Found** : /api/person/4/
> > ```
> > {
> >	    "id" : 4
> > }
> > ```

<br>

**Guess why ?** <br>
> ∎&nbsp;&nbsp; It is because you have made your own **custom endpoints** using <code>@api_view</code> <br>
> ∎&nbsp;&nbsp; The current view (person) is only handling requests to : &nbsp;<code>/api/person/</code> <br>
> ∎&nbsp;&nbsp; So while trying to access : &nbsp;<code>/api/person/{id}/</code> , the <code>person(request)</code> function doesn't take an <code>id</code> parameter from the URL <br>
> ∎&nbsp;&nbsp; Django doesn't know what to do with that <code>{id}</code> unless you define it in your **URLconf** and **view**

<br>

> Now the question is what is the solution to this problem ?
> 
> | Approach                    | Manual Routes Needed ? | Pros                         | Best For           |
> |-----------------------------|------------------------|------------------------------|--------------------|
> | `@api_view`                 | Yes                    | Full control over logic      | Custom endpoints   |
> | `ModelViewSet`              | No (uses routers)      | Less boilerplate, clean REST | Standard CRUD APIs |

<br>

<ins>**Solution - 1**</ins> &nbsp;:
> To handle <code>/api/person/{id}/</code> , you need a **separate view** for person detail operations (**GET** / **PUT** / **PATCH** / **DELETE** &nbsp;for a single object)
> <br><br>
>
> > <ins>**Step 1**</ins> &nbsp;&nbsp;➜&nbsp;&nbsp; Create <code>person_detail</code> view in the <code>person_api/home/views.py</code>
> > ```
> > @api_view(['GET', 'PUT', 'PATCH', 'DELETE'])
> > def person_detail(request, id):                                                  # /api/person/{id}/
> >     try:
> >         obj = Person.objects.get(id=id)
> >     except Person.DoesNotExist:
> >         return Response({'error': 'Person not found'}, status=404)
> > 
> >     if request.method == 'GET':
> >         serializer = PersonSerializer(obj)
> >         return Response(serializer.data)
> > 
> >     elif request.method == 'PUT':
> >         serializer = PersonSerializer(obj, data=request.data)
> >         if serializer.is_valid():
> >             serializer.save()
> >             return Response(serializer.data)
> >         return Response(serializer.errors)
> > 
> >     elif request.method == 'PATCH':
> >         serializer = PersonSerializer(obj, data=request.data, partial=True)
> >         if serializer.is_valid():
> >             serializer.save()
> >             return Response(serializer.data)
> >         return Response(serializer.errors)
> > 
> >     elif request.method == 'DELETE':
> >         obj.delete()
> >         return Response({'message': 'Person deleted'})
> > ```
> <br>
>
> > <ins>**Step 2**</ins> &nbsp;&nbsp;➜&nbsp;&nbsp; Update <code>person_api/api/urls.py</code>
> > ```
> > from home.views import index, person, person_detail
> > from django.urls import path
> >
> > urlpatterns = [
> >     path('index/', index),
> >     path('person/',person),
> >     path('person/<int:id>/', person_detail),
> > ]
> > ```

<br>

<ins>**Solution - 2**</ins> &nbsp;: 
> using <code>ModelViewSet</code> to simplify code and avoid manually writing separate views for detail and list endpoints
> <br><br>
>
> > When you use a <code>ModelViewSet</code> with a <code>DefaultRouter</code>, Django REST Framework **automatically** wires up all the standard CRUD routes for you, including &nbsp;: <br>
> > <code>/api/person/</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;➜&nbsp;&nbsp; list, create  <br>
> > <code>/api/person/{id}/</code> &nbsp;&nbsp;&nbsp;➜&nbsp;&nbsp; retrieve, update, partial_update, destroy  <br>
> <br>
>
> > <ins>**Step 1**</ins> &nbsp;&nbsp;➜&nbsp;&nbsp; Create the ViewSet in <code>person_api/home/views.py</code>
> > ```
> > from rest_framework.response import Response
> > from .models import Person
> > from .serializers import PersonSerializer
> > from rest_framework.viewsets import ModelViewSet
> >
> > 
> > class PersonViewSet(ModelViewSet):
> >     queryset = Person.objects.all()
> >     serializer_class = PersonSerializer
> > ```
> <br>
>
> > <ins>**Step 2**</ins> &nbsp;&nbsp;➜&nbsp;&nbsp; Update <code>person_api/api/urls.py</code> to use a **Router**
> > ```
> > from rest_framework.routers import DefaultRouter
> > from home.views import index, PersonViewSet
> > from django.urls import path, include
> >
> > router = DefaultRouter()
> > router.register(r'person', PersonViewSet, basename='person')
> > 
> > urlpatterns = [
> >     path('api/', include(router.urls)),
> > ]
> > ```
> <br>
> 
> ⭐ &nbsp;Now no need to manually define <code>@api_view</code> functions or URL patterns for each case.

<br>

> Now you can perform all these actions :
> 
> | HTTP Method     | Endpoint                       | Action          |
> |-----------------|--------------------------------|-----------------|
> | **GET**         | <code>/api/person/</code>      | List people     |
> | **POST**        | <code>/api/person/</code>      | Create person   |
> | **GET**         | <code>/api/person/{id}/</code> | Retrieve person |
> | **PUT**         | <code>/api/person/{id}/</code> | Full update     |
> | **PATCH**       | <code>/api/person/{id}/</code> | Partial update  |
> | **DELETE**      | <code>/api/person/{id}/</code> | Delete person   |

<br>

<div>
    <h1>Validation</h1>
</div>

<br>

> Let's add a validation for <code>age</code> field of the <code>Person</code> model &nbsp;**:**
> <br><br>
> 
> > <code>person_api/home/serializers.py</code>
> > ```
> > from rest_framework import serializers
> > from .models import Person
> >
> >
> >
> > class PersonSerializer(serializers.ModelSerializer):
> > 
> >     class Meta:
> >         model = Person
> >         fields = '__all__'                         # to include all fields of the Person model
> >         # fields = ['name', 'age']                 # to include specific fields of the Person model
> >         # exclude = ['age']                        # to exclude the specific columns of the Person model
> > 
> >     def validate(self, data):
> >         if data['age'] < 18:
> >             raise serializers.ValidationError('Age should be 18 or older.')
> >         return data
> > ```
> <br>
>
> > Another way of implementing validation is by using <code>validate_<field_name>()</code> method : <br>
> > <code>person_api/home/serializers.py</code>
> > ```
> > from rest_framework import serializers
> > from .models import Person
> >
> >
> >
> > class PersonSerializer(serializers.ModelSerializer):
> > 
> >     class Meta:
> >         model = Person
> >         fields = '__all__'
> > 
> >     def validate_age(self, data):
> >         if data < 18:
> >             raise serializers.ValidationError('Age should be 18 or older.')
> >         return data
> > ```
> <br>
>
> > <code>PATCH</code> &nbsp;&nbsp;&nbsp;http://localhost:8000/api/person/ &nbsp;&nbsp;or &nbsp;&nbsp;&nbsp;http://localhost:8000/api/person/5/
> > ```
> > {
> >     "id" : 5,
> >     "age" : 13
> > }
> > ```
> > <br>
> >
> > <code>POST</code> &nbsp;&nbsp;&nbsp;http://localhost:8000/api/person/
> > ```
> > {
> >     "name" : "Little Boy",
> >     "age" : 12
> > }
> > ```

<br>

> Now let's add validation for the <code>name</code> field &nbsp;**:**
> ```
> from rest_framework import serializers
> from .models import Person
>
>
> class PersonSerializer(serializers.ModelSerializer):
> 
>     class Meta:
>         model = Person
>         fields = '__all__'
> 
>     def validate(self, data):
>
>         special_characters = "!@#$%^&*()-+?_=,<>/"
>         if any(c in special_characters for c in data['name']):
>             raise serializers.ValidationError('Name cannot contain special characters.')
>
>         if data['age'] < 18:
>             raise serializers.ValidationError('Age should be greater than 18')
>         
>         return data
> ```	
> <br>
>
> > <code>POST</code> &nbsp;&nbsp;&nbsp;http://localhost:8000/api/person/
> > ```
> > {
> >    "name" : "R@hit",
> >    "age" : 15
> > }
> > ```
> > <code>"non_field_errors": ["Name cannot contain special characters."]</code>
> <br>
>
> > <code>POST</code> &nbsp;&nbsp;&nbsp;http://localhost:8000/api/person/
> > ```
> >  {
> >      "name" : "Rohit",
> >      "age" : 15
> >  }
> > ```
> > <code>"non_field_errors": ["Age should be 18 or older."]</code>
> <br>
>
> > <code>POST</code> &nbsp;&nbsp;&nbsp;http://localhost:8000/api/person/
> > ```
> >  {
> >      "name" : "Rohit",
> >      "age" : 25
> >  }
> > ```
> > ✔️ &nbsp;A person is successfully created with {"id" : 6, "name" : "Rohit", "age" : 25}
> <br>
>
> >  To update the <code>age</code> of the person with <code>id</code> = 6 &nbsp;**:** 
> >
> > <code>PATCH</code> &nbsp;&nbsp;&nbsp;http://localhost:8000/api/person/6/
> > ```
> >  {
> >      "age" : 26
> >  }
> > ```
> >
> > <code>KeyError: 'name'</code>
> >
> > ⭐ &nbsp;In a <code>PATCH</code> request, data will only include the fields that are being updated. <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;If <code>name"</code> is not in the request, <code>data['name']</code> will raise a <ins>**KeyError**</ins>.
> <br>
>
> >  Similarly, since <code>age</code> is not in the request, <code>data['age']</code> will raise a <ins>**KeyError**</ins>.
> >
> >  <code>PATCH</code> &nbsp;&nbsp;&nbsp;http://localhost:8000/api/person/6/
> > ```
> >  {
> >      "name" : "Rohit Sharma"
> >  }
> > ```
> > <code>KeyError: 'age'</code>



