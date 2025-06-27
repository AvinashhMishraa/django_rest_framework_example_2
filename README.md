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
>     ●●●
>     'rest_framework',
> ]
> ```

<br>

> <code>py manage.py startapp home</code>

<br>

<h1>@api_view( )</h1>

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

<h1>Routers</h1>

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
> >     ●●●
> >     elif request.method == 'POST' :
> >         data = request.data
> >         print(data)
> >         print(data['name'])
> >         print(data['age'])
> >     ●●●
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
> >     ●●●
> >     elif request.method == 'POST' :
> >         data = request.data
> >         print(data)
> >         return Response(data)
> >     ●●●
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

<h1>Why Serializer ?</h1>

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
> >     ●●●
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
> >
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
> >     path('person/', person)
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

✅ &nbsp;<ins>**Solution - 1**</ins> &nbsp;:
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
> >     path('person/', person),
> >     path('person/<int:id>/', person_detail),
> > ]
> > ```

<br>

✅ &nbsp;<ins>**Solution - 2**</ins> &nbsp;: 
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

<h1>Validation</h1>

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
> <br>
> 
> > <code>person_api/home/serializers.py</code>
> > ```
> > from rest_framework import serializers
> > from .models import Person
> > 
> > 
> > class PersonSerializer(serializers.ModelSerializer):
> > 
> >     class Meta:
> >         model = Person
> >         fields = '__all__'
> > 
> >     def validate(self, data):
> > 
> >         special_characters = "!@#$%^&*()-+?_=,<>/"
> >         if any(c in special_characters for c in data['name']):
> >             raise serializers.ValidationError('Name cannot contain special characters.')
> > 
> >         if data['age'] < 18:
> >             raise serializers.ValidationError('Age should be greater than 18')
> >         
> >         return data
> > ```	
> <br>
>
> > <code>POST</code> &nbsp;&nbsp;&nbsp;http://localhost:8000/api/person/
> > ```
> > {
> >    "name" : "R@hit",
> >    "age" : 15
> > }
> > ```
> > ❌ &nbsp;<code>"non_field_errors": ["Name cannot contain special characters."]</code>
> <br>
>
> > <code>POST</code> &nbsp;&nbsp;&nbsp;http://localhost:8000/api/person/
> > ```
> >  {
> >      "name" : "Rohit",
> >      "age" : 15
> >  }
> > ```
> > ❌ &nbsp;<code>"non_field_errors": ["Age should be 18 or older."]</code>
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
> > ❌ &nbsp;<code>KeyError: 'name'</code>
> >
> > ⭐ &nbsp;In a <code>PATCH</code> request, data will only include the fields that are being updated. <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;If <code>name</code> is not in the request, <code>data['name']</code> will raise a <ins>**KeyError**</ins>.
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
> > ❌ &nbsp;<code>KeyError: 'age'</code>

<br>

<ins>**Solution**</ins>
> ⭐ &nbsp;use <code>.get('field_name')</code> to safely access optional fields, and fall back to the instance’s current value if it’s not present.
> <br><br>
>
>
> > <code>person_api/home/serializers.py</code>
> > ```
> > from rest_framework import serializers
> > from .models import Person
> >
> > 
> > class PersonSerializer(serializers.ModelSerializer):
> >
> >      class Meta:
> >          model = Person
> >          fields = '__all__'
> >  
> >      def validate(self, data):
> >  
> >          # Handle 'name' validation
> >          special_characters = "!@#$%^&*()-+?_=,<>/"
> >          name = data.get('name', self.instance.name if self.instance else None)
> >          if name and any(c in special_characters for c in name):
> >              raise serializers.ValidationError('Name cannot contain special characters.')
> >  
> >          # Handle 'age' validation
> >          age = data.get('age', self.instance.age if self.instance else None)
> >          if age < 18:
> >              raise serializers.ValidationError('Age should be 18 or older.')
> >          
> >          return data
> >  ```
> <br>
>
> **Explaination :**
>
> 🔸 &nbsp;<code>self.instance</code> exists if it's an update ( <code>PUT</code>, <code>PATCH</code> ) <br>
>
> 🔸 &nbsp;We fallback to self.instance.field if a field is missing from data ( i.e., not part of the <code>PATCH</code> ) <br>
>
> 🔸 &nbsp;This works for <code>POST</code>, <code>PUT</code>, and <code>PATCH</code> safely <br>
>
> 🔸 &nbsp;Let's see why it works ? <br>
> > 
> > ```
> > dictionary.get(key, default_value_if_key_missing)
> > ```
> > 
> > ∎ &nbsp;If <code>key</code> exists &nbsp;&nbsp;➜&nbsp;&nbsp; returns <code>dictionary[key]</code> <br>
> > 
> > ∎ &nbsp;If <code>key</code> doesn't exist &nbsp;&nbsp;➜&nbsp;&nbsp; returns the <code>default_value_if_key_missing</code> instead of throwing a <code>KeyError</code> <br>
> > 
> > ```
> > name = data.get(
> >    'name',                                              # Look for 'name' in the incoming request data
> >    self.instance.name if self.instance else None        # If 'name' not in data, use existing value from instance
> > )
> > ```

<br>

<h1>Serializing Foreign Key</h1>

<br>

> To create a **foreign key**, let's create a model <code>Color</code> first &nbsp;**:**
> <br><br>
>
> > <code>person_api/home/models.py</code>
> > ```
> > from django.db import models
> >
> >
> > class Color(models.Model):
> >     color_name = models.CharField(max_length=100)
> > 
> >     def __str__(self) -> str:
> >         return self.color_name
> >
> > 
> > class Person(models.Model):
> >     name = models.CharField(max_length=100)
> >     age = models.IntegerField()
> > ```
> <br>
>
> > <code>person_api/home/admin.py</code>
> > ```
> > from django.contrib import admin
> > from home.models import Color
> > 
> >
> > admin.site.register(Color)
> > ```
> <br>
>
> <code>py manage.py makemigrations</code> <br>
> <code>py manage.py migrate</code>

<br>

> Now let's add one **Foreign Key** <code>color</code> in the <code>Person</code> model.
> <br><br>
>
> > <code>person_api/home/models.py</code>
> > ```
> > from django.db import models
> > 
> > 
> > class Color(models.Model):
> >     color_name = models.CharField(max_length=100)
> > 
> >     def __str__(self) -> str:
> >         return self.color_name
> > 
> > 
> > class Person(models.Model):
> >     color = models.ForeignKey(Color, null=True, blank=True, on_delete=models.CASCADE, related_name="color")
> >     name = models.CharField(max_length=100)
> >     age = models.IntegerField()
> > ```
> <br>
>
> <code>py manage.py makemigrations</code> <br>
> <code>py manage.py migrate</code>

<br>

> <code>py manage.py runserver</code> <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;∎ &nbsp;&nbsp;Now after running the server, you will find all persons created till now have a <code>color</code> field attached to it. <br>
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;∎ &nbsp;&nbsp;However there is no value assigned to this field. <br>
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;∎ &nbsp;&nbsp;Let's create some colors through <ins>**admin**</ins>. You can also use POSTMAN. <br>
>
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;◆ &nbsp;<code>py manage.py createsuperuser</code>  <br><br>
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;● &nbsp;&nbsp;username &nbsp;&nbsp; - &nbsp;&nbsp; XXXXX  <br>
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;● &nbsp;&nbsp;email &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - &nbsp;&nbsp; XXXXX  <br>
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;● &nbsp;&nbsp;password &nbsp;&nbsp; - &nbsp;&nbsp; XXXXX  <br>
>
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;◆ &nbsp;Now let's run the server &nbsp;-&nbsp; <code>py manage.py runserver</code> <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;And hit the url &nbsp;-&nbsp; http://localhost:8000/admin/

<br>

> Now let's add some color to a couple of persons from the <ins>**admin**</ins> side &nbsp;&nbsp;:
> 
> <br>
>
> > <code>person_api/home/admin.py</code>
> > ```
> > from django.contrib import admin
> > from home.models import Color, Person
> > 
> > admin.site.register(Color)
> > admin.site.register(Person)
> > ```
> <br>
> 
> > Go to http://localhost:8000/admin/ and select a color from the dropdown for a couple of persons.
> <br>
> 
> > Verify it &nbsp;&nbsp;&nbsp;➜&nbsp;&nbsp;&nbsp; <code>GET</code> &nbsp;&nbsp;http://localhost:8000/api/person/
> > ```
> > [
> >     {
> >         "id": 1,
> >         "name": "A1",
> >         "age": 25,
> >         "color": 1
> >     },
> >     {
> >         "id": 2,
> >         "name": "A2",
> >         "age": 27,
> >         "color": null
> >     },
> >     {
> >         "id": 3,
> >         "name": "A3",
> >         "age": 40,
> >         "color": 2
> >     }
> > ]
> > ```

<br>

<h3>using <code>depth</code> for <ins>serializing foreign keys</ins> &nbsp;⭐</h3>

> Now how to know which color the person <ins>**A1**</ins> likes from such an output. <br>
> For this, the color <ins>**id**</ins> needs to be replaced by color <ins>**name**</ins>.     <br>
> Let's see how ?
> 
> <br>
> 
> > But first let's remove persons with no color. <br>
> > To do this, just replace <code>objs = Person.objects.all()</code> with <code>objs = Person.objects.filter(color__isnull = False)</code>
> >
> > <code>person_api/home/views.py</code>
> > ```
> > 
> > ●●●
> > 
> > 
> > # /api/person/
> > @api_view(['GET', 'POST', 'PUT', 'PATCH', 'DELETE'])
> > def person(request):
> >     if request.method == 'GET':
> >         objs = Person.objects.filter(color__isnull = False)                  # objs = Person.objects.all()
> >         serializer = PersonSerializer(objs, many = True)
> >         return Response(serializer.data)
> >     
> >     elif request.method == 'POST':
> >         ●●●
> >     
> >     elif request.method == 'PUT':
> >         ●●●
> >     
> >     elif request.method == 'PATCH':
> >         ●●●
> >		
> >     else:
> >	        ●●●
> > ```
> <br>
> 
> > Now to identify the **color name** for each person, just add <code>depth</code> in the <code>Meta</code> class of the <code>PersonSerializer</code> class.
> > <br>
> >
> > <code>person_api/home/serializers.py</code>
> > ```
> > from rest_framework import serializers
> > from .models import Person
> >
> >
> > class PersonSerializer(serializers.ModelSerializer):
> >
> >     class Meta:
> >         model = Person
> >         fields = '__all__'
> >         depth = 1
> > 
> >     def validate(self, data):
> >         ●●●
> > ```
>
> <br>
> 
> <code>GET</code> &nbsp;&nbsp;http://localhost:8000/api/person/
> ```
> [
>     {
>         "id": 1,
>         "name": "A1",
>         "age": 25,
>         "color": {
>             "id": 1,
>             "color_name": "RED"
>         }
>     },
>     {
>         "id": 3,
>         "name": "A3",
>         "age": 40,
>         "color": {
>             "id": 2,
>             "color_name": "BLUE"
>         }
>     },
>     ●●●
> ]
> ```

<br>

> But what if we want only the <code>color_name</code> in the output and not any other field of the <code>Color</code> model? <br>
>
> In that case, we can't use <code>depth</code> because it serializes <ins>**all the fields**</ins> of the <code>Color</code> model. &nbsp;⚠️ <br>
>
> We have to remove <code>depth = 1</code> from <code>PersonSerializer</code> class and then create a class of <code>ColorSerializer</code> with <code>fields = ['color_name']</code> in the <code>meta</code> class which will help Django Rest Framework to understand the structure of the data that needs to be passed ?
> <br>
> 
> > <code>person_api/home/serializers.py</code>
> > ```
> > from rest_framework import serializers
> > from .models import Person, Color
> > 
> > 
> > class ColorSerializer(serializers.ModelSerializer):
> >     
> >     class Meta:
> >         model = Color
> >         # fields = '__all__'                # fields = ['color_name', 'id']
> >         fields = ['color_name']             # if you want to only show color name in the nested json output
> > 
> > 
> > class PersonSerializer(serializers.ModelSerializer):
> > 
> >     # color = ColorSerializer()                                  # nested serializers are required by default
> >     color = ColorSerializer(required=False, allow_null=True)
> > 
> >     class Meta:
> >         model = Person
> >         fields = '__all__'
> >         # depth = 1
> > 
> >     def validate(self, data):
> >         ●●●
> > ```
> <br>
> 
> > <code>GET</code> &nbsp;&nbsp;&nbsp;&nbsp;http://localhost:8000/api/person/ <br>
> >
> > &nbsp;**Output &nbsp;**:
> > ```
> > [
> >     {
> >         "id": 1,
> >         "color": {
> >             "color_name": "RED"
> >         },
> >         "name": "A1",
> >         "age": 25
> >     },
> >     {
> >         "id": 3,
> >         "color": {
> >             "color_name": "BLUE"
> >         },
> >         "name": "A3",
> >         "age": 40
> >     },
> >     ●●●
> > ]
> > ```
> <br>
>
> > It is important to note that the <code>color</code> field is **optional** but if you pass it in the body, you have to pass it as a <ins>nested JSON structure</ins> because the <code>color</code> variable in the <code>PersonSerializer</code> above is actually a **nested serializer**.
> >
> > <br>
> >
> > <code>POST</code> &nbsp;&nbsp;&nbsp;&nbsp;http://localhost:8000/api/person/
> > ```
> > {
> >     "name": "Abc",
> >     "age": 25,
> > }
> > ```
> >
> >
> > Person successfully created
> > ```
> > {
> >     "id": 21,
> >     "color": 1,
> >     "name": "Abc",
> >     "age": 25
> > }
> > ```
> >
> <br>
>
> > <code>POST</code> &nbsp;&nbsp;&nbsp;&nbsp;http://localhost:8000/api/person/
> > ```
> > {
> >     "name": "Xyz",
> >     "age": 21,
> >     "color": 1
> > }
> > ```
> >
> >
> > **Output &nbsp;:**
> > ```
> > {
> >     "color": {
> >         "non_field_errors": [
> >             "Invalid data. Expected a dictionary, but got int."
> >         ]
> >     }
> > }
> > ```
> <br>
> 
> > <code>POST</code> &nbsp;&nbsp;&nbsp;&nbsp;http://localhost:8000/api/person/
> > ```
> > {
> >     "name": "Xyz",
> >     "age": 21,
> >     "color": {
> >         "id": 1
> >     }
> > }
> > ```
> >
> >
> > **Output &nbsp;:**
> > ```
> > {
> >     "color": {
> >         "color_name": [
> >             "This field is required."
> >         ]
> >     }
> > }
> > ```
> <br>
> 
> > <code>POST</code> &nbsp;&nbsp;&nbsp;&nbsp;http://localhost:8000/api/person/
> > ```
> > {
> >     "name": "Xyz",
> >     "age": 21,
> >     "color": {
> >         "id": 1,
> >         "color_name": "RED"
> >     }
> > }
> > ```
> >
> >
> > **Output &nbsp;:**
> > ```
> > AssertionError: The `.create()` method does not support writable nested fields by default.
> >
> > Write an explicit `.create()` method for serializer `home.serializers.PersonSerializer`, 
> > or set `read_only=True` on nested serializer fields.
> > ```
> <br>
>
> > Let's try to change the color of the person 2 from **blue** to **red**. <br>
> > <code>PATCH</code> &nbsp;&nbsp;&nbsp;&nbsp;http://localhost:8000/api/person/  &nbsp;&nbsp;&nbsp;&nbsp;http://localhost:8000/api/person/3/
> > ```
> > {
> >     "id": 3,
> >     "age": 40,
> >     "color": 1
> > }
> > ```
> > 
> > 
> > **Output &nbsp;:**
> > ```
> > {
> >     "color": {
> >         "non_field_errors": [
> >             "Invalid data. Expected a dictionary, but got int."
> >         ]
> >     }
> > }
> > ```
> <br>
>
> > <code>PATCH</code> &nbsp;&nbsp;&nbsp;&nbsp;http://localhost:8000/api/person/ &nbsp;&nbsp;&nbsp;&nbsp;http://localhost:8000/api/person/3/
> > ```
> > {
> >     "id": 3,
> >     "age": 40,
> >     "color": {
> >         "id": 1
> >     }
> > }
> > ```
> > 
> >
> > **Output &nbsp;:**
> > ```
> > AssertionError: The `.update()` method does not support writable nested fields by default.
> >
> > Write an explicit `.update()` method for serializer `home.serializers.PersonSerializer`, 
> > or set `read_only=True` on nested serializer fields
> > ```
> <br>
> 
> > <code>PATCH</code> &nbsp;&nbsp;&nbsp;&nbsp;http://localhost:8000/api/person/ &nbsp;&nbsp;&nbsp;&nbsp;http://localhost:8000/api/person/3/
> > ```
> > {
> >     "id": 3,
> >     "age": 40,
> >     "color": {
> >         "id": 1,
> >         "color_name": "RED"
> >     }
> > }
> > ```
> > 
> >
> > **Output &nbsp;:**
> > ```
> > AssertionError: The `.update()` method does not support writable nested fields by default.
> >
> > Write an explicit `.update()` method for serializer `home.serializers.PersonSerializer`, 
> > or set `read_only=True` on nested serializer fields
> > ```
> <br>
>
> > <code>DELETE</code> &nbsp;&nbsp;http://localhost:8000/api/person/
> > ```
> > {
> >     "id": 20
> > }
> > ```
> > 
> > 
> > **Output** &nbsp;&nbsp;➜&nbsp;&nbsp; <code>{"message": "person deleted"}</code>

<br>

> As you can see you are able to **get** and **delete** persons <ins>but not **create** and **update** them</ins> using **POSTMAN**, you may ask <ins>**why this is happening**</ins> ? However you can still do all CRUD operations through <code>admin</code>.
> 
> <br>
> 
> > In your <code>PersonSerializer</code>, you made <code>color = ColorSerializer(required=False, allow_null=True)</code> — this makes it a **nested serializer**.
> >
> > 
> > DRF treats this as an **embedded object**. But by default, <code>ModelSerializer</code> **doesn't know how to create or update related objects through a nested serializer** unless you override <code>.create()</code> and <code>.update()</code> methods <ins>explicitly</ins>.
> >
> > 
> > | Situation                                       | Fix                                                     |
> > | ----------------------------------------------- | ------------------------------------------------------- |
> > | You send `"color": { "id": 1, ... }` in request | You **must override** `.create()` and `.update()`       |
> > | You send `"color": 1`                           | ✅ Use `PrimaryKeyRelatedField` (simpler, DRF-friendly) |
> <br>
>
> > So if you want to keep using <code>ColorSerializer()</code> and nested format like :
> > ```
> > "color": {
> >     "id": 1,
> >     "color_name": "RED"
> > }
> > ```
> > Then you have to write your own custom <code>.create()</code> and <code>.update()</code> methods for <code>POST</code> and <code>PUT</code>/<code>PATCH</code> requests respectively.
> <br>
> 
> - However the best way to solve this problem is by using <code>PrimaryKeyRelatedField</code> instead of nested <code>ColorSerializer</code>. ⭐<br>
> This is cleaner and simpler <ins>when you only want to link an existing <code>Color</code> (not create/update nested <code>Color</code>).</ins> <br>
> 
>
> - The bottom line is that unless you are actually modifying <code>Color</code> fields inside the <code>Person</code> form/API, prefer <code>PrimaryKeyRelatedField</code> for clean & simple REST.
> 
> 
> > Remove the <code>ColorSerializer</code> class and change this in <code>PersonSerializer</code> class:
> > ```
> > color = ColorSerializer(
> >    required=False,                                # optional
> >    allow_null=True                                # optional
> > )
> > ```
> >
> > 
> > To
> > ```
> > color = serializers.PrimaryKeyRelatedField(
> >    queryset=Color.objects.all(),
> >    required=False,                                # optional
> >    allow_null=True                                # optional
> > )
> > ```
> <br>
> 
> **Solution &nbsp;:**
> > <code>person_api/home/serializers.py</code>
> > ```
> > from rest_framework import serializers
> > from .models import Person, Color
> > 
> > 
> > class PersonSerializer(serializers.ModelSerializer):
> >     
> >     color = serializers.PrimaryKeyRelatedField(
> >         queryset=Color.objects.all(),
> >         required=False,
> >         allow_null=True
> >     )
> > 
> >     class Meta:
> >         model = Person
> >         fields = '__all__'
> >         # depth = 1
> > 
> >     def validate(self, data):
> >         ●●●
> > ```
> <br>
> 
> Now try all **CRUD** APIs. And you will be able to do all of that.
> <br>
> 
> > <code>POST</code> &nbsp;&nbsp;http://localhost:8000/api/person/
> > ```
> > {
> >     "name": "Xyz",
> >     "age": 21,
> >     "color": 1
> > }
> > ```
> >
> >
> > Person successfully created
> > ```
> > {
> >     "id": 22,
> >     "color": 1,
> >     "name": "Xyz",
> >     "age": 21
> > }
> > ```
> <br>
>
> > <code>PATCH</code> &nbsp;&nbsp;http://localhost:8000/api/person/
> > ```
> > {
> >     "id": 22,
> >     "color": null
> > }
> > ```
> >
> >
> > Person successfully updated
> > ```
> > {
> >     "id": 22,
> >     "color": null,
> >     "name": "Xyz",
> >     "age": 21
> > }
> > ```

<br>

> The best way to control the order of fields of a Model in a Django Rest Framework API is through <ins>**serializer**</ins>.
> 
> <br>
> 
> > Just remove <code>fields = '\_\_all__'</code> and manually specify field order in the <code>PersonSerializer</code> class. <br>
> > 
> > 
> > <code>person_api/home/serializers.py</code>
> > ```
> > ●●●
> > 
> > class PersonSerializer(serializers.ModelSerializer):
> > 
> >     color = serializers.PrimaryKeyRelatedField(
> >         queryset=Color.objects.all(),
> >         required=False,
> >         allow_null=True
> >     )
> > 
> >     class Meta:
> >         model = Person
> >         # fields = '__all__'
> >         fields = ['id', 'name', 'age', 'color']            # Custom order
> >
> >     def validate(self, data):
> >         ●●●
> > ```
> <br>
> 
> <code>GET</code> &nbsp;&nbsp;http://localhost:8000/api/person/
> ```
> [
>     {
>         "id": 1,
>         "name": "A1",
>         "age": 25,
>         "color": 1
>     },
>     {
>         "id": 3,
>         "name": "A3",
>         "age": 40,
>         "color": 2
>     },
>     ●●●
> ]
> ```

<br>

<h3>Difference &nbsp;between &nbsp;using &nbsp;- &nbsp;&nbsp;<ins><code>depth</code> in a serializer class</ins> &nbsp;&nbsp;&&nbsp;&nbsp; <ins>nested serializer</ins></h3>

<br>

> <code>depth</code> is a shortcut to include **related models** in the API response automatically. It is used in a **serializer class** to serialize **all fields** of different related models. It automatically follows foreign key and reverse relationships and includes the full nested objects.
> <br><br>
> 
> Whereas calling the serializer class (let say model **'A'**) inside another serializer class (let say model **'B'**) gives you the control of customization and allows you to chose as to <ins>which fields of the model **'B'** you want to serialize</ins>. In short, it gives **full control** over how related objects are serialized.
> <br><br>
>
> <h4>🔸 &nbsp;<code>depth</code> &nbsp;&nbsp;vs&nbsp;&nbsp; <code>nested serializer</code></h4>
>
> | Feature                       | `depth`                  | Nested `serializer`                        |
> | ----------------------------- | ------------------------ | ------------------------------------------ |
> | Ease of use                   | Very easy                | Requires more code                         |
> | Customization                 | ❌ Not possible         | ✅ Full control (fields, validation, etc.) |
> | Write support (`POST`, `PUT`) | ❌ Read-only            | ✅ Can handle nested writes if coded       |
> | Performance control           | ❌ Less control         | ✅ Can optimize queries manually           |
> | Fine-grained validation       | ❌ No                   | ✅ Yes                                     |
> | Recommended for production?   | ❌ Small use cases only | ✅ Always preferred for complex relations  |
> <br>
>
> <h4>🔸 &nbsp;When to use what ?</h4>
>
> | Use Case                                                      | Use                                         |
> | ------------------------------------------------------------- | ------------------------------------------- |
> | Quick read-only API of related fields                         | `depth=1`                                   |
> | You want to <code>POST</code>/<code>PUT</code> nested objects | Nested `serializer`                         |
> | You want custom logic/validation                              | Nested `serializer`                         |
> | You care about performance                                    | Nested `serializer` (with `select_related`) |

<br>

<h1>SerializerMethodField( )</h1>

<br>

> If you want to create a new field for a <code>serializer</code> without actually adding a field into the corresponding model :
>
> 
> To create a field, you need to call it from <code>SerializerMethodField()</code> and then write a method <code>get_\<field_name></code>.
> <br><br>
> 
> > <code>person_api/home/serializers.py</code>
> > ```
> > class PersonSerializer(serializers.ModelSerializer):
> > 
> >     color = serializers.PrimaryKeyRelatedField(
> >         queryset=Color.objects.all(),
> >         required=False,
> >         allow_null=True
> >     )
> >     country = serializers.SerializerMethodField()                                  # line added
> > 
> >     class Meta:
> >         model = Person
> >         # fields = '__all__'
> >         fields = ['id', 'name', 'age', 'color', 'country']                         # field added
> > 
> >     def get_country(self, obj):                                                    # method added
> >         return "India"
> > 
> >     def validate(self, data):
> >         ●●●
> > ```
> <br>
>
> > <code>GET</code> &nbsp;&nbsp;http://localhost:8000/api/person/
> > ```
> > [
> >     {
> >         "id": 1,
> >         "name": "A1",
> >         "age": 25,
> >         "color": 1,
> >         "country": "India"
> >     },
> >     {
> >         "id": 3,
> >         "name": "A3",
> >         "age": 40,
> >         "color": 2,
> >         "country": "India"
> >     },
> >     ●●●
> > ]
> > ```
> <br>
>
> > <code>GET</code> &nbsp;&nbsp;http://localhost:8000/api/person/2/
> > ```
> > {
> >     "id": 2,
> >     "name": "A2",
> >     "age": 27,
> >     "color": null,
> >     "country": "India"
> > }
> > ```

<br>

> Let's see another example where we add <code>color_info</code> field in the <code>PersonSerializer</code> class :
> 
> <br>
> 
> >  <code>person_api/home/serializers.py</code>
> > ```
> > from rest_framework import serializers
> > from .models import Person, Color
> > from matplotlib import colors as mcolors                                 # Module imported
> > 
> > 
> > def color_name_to_hex(color_name):                                       # Method added
> >     try:
> >         return mcolors.to_hex(color_name)
> >     except ValueError:
> >         return None
> >
> >
> > class PersonSerializer(serializers.ModelSerializer):
> > 
> >     color = serializers.PrimaryKeyRelatedField(
> >         queryset=Color.objects.all(),
> >         required=False,
> >         allow_null=True
> >     )
> >     color_info = serializers.SerializerMethodField()
> > 
> >     class Meta:
> >         model = Person
> >         fields = ['id', 'name', 'age', 'color', 'color_info']             # Custom order here
> > 
> >     def get_color_info(self, obj):
> >         color_obj = Color.objects.get(id = obj.color.id)
> >         hex_code = color_name_to_hex(color_obj.color_name)                # Calls the color_name_to_hex() method
> >         return {
> >             'color_name' : color_obj.color_name, 
> >             'hex_code' : hex_code or '#000'                               # Fallback if invalid
> >         }
> >     
> >     def validate(self, data):
> >         ●●●
> > ```
> <br>
>
> <code>GET</code> &nbsp;&nbsp;http://localhost:8000/api/person/
> ```
> [
>     {
>         "id": 1,
>         "name": "A1",
>         "age": 25,
>         "color": 1,
>         "color_info": {
>             "color_name": "RED",
>             "hex_code": "#ff0000"
>         }
>     },
>     {
>         "id": 3,
>         "name": "A3",
>         "age": 40,
>         "color": 2,
>         "color_info": {
>             "color_name": "BLUE",
>             "hex_code": "#0000ff"
>         }
>     },
>     ●●●
> ]
> ```

<br>

> > ❌ &nbsp;However, now if try to access detail of a specific person with no color, it will throw <code>AttributeError</code> : <br>
> > 
> > http://localhost:8000/api/person/2/
> > 
> > <code>person_api\home\serializers.py</code>
> > ```
> > color_obj = Color.objects.get(id = obj.color.id)
> >                                    ^^^^^^^^^^^^
> > AttributeError: 'NoneType' object has no attribute 'id'
> > ```
> 
> <br>
>
> > ✔️ &nbsp;Solution &nbsp;➜&nbsp; Handle the case where <code>color</code> is <code>None</code>
> > 
> > Update your <code>get_color_info()</code> method like this :
> >
> > <code>person_api/home/serializers.py</code>
> > ```
> > ●●●
> > 
> > class PersonSerializer(serializers.ModelSerializer):
> > 
> >     ●●●
> > 
> >     def get_color_info(self, obj):                                   # Method updated                            
> >         color_obj = obj.color
> > 
> >         if not color_obj:
> >             return None
> > 
> >         hex_code = color_name_to_hex(color_obj.color_name)
> >         return {
> >             'color_name': color_obj.color_name,
> >             'hex_code': hex_code or None
> >         }
> > 
> >     ●●●
> > ```
> <br>
> 
> <code>GET</code> &nbsp;&nbsp;http://localhost:8000/api/person/2/
> ```
> {
>     "id": 2,
>     "name": "A2",
>     "age": 27,
>     "color": null,
>     "color_info": null
> }
> ```
> <br>
> 
> <code>GET</code> &nbsp;&nbsp;http://localhost:8000/api/person/3/
> ```
> {
>     "id": 3,
>     "name": "A3",
>     "age": 40,
>     "color": 2,
>     "color_info": {
>         "color_name": "BLUE",
>         "hex_code": "#0000ff"
>     }
> }
> ```

<br>

<div>
    <h1><code>.select_related()</code></h1>
</div>

To avoid hitting the database multiple times when accessing related objects like <code>obj.color</code>, you should use <code>.select_related()</code> in your views or queryset logic. This performs a **SQL join** and fetches the related objects in a single query.

<h3><ins>But when to use it ?</ins></h3>

Use it for **ForeignKey** and **OneToOne** relationships where you know you'll need the related object.

> <code>person_api/home/views.py</code>
> ```
> ●●●
>
> 
> @api_view(['GET', 'POST', 'PUT', 'PATCH', 'DELETE'])
> def person(request):
>     if request.method == 'GET':
>         # objs = Person.objects.all()
>         # objs = Person.objects.filter(color__isnull = False)       
>         objs = Person.objects.select_related('color').filter(color__isnull = False)            # line added
>         serializer = PersonSerializer(objs, many = True)
>         return Response(serializer.data)
>
>     ●●●
> 
> @api_view(['GET', 'PUT', 'PATCH', 'DELETE'])
> def person_detail(request, id):
>     try:
>         # obj = Person.objects.get(id=id)
>         obj = Person.objects.select_related('color').get(id=id)                                # line added
>     except Person.DoesNotExist:
>         return Response({'error': 'Person not found'}, status=404)
> 
>     ●●●
> ```
> 
> Since <code>color</code> is a <ins>**ForeignKey**</ins> , <code>.select_related('color')</code> will pull both <code>Person</code> and <code>Color</code> data in <ins>**one query**</ins>.
> 
> <br>
> 
> > <code>GET</code> &nbsp;&nbsp;http://localhost:8000/api/person/
> > ```
> > [
> >     {
> >         "id": 1,
> >         "name": "A1",
> >         "age": 25,
> >         "color": 1,
> >         "color_info": {
> >             "color_name": "RED",
> >             "hex_code": "#ff0000"
> >         }
> >     },
> >     {
> >         "id": 3,
> >         "name": "A3",
> >         "age": 40,
> >         "color": 2,
> >         "color_info": {
> >             "color_name": "BLUE",
> >             "hex_code": "#0000ff"
> >         }
> >     },
> >     ●●●
> > ]
> > ```
> <br>
> 
> > <code>GET</code> &nbsp;&nbsp;http://localhost:8000/api/person/2/
> > ```
> > {
> >     "id": 2,
> >     "name": "A2",
> >     "age": 27,
> >     "color": null,
> >     "color_info": null
> > }
> > ```

<br>

> When you want to write all the functionalities and logic manually and don't want to use any internal functionality of <code>ModelSerializer</code> or <code>HyperModelSerializer</code>, you simply use <code>Serializer</code> class.
>
> For example, you use this <code>Serializer</code> class for **validation** <ins>where you need to write your own custom logic</ins>. &nbsp;⭐
>
> So when you create a **login page**, you'd want to validate the authentication details through serializer without even touching the database.
>
> However, you can do it through <code>ModelSerializer</code> too but generally we don't do it like that, especially when we want to just validate the data.
> 
> <br>
>
> > <code>person_api/home/serializers.py</code>
> > ```
> > ●●●
> >
> > class LoginSerializer(serializers.Serializer):
> >     email = serializers.EmailField()
> >     password = serializers.CharField()
> > ```
> <br>
>
> > <code>person_api/home/views.py</code>
> > ```
> > from home.serializers import LoginSerializer
> >
> > ●●●
> >
> > # /api/login/
> > @api_view(['POST'])
> > def login(request):
> >     data = request.data
> >     serializer = LoginSerializer(data = data)
> > 
> >     if serializer.is_valid():
> >         data = serializer._validated_data
> >         print(data)
> >         return Response({'message' : 'success'})
> > 
> >     return Response(serializer.errors)
> > ```
> <br>
> 
> > <code>person_api/api/urls.py</code>
> > ```
> > from home.views import index, person, person_detail, login                # login added
> > from django.urls import path
> > 
> > urlpatterns = [
> >     path('index/', index),
> >     path('person/', person),
> >     path('person/<int:id>/', person_detail),
> >     path('login/', login),                                                # login path added
> > ]
> > ```
> <br>
>
> > <code>POST</code> &nbsp;&nbsp;http://localhost:8000/api/login/
> > 
> > **<ins>Output</ins> :**
> > ```
> > {
> >     "email": [
> >         "This field is required."
> >     ],
> >     "password": [
> >         "This field is required."
> >     ]
> > }
> > ```
> <br>
>
> > <code>POST</code> &nbsp;&nbsp;http://localhost:8000/api/login/
> > ```
> > {
> >     "email" : "avinash",
> >     "password" : "123"
> > }
> > ```
> >
> > **<ins>Output</ins> &nbsp;:**
> > ```
> > {
> >     "email": [
> >        "Enter a valid email address."
> >     ]
> > }
> > ```
> <br>
> 
> > <code>POST</code> &nbsp;&nbsp;http://localhost:8000/api/login/
> > ```
> > {
> >     "email" : "avinash@gmail.com",
> >     "password" : "123"
> > }
> > ```
> >
> > **<ins>Output</ins> &nbsp;:**
> > ```
> > {
> >     "message": "success"
> > }
> > ```

<br>

<div>
	<h1><code>APIView</code> &nbsp;⟶&nbsp; Class-Based View</h1>
</div>

∎ &nbsp;A class-based approach using DRF’s <code>APIView</code> or its subclasses.

∎ &nbsp;Gives you **full OOP-style control**, method dispatching (<code>get()</code>, <code>post()</code>, etc.), and <ins>easy reuse</ins>.

∎ &nbsp;It encapsulates all the **CRUD methods** and **automatically** calls it according to the type of <code>Request</code> <br>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;For example &nbsp;➜&nbsp; if it is a <code>GET</code> request, it will automatically call the <code>get()</code> method and so on.

<br>

> > <code>person_api/home/views.py</code>
> > ```
> > from rest_framework.views import APIView
> >
> > ●●●
> > 
> > class Persons(APIView):
> > 
> >     def get(self, request):
> >         return Response({'message' : 'This is a get request'})
> >     
> >     def post(self, request):
> >         return Response({'message' : 'This is a post request'})
> >     
> >     def put(self, request):
> >         return Response({'message' : 'This is a put request'})
> >     
> >     def patch(self, request):
> >         return Response({'message' : 'This is a patch request'})
> >     
> >     def delete(self, request):
> >         return Response({'message' : 'This is a delete request'})
> > ```
> <br>
> 
> > <code>person_api/api/urls.py</code>
> > ```
> > from home.views import index, person, person_detail, login, Persons
> > from django.urls import path
> > 
> > 
> > urlpatterns = [
> >     path('index/', index),                              # Function based view
> >     path('person/', person),                            # Function based view
> >     path('person/<int:id>/', person_detail),            # Function based view
> >     path('login/', login),                              # Function based view
> >     path('persons/', Persons.as_view())                 # Class based view                  # line added
> > ]
> > ```
> <br>
> 
> > Now if you hit these request methods with no body &nbsp;**:** <br>
> > <code>GET</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;http://localhost:8000/api/persons/ &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;➜ &nbsp;&nbsp;&nbsp;&nbsp; <code>{"message": "This is a GET request"}</code> <br>
> > <code>POST</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;http://localhost:8000/api/persons/ &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;➜ &nbsp;&nbsp;&nbsp;&nbsp; <code>{"message": "This is a POST request"}</code> <br>
> > <code>PUT</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;http://localhost:8000/api/persons/ &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;➜ &nbsp;&nbsp;&nbsp;&nbsp; <code>{"message": "This is a PUT request"}</code> <br>
> > <code>PATCH</code> &nbsp;&nbsp;&nbsp;&nbsp;http://localhost:8000/api/persons/ &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;➜ &nbsp;&nbsp;&nbsp;&nbsp; <code>{"message": "This is a PATCH request"}</code> <br>
> > <code>DELETE</code> &nbsp;&nbsp;http://localhost:8000/api/persons/ &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;➜ &nbsp;&nbsp;&nbsp;&nbsp; <code>{"message": "This is a DELETE request"}</code>

<br>

> > <code>person_api/home/views.py</code>
> > ```
> > # /api/persons/
> > class Persons(APIView):
> > 
> >     def get(self, request): 
> >         objs = Person.objects.select_related('color').filter(color__isnull = False)
> >         serializer = PersonSerializer(objs, many = True)
> >         return Response(serializer.data)
> >     
> >     def post(self, request):
> >         data = request.data
> >         serializer = PersonSerializer(data = data)
> >         if serializer.is_valid():
> >             serializer.save()
> >             return Response(serializer.data)
> >         return Response(serializer.errors)
> >     
> >     def put(self, request):
> >         data = request.data
> >         obj = Person.objects.get(id = data['id'])
> >         serializer = PersonSerializer(obj, data = data)
> >         if serializer.is_valid():
> >             serializer.save()
> >             return Response(serializer.data)
> >         return Response(serializer.errors)
> >     
> >     def patch(self, request):
> >         data = request.data
> >         obj = Person.objects.get(id = data['id'])
> >         serializer = PersonSerializer(obj, data = data, partial = True)
> >         if serializer.is_valid():
> >             serializer.save()
> >             return Response(serializer.data)
> >         return Response(serializer.errors)
> >     
> >     def delete(self, request):
> >         data = request.data
> >         obj = Person.objects.get(id = data['id'])
> >         obj.delete()
> >         return Response({'message' : 'person deleted'})
> > ```
> 
> <br>
> 
> > <code>POST</code> &nbsp;&nbsp;http://localhost:8000/api/persons/
> > ```
> > {
> >     "name": "AXX",
> >     "age" : 35,
> >     "color" : 3
> > }
> > ```
> > 
> > 
> > **Output &nbsp;:**
> > ```
> > {
> >     "id": 23,
> >     "name": "AXX",
> >     "age": 35,
> >     "color": 3,
> >     "color_info": {
> >         "color_name": "GREEN",
> >         "hex_code": "#008000"
> >     }
> > }
> > ```
> 
> <br>
> 
> > <code>PATCH</code> &nbsp;&nbsp;http://localhost:8000/api/persons/
> > ```
> > {
> >     "id": 23,
> >     "name": "A23"
> > }
> > ```
> > 
> > 
> > **Output &nbsp;:**
> > ```
> > {
> >     "id": 23,
> >     "name": "A23",
> >     "age": 35,
> >     "color": 3,
> >     "color_info": {
> >         "color_name": "GREEN",
> >         "hex_code": "#008000"
> >     }
> > }
> > ```
> 
> <br>
> 
> > <code>PATCH</code> &nbsp;&nbsp;http://localhost:8000/api/person/23/
> > ```
> > {
> >     "age": 36
> > }
> > ```
> > 
> > 
> > **Output &nbsp;:**
> > ```
> > {
> >     "id": 23,
> >     "name": "A23",
> >     "age": 36,
> >     "color": 3,
> >     "color_info": {
> >         "color_name": "GREEN",
> >         "hex_code": "#008000"
> >     }
> > }
> > ```

<br>

<h1>ModelViewSet</h1>

Now that you have seen both function based view <code>@api_view()</code> and class based view <code>ApiView()</code> which are custom CRUD suuport, let's now see <ins>ModelViewSet</ins> which gives you **full-featured RESTful APIs** + **auto-routing**, **filters**, **auth**, etc.

<br>

> > <code>person_api/home/views.py</code>
> > ```
> > from rest_framework import viewsets
> >
> >
> > class PeopleViewSet(viewsets.ModelViewSet):
> >     serializer_class = PersonSerializer
> >     queryset = Person.objects.all()
> > ```
>
> <br>
>
> > <code>person_api/api/urls.py</code>
> > ```
> > from home.views import index, person, person_detail, login, Persons, PeopleViewSet          # PeopleViewSet
> > from django.urls import path, include                                                       # include
> > from rest_framework.routers import DefaultRouter                                            # DefaultRouter
> > 
> > 
> > router = DefaultRouter()
> > router.register(r'people', PeopleViewSet, basename='people')
> > urlpatterns = router.urls
> > 
> > 
> > urlpatterns = [
> >     path('index/', index),                              # Function based view
> >     path('person/', person),                            # Function based view
> >     path('person/<int:id>/', person_detail),            # Function based view
> >     path('login/', login),                              # Function based view
> >     path('persons/', Persons.as_view()),                # Class based view
> >     path('', include(router.urls))                      # ModelViewSet
> > ]
> > ```
> 
> <br>
> 
> > <code>GET</code> &nbsp;&nbsp;http://localhost:8000/api/
> > <br>
> > 
> > **Output &nbsp;:**
> > ```
> > {
> >     "people": "http://localhost:8000/api/people/"
> > }
> > ```
>
> <br>
> 
> So http://localhost:8000/api/people/ is the URL you can use **for all the CRUD operations** <ins>without you having to bother about how actually the CRUD logic is implemented in the backend</ins>. This is **Abstraction** in Object Oriented Programming where you need to focus only on Application and not on the working details of a set of REST APIs for a model.

<br>

> <code>GET</code> &nbsp;&nbsp;http://localhost:8000/api/people/
> <br>
> 
> **Output &nbsp;:**
> ```
> [
>     {
>         "id": 1,
>         "name": "A1",
>         "age": 25,
>         "color": 1,
>         "color_info": {
>             "color_name": "RED",
>             "hex_code": "#ff0000"
>         }
>     },
>     {
>         "id": 2,
>         "name": "A2",
>         "age": 27,
>         "color": null,
>         "color_info": null
>     },
>     {
>         "id": 3,
>         "name": "A3",
>         "age": 40,
>         "color": 2,
>         "color_info": {
>             "color_name": "BLUE",
>             "hex_code": "#0000ff"
>         }
>     }
> ]
> ```

<br>

> <code>POST</code> &nbsp;&nbsp;http://localhost:8000/api/people/
> ```
> {
>     "name": "AAA",
>     "age" : 29,
>     "color": 3
> }
> ```
> <br>
>
> **Output &nbsp;:**
> ```
> {
>     "id": 24,
>     "name": "AAA",
>     "age": 29,
>     "color": 3,
>     "color_info": {
>         "color_name": "GREEN",
>         "hex_code": "#008000"
>     }
> }
> ```

<br>

> <code>GET</code> &nbsp;&nbsp;http://localhost:8000/api/people/24/
> <br>
> 
> **Output &nbsp;:**
> ```
> {
>     "id": 24,
>     "name": "AAA",
>     "age": 29,
>     "color": 3,
>     "color_info": {
>         "color_name": "GREEN",
>         "hex_code": "#008000"
>     }
> }
> ```

<br>

> > > <code>PATCH</code> &nbsp;&nbsp;http://localhost:8000/api/people/
> > > ```
> > > {
> > >     "id": 24,
> > >     "name": "A24",
> > >     "color": 1
> > > }
> > > ```
> > > **Output &nbsp;:** &nbsp; <code>Method Not Allowed: /api/people/</code>
> >
> > <br>
> > 
> > > The correct way of doing a <code>PATCH</code> is &nbsp;&nbsp;➜&nbsp;&nbsp; <code>PATCH</code> &nbsp;&nbsp;http://localhost:8000/api/people/24/
> > > ```
> > > {
> > >     "id" : 24,                # optional
> > >     "name": "A24",
> > >     "color": 1
> > > }
> > > ```
> > > **Output &nbsp;:**
> > > ```
> > > {
> > >     "id": 24,
> > >     "name": "A24",
> > >     "age": 29,
> > >     "color": 1,
> > >     "color_info": {
> > >         "color_name": "RED",
> > >         "hex_code": "#ff0000"
> > >     }
> > > }
> > > ```
> 
> <br>
> 
> Similarly the correct way of doing a <code>PUT</code> is by using http://localhost:8000/api/people/24/ and not http://localhost:8000/api/people/

<br>

> Now that you have seen all kinds of ways to create an API, let's compare them. 
> 
> 
> | Feature/Aspect  | Function-Based API          | Class-Based API (`APIView`)  | ModelViewSet                          |
> | --------------- | --------------------------- | ---------------------------- | ------------------------------------- |
> | Style           | Procedural                  | OOP                          | OOP + Auto-routing                    |
> | Code Structure  | Simple                      | Clean, method-separated      | Highly abstracted                     |
> | Suitable for    | Small/simple APIs           | Medium complexity            | Full-featured RESTful APIs            |
> | URL Handling    | Manual                      | Manual                       | Auto via Routers                      |
> | CRUD Support    | Custom                      | Custom                       | Full (Create, Retrieve, etc.)         |
> | Extensibility   | Low                         | Medium                       | High (Mixins, Permissions, Filters)   |
> | Performance     | Excellent                   | Excellent                    | Slightly more abstracted              |
> | Testability     | Good                        | Very good                    | Excellent                             |
> | Common Use Case | Prototypes, quick endpoints | Reusable views, custom logic | Full REST APIs with standard patterns |

<br>

<h1>Mixins</h1>

<br>

<h3>✅ &nbsp;What is a Mixin ?</h3>

A **mixin** is a type of **multiple inheritance** pattern where a class is used to add **extra functionality** to other classes, without being a full standalone class on its own.

> In simple words:
> 
> 👉 &nbsp;**Mixin &nbsp;=&nbsp; reusable feature class**

You use <code>mixins</code> to **"mix in"** behavior into other classes.

A mixin : <br>
&nbsp;&nbsp;●&nbsp;&nbsp; Usually doesn’t stand alone. <br>
&nbsp;&nbsp;●&nbsp;&nbsp; Doesn't define the complete object. <br>
&nbsp;&nbsp;●&nbsp;&nbsp; Only provides additional methods or behavior.

<br>

<h3>✅ &nbsp;Why do we need Mixins ?</h3>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;●&nbsp;&nbsp; Code reuse <br> 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;●&nbsp;&nbsp; Separation of concerns <br> 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;●&nbsp;&nbsp; Avoid repetition (DRY principle) <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;●&nbsp;&nbsp; Easier testing & maintenance.

<br>

<h3>✅ &nbsp;Simple Example in Pure Python</h3>

> Suppose we want some logging feature for many classes :
> ```
> class LoggerMixin:
>     def log(self, message):
>         print(f"[LOG]: {message}")
> 
> class Animal:
>     def speak(self):
>         print("Animal speaks")
> 
> class Dog(Animal, LoggerMixin):
>     def speak(self):
>         self.log("Dog is barking")
>         print("Bark!")
> 
> dog = Dog()
> dog.speak()
> ```
>
> Output :
> ```
> [LOG]: Dog is barking
> Bark!
> ```
>
> ✔ <code>LoggerMixin</code> gives logging capability to <code>Dog</code> without forcing <code>Animal</code> or <code>Dog</code> to have that logic directly.

<br>

<h3>✅ &nbsp;Mixin Example in Django</h3>

Django uses **mixins** heavily for views. It can combine behaviors by mixing them together.

Here is how you can build your DRF view using Mixins :

> 👉 &nbsp;<code>views.py</code>
> ```
> from rest_framework import generics, mixins
> from .models import Person
> from .serializers import PersonSerializer
> 
> class PersonView(
>     mixins.ListModelMixin,                      # for GET list
>     mixins.CreateModelMixin,                    # for POST create
>     mixins.RetrieveModelMixin,                  # for GET detail
>     mixins.UpdateModelMixin,                    # for PUT/PATCH update
>     mixins.DestroyModelMixin,                   # for DELETE
>     generics.GenericAPIView                     # gives base functionality / base APIView structure
> ):
>     queryset = Person.objects.all()
>     serializer_class = PersonSerializer
> 
>     def get(self, request, *args, **kwargs):
>         if 'pk' in kwargs:
>             return self.retrieve(request, *args, **kwargs)
>         return self.list(request, *args, **kwargs)
> 
>     def post(self, request, *args, **kwargs):
>         return self.create(request, *args, **kwargs)
> 
>     def put(self, request, *args, **kwargs):
>         return self.update(request, *args, **kwargs)
> 
>     def patch(self, request, *args, **kwargs):
>         return self.partial_update(request, *args, **kwargs)
> 
>     def delete(self, request, *args, **kwargs):
>         return self.destroy(request, *args, **kwargs)
> ```

<br>

> 👉 &nbsp;<code>urls.py</code>
> ```
> from django.urls import path
> from .views import PersonView
> 
> urlpatterns = [
>     path('person/', PersonView.as_view()),                    # for list & create
>     path('person/<int:pk>/', PersonView.as_view()),           # for retrieve, update, delete
> ]
> ```

<br>

| HTTP      | URL          | Action   |
| --------- | ------------ | -------- |
| GET       | `/person/`   | list     |
| POST      | `/person/`   | create   |
| GET       | `/person/1/` | retrieve |
| PUT/PATCH | `/person/1/` | update   |
| DELETE    | `/person/1/` | delete   |

<br>

<h3>✅ &nbsp;Quick Summary Table</h3>

| **Concept** | **Explanation**                            |
| ----------- | ------------------------------------------ |
| Class       | Full standalone blueprint                  |
| Mixin       | Partial behavior to add                    |
| Use case    | Add reusable small features                |
| Django use  | Views, permissions, serializers, querysets |

<br>

<h3>✅ &nbsp;DRF Generic Flow Structure</h3>

<pre>
APIView (base - just HTTP methods from basic low-level view)
|
|--- GenericAPIView (adds queryset, serializer, filtering, pagination etc.)
     |
     |--- mixins.CreateModelMixin     ➜   adds create()
     |--- mixins.ListModelMixin       ➜   adds list()
     |--- mixins.UpdateModelMixin     ➜   adds update()
     |--- mixins.DestroyModelMixin    ➜   adds destroy()
     |--- mixins.RetrieveModelMixin   ➜   adds retrieve()
</pre>

Instead of forcing you to rewrite similar code in every view, DRF gives you **Mixins** (reusable building blocks) to add specific behavior : <ins>list</ins> , <ins>create</ins> , <ins>update</ins> , <ins>delete</ins> , <ins>retrieve</ins>. Then you compose them together as per your need.

<br>

<h1>Bulk Create &nbsp;/&nbsp; Bulk Update &nbsp;/&nbsp; Bulk Delete</h1>

<br>

> If you try to hit any of these urls for bulk create, you will get error.
> <br>
> 
> <code>PUT</code> &nbsp;&nbsp;http://localhost:8000/api/person/    &nbsp;&nbsp;&nbsp;&nbsp; - **option 1** &nbsp;(function based view) <br>
> <code>PUT</code> &nbsp;&nbsp;http://localhost:8000/api/persons/   &nbsp;&nbsp;&nbsp;- **option 2** &nbsp;(class based view)    <br>
> <code>PUT</code> &nbsp;&nbsp;http://localhost:8000/api/people/    &nbsp;&nbsp;&nbsp;&nbsp; - **option 3** &nbsp;(ModelViewSet)        <br>
> ```
> [
>     {"name": "S1", "age": 16, "color": 3},
>     {"name": "S2", "age": 35, "color": 3},
>     {"name": "S3", "age": 27},
>     {"name": "S4", "age":28, "color": null}
> ]
> ```

<br>

🧠 The bulk operations (create / update / delete) **reduce database hits from N queries ➜ 1 query**.

<br>

> > <code>person_api/home/views.py</code> &nbsp;⭐
> > ```
> > from rest_framework.views import APIView
> > from rest_framework.response import Response
> > from rest_framework import status
> > from .models import Person
> > from .serializers import PersonSerializer
> > 
> > 
> > # /api/person/bulk-create/
> > class BulkPersonCreateView(APIView):
> >     def post(self, request):
> >         serializer = PersonSerializer(data=request.data, many=True)
> >         if serializer.is_valid():
> >             serializer.save()
> >             return Response(
> >                 {"message": "Bulk create successful", "data": serializer.data}, 
> >                 status=status.HTTP_201_CREATED
> >             )
> >         return Response(
> >             serializer.errors, 
> >             status=status.HTTP_400_BAD_REQUEST
> >         )
> > 
> > 
> > # /api/person/bulk-update/
> > class BulkPersonUpdateView(APIView):
> >     def put(self, request):
> >         data = request.data
> >         response_data = []
> >         errors = []
> > 
> >         for item in data:
> >             try:
> >                 person = Person.objects.get(id=item['id'])
> >                 serializer = PersonSerializer(person, data=item, partial=True)
> >                 if serializer.is_valid():
> >                     serializer.save()
> >                     response_data.append(serializer.data)
> >                 else:
> >                     errors.append(serializer.errors)
> >             except Person.DoesNotExist:
> >                 errors.append({"id": item.get("id"), "error": "Not Found"})
> > 
> >         if errors:
> >             return Response(
> >                 {"errors": errors, "updated": response_data}, 
> >                 status=status.HTTP_207_MULTI_STATUS
> >             )
> > 
> >         return Response({"message": "All records updated successfully", "data": response_data})
> > 
> >
> > # /api/person/bulk-delete/
> > class BulkPersonDeleteView(APIView):
> >     def delete(self, request):
> >         ids = request.data.get("ids", [])
> > 
> >         if not isinstance(ids, list) or not all(isinstance(i, int) for i in ids):
> >             return Response(
> >                 {"error": "Invalid data. 'ids' must be a list of integers."},
> >                 status=status.HTTP_400_BAD_REQUEST
> >             )
> > 
> >         persons = Person.objects.filter(id__in=ids)
> > 
> >         if not persons.exists():
> >             return Response(
> >                 {"error": "No matching persons found for the provided IDs."},
> >                 status=status.HTTP_404_NOT_FOUND
> >             )
> > 
> >         found_ids = list(persons.values_list('id', flat=True))
> > 
> >         deleted_count = persons.count()
> >         persons.delete()
> > 
> >         return Response(
> >             {
> >                 "message": f"{deleted_count} persons deleted successfully.",
> >                 "deleted_ids": found_ids,
> >                 "missing_ids": list(set(ids) - set(found_ids))
> >             },
> >            status=status.HTTP_200_OK
> >         )
> > ```
>
> <br>
>
> > <code>person_api/api/urls.py</code>
> > ```
> > from django.urls import path
> > from .views import BulkPersonCreateView, BulkPersonUpdateView, BulkPersonDeleteView
> > 
> > urlpatterns = [
> >     path('person/bulk-create/', BulkPersonCreateView.as_view()),            # Class based view
> >     path('person/bulk-update/', BulkPersonUpdateView.as_view()),            # Class based view
> >     path('person/bulk-delete/', BulkPersonDeleteView.as_view()),            # Class based view
> > ]
> > ```

<br>

> 🔸 &nbsp;The <ins>**bulk-create**</ins> code is implemented in such a way that **either all records are created at once or none**. &nbsp;⭐
> 
> <br>
>
> > <code>POST</code> &nbsp;&nbsp;http://localhost:8000/api/person/bulk-create/
> > ```
> > [
> >  	{"name": "S1", "age": 16, "color": 3},
> >  	{"name": "S2", "age": 35, "color": 3},
> >  	{"name": "S3", "age": 27},
> > 	{"name": "S4", "age":28, "color": null}
> > ]
> > ```
> > <br>
> >
> > **Output &nbsp;:**
> > ```
> > [
> >     {"non_field_errors": ["Age should be 18 or older."]},
> >     {},
> >     {},
> >     {}
> > ]
> > ```
> 
> <br>
> 
> It means that the first record here is corrupt because of age validation. <br>
> Changing it will make the <ins>**bulk-create**</ins> successful. <br>
> Let's verify it.
> 
> <br>
> 
> > <code>POST</code> &nbsp;&nbsp;http://localhost:8000/api/person/bulk-create/
> > ```
> > [
> >  	{"name": "S1", "age": 26, "color": 3},                   
> >  	{"name": "S2", "age": 35, "color": 3},
> >  	{"name": "S3", "age": 27},
> > 	{"name": "S4", "age":28, "color": null}
> > ]
> > ```
> > <br>
> >
> > **Output &nbsp;:**
> > ```
> > {
> >     "message": "Bulk create successful",
> >     "data": [
> >         {
> >             "id": 33,
> >             "name": "S1",
> >             "age": 26,
> >             "color": 3,
> >             "color_info": {
> >                 "color_name": "GREEN",
> >                 "hex_code": "#008000"
> >             }
> >         },
> >         {
> >             "id": 34,
> >             "name": "S2",
> >             "age": 35,
> >             "color": 3,
> >             "color_info": {
> >                 "color_name": "GREEN",
> >                 "hex_code": "#008000"
> >             }
> >         },
> >         {
> >             "id": 35,
> >             "name": "S3",
> >             "age": 27,
> >             "color": null,
> >             "color_info": null
> >         },
> >         {
> >             "id": 36,
> >             "name": "S4",
> >             "age": 28,
> >             "color": null,
> >             "color_info": null
> >         }
> >     ]
> > }
> > ```

<br>

> 🔸 &nbsp;The <ins>**bulk-update**</ins> code is implemented in such a way that **it's not EITHER ALL OR NONE**. &nbsp;⭐ <br>
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;It means only those records will be updated which are not corrupt.
> 
> <br>
>
> > <code>PUT</code> &nbsp;&nbsp;http://localhost:8000/api/person/bulk-update/
> > ```
> > [
> >   {"id": 33, "name": "Test33", "age": 12},
> >   {"id": 35, "name": "Test35"},
> >   {"id": 36, "color": 2}
> > ]
> > ```
> > <br>
> >
> > **Output &nbsp;:**
> > ```
> > {
> >     "errors": [
> >         {
> >             "non_field_errors": [
> >                 "Age should be 18 or older."
> >             ]
> >         }
> >     ],
> >     "updated": [
> >         {
> >             "id": 35,
> >             "name": "Test35",
> >             "age": 27,
> >             "color": null,
> >             "color_info": null
> >         },
> >         {
> >             "id": 36,
> >             "name": "S4",
> >             "age": 28,
> >             "color": 2,
> >             "color_info": {
> >                 "color_name": "BLUE",
> >                 "hex_code": "#0000ff"
> >             }
> >         }
> >     ]
> > }
> > ```
>
> <br> 
> 
> Let's see another example of <ins>**bulk-update**</ins>
>
> <br>
>
> > <code>PUT</code> &nbsp;&nbsp;http://localhost:8000/api/person/bulk-update/
> > ```
> > [
> > 	{"id": 1, "name": "Avinash Mishra", "age": 32, "color": 1},
> > 	{"id": 2, "name": "Aman Mishra", "age": 27, "color": null},
> > 	{"id": 3, "name": "Radha Jha", "age": 29, "color": 3},
> > 	{"id": 21, "name": "Gopal Krisna Jha", "age": 35, "color": 3},
> > 	{"id": 22, "name": "Bechan Mishra", "age": 54, "color": 1},
> > 	{"id": 25, "name": "Bina Mishra", "age": 50}
> > ]
> > ```
> > <br>
> >
> > **Output &nbsp;:**
> > ```
> > {
> >     "message": "All records updated successfully",
> >     "data": [
> >         {
> >             "id": 1,
> >             "name": "Avinash Mishra",
> >             "age": 32,
> >             "color": 1,
> >             "color_info": {
> >                 "color_name": "RED",
> >                 "hex_code": "#ff0000"
> >             }
> >         },
> >         {
> >             "id": 2,
> >             "name": "Aman Mishra",
> >             "age": 27,
> >             "color": null,
> >             "color_info": null
> >         },
> >         {
> >             "id": 3,
> >             "name": "Radha Jha",
> >             "age": 29,
> >             "color": 3,
> >             "color_info": {
> >                 "color_name": "GREEN",
> >                 "hex_code": "#008000"
> >             }
> >         },
> >         {
> >             "id": 21,
> >             "name": "Gopal Krisna Jha",
> >             "age": 35,
> >             "color": 3,
> >             "color_info": {
> >                 "color_name": "GREEN",
> >                 "hex_code": "#008000"
> >             }
> >         },
> >         {
> >             "id": 22,
> >             "name": "Bechan Mishra",
> >             "age": 54,
> >             "color": 1,
> >             "color_info": {
> >                 "color_name": "RED",
> >                 "hex_code": "#ff0000"
> >             }
> >         },
> >         {
> >             "id": 25,
> >             "name": "Bina Mishra",
> >             "age": 50,
> >             "color": 2,
> >             "color_info": {
> >                 "color_name": "BLUE",
> >                 "hex_code": "#0000ff"
> >             }
> >         }
> >     ]
> > }
> > ```

<br>

> 🔸 &nbsp;The <ins>**bulk-delete**</ins> code is implemented in such a way that **it's not EITHER ALL OR NONE**. &nbsp;⭐ <br>
> 
> <br>
> 
> > **Single Delete**
> >
> > <code>DELETE</code> &nbsp;&nbsp;http://localhost:8000/api/person/35/
> > ```
> > {
> >     "message": "Person deleted"
> > }
> > ```
>
> <br>
>
> > **Bulk Delete**
> > 
> > <code>DELETE</code> &nbsp;&nbsp;http://localhost:8000/api/person/bulk-delete/
> > ```
> > {
> >     "ids": [33, 34, 35, 36]
> > }
> > ```
> > <br>
> >
> > **Output &nbsp;:**
> > ```
> > {
> >     "message": "3 persons deleted successfully.",
> >     "deleted_ids": [
> >         33,
> >         34,
> >         36
> >     ],
> >     "missing_ids": [
> >         35
> >     ]
> > }
> > ```

<br>

<h1>Filters</h1>

<br>

<h3>⬛ &nbsp;.filter(&nbsp;)</h3>

<br>

> ✅ &nbsp;Let's first build a <code>search</code> api around a **single field** of the <code>Person</code> model in <code>PeopleViewSet</code>.
> 
> <br> 
>
> > 🔶 &nbsp;**Filter on Name**
> >
> > <br>
> >
> > <code>person_api/home/views.py</code>
> > ```
> > class PeopleViewSet(viewsets.ModelViewSet):
> >     serializer_class = PersonSerializer
> >     queryset = Person.objects.all()
> > 
> > 
> >     def list(self, request):
> >         search = request.GET.get('search')                            # http://localhost:8000/api/people/?search=
> >         queryset = self.queryset
> >         if search:
> >             queryset = queryset.filter(name__startswith=search)
> >         serializer = self.serializer_class(queryset, many=True)           # PersonSerializer(queryset, many=True)
> >         return Response({'status': 200, 'data': serializer.data})
> > ```
> >
> > <br>
> >
> > <code>GET</code> &nbsp;&nbsp;http://localhost:8000/api/people/?search=b
> > ```
> > {
> >     "status": 200,
> >     "data": [
> >         {
> >             "id": 22,
> >             "name": "Bechan Mishra",
> >             "age": 54,
> >             "color": 1,
> >             "color_info": {
> >                 "color_name": "RED",
> >                 "hex_code": "#ff0000"
> >             }
> >         },
> >         {
> >             "id": 25,
> >             "name": "Bina Mishra",
> >             "age": 50,
> >             "color": 2,
> >             "color_info": {
> >                 "color_name": "BLUE",
> >                 "hex_code": "#0000ff"
> >             }
> >         }
> >     ]
> > }
> > ```
> >
> > <br>
> >
> > <code>GET</code> &nbsp;&nbsp;http://localhost:8000/api/people/?search=bi
> > ```
> > {
> >     "status": 200,
> >     "data": [
> >         {
> >             "id": 25,
> >             "name": "Bina Mishra",
> >             "age": 50,
> >             "color": 2,
> >             "color_info": {
> >                 "color_name": "BLUE",
> >                 "hex_code": "#0000ff"
> >             }
> >         }
> >     ]
> > }
> > ```
> 
> <br>
> 
> > 🔶 &nbsp;**Filter on Age**
> >
> > <br>
> >
> > Just modify the <code>list()</code> method of the <code>PeopleViewSet</code> class in the <code>person_api/home/views.py</code> file
> > ```
> > if search:
> >     queryset = queryset.filter(age__gte=search)
> > ```
> >
> > <br>
> >
> > <code>GET</code> &nbsp;&nbsp;http://localhost:8000/api/people/?search=50
> > ```
> > {
> >     "status": 200,
> >     "data": [
> >         {
> >             "id": 22,
> >             "name": "Bechan Mishra",
> >             "age": 54,
> >             "color": 1,
> >             "color_info": {
> >                 "color_name": "RED",
> >                 "hex_code": "#ff0000"
> >             }
> >         },
> >         {
> >             "id": 25,
> >             "name": "Bina Mishra",
> >             "age": 50,
> >             "color": 2,
> >             "color_info": {
> >                 "color_name": "BLUE",
> >                 "hex_code": "#0000ff"
> >             }
> >         }
> >     ]
> > }
> > ```

<br>

> ✅ &nbsp;Let's now customize the same api by <ins>**combining filters on multiple fields**</ins> of the <code>Person</code> model <br>
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; like <code>name</code> and <code>age</code> or it's related fields like <code>color__color_name</code>.
>
> <br>
>
> > 🔶 &nbsp;Find all the persons who have the letter **"r"** in both his <code>name</code> and <code>color</code>.
> > 
> > <br>
> > 
> > ```
> > if search:
> >     queryset = queryset.filter(
> >         Q(name__icontains=search) &
> >         Q(color__color_name__icontains=search)
> >    )
> > ```
> > Use <code>from django.db.models import Q</code> for combining filters with <code>|</code> or <code>&</code>. &nbsp;⭐
> >
> > <br>
> >
> > **<ins>Solution</ins> &nbsp;:**
> >
> > <code>GET</code> &nbsp;&nbsp;http://localhost:8000/api/people/?search=r
> > ```
> > from django.db.models import Q
> > 
> > 
> > class PeopleViewSet(viewsets.ModelViewSet):
> >     serializer_class = PersonSerializer
> >     queryset = Person.objects.all()
> > 
> >     def list(self, request):
> >         search = request.GET.get('search')
> >         queryset = self.queryset
> >         if search:
> >             queryset = queryset.filter(
> >                 Q(name__icontains=search) &
> >                 Q(color__color_name__icontains=search)
> >             )
> >         serializer = self.serializer_class(queryset, many=True)           # PersonSerializer(queryset, many=True)
> >         return Response({'status': 200, 'data': serializer.data})
> > ```
> > 
> > <br>
> >
> > **Output &nbsp;:**
> > ```
> > {
> >     "status": 200,
> >     "data": [
> >         {
> >             "id": 1,
> >             "name": "Avinash Mishra",
> >             "age": 32,
> >             "color": 1,
> >             "color_info": {
> >                 "color_name": "RED",
> >                 "hex_code": "#ff0000"
> >             }
> >         },
> >         {
> >             "id": 3,
> >             "name": "Radha Jha",
> >             "age": 29,
> >             "color": 3,
> >             "color_info": {
> >                 "color_name": "GREEN",
> >                 "hex_code": "#008000"
> >             }
> >         },
> >         {
> >             "id": 21,
> >             "name": "Gopal Krisna Jha",
> >             "age": 35,
> >             "color": 3,
> >             "color_info": {
> >                 "color_name": "GREEN",
> >                 "hex_code": "#008000"
> >             }
> >         },
> >         {
> >             "id": 22,
> >             "name": "Bechan Mishra",
> >             "age": 54,
> >             "color": 1,
> >             "color_info": {
> >                 "color_name": "RED",
> >                 "hex_code": "#ff0000"
> >             }
> >         }
> >     ]
> > }
> > ```
> 
> <br>
>
> > 🔶 &nbsp;Find all the persons who have the letter **"r"** in both his <code>name</code> & <code>color</code> and also who have <code>age <= 33</code>
> > 
> > <br>
> > 
> > ```
> > if search:
> >     queryset = queryset.filter(
> >         Q(name__icontains=search) &
> >         Q(age__lte=33) &
> >         Q(color__color_name__icontains=search)
> >    )
> > ````
> >
> > <br>
> >
> > **<ins>Solution</ins> &nbsp;:**
> >
> > <code>GET</code> &nbsp;&nbsp;http://localhost:8000/api/people/?search=r
> > ```
> > from django.db.models import Q
> > 
> > 
> > class PeopleViewSet(viewsets.ModelViewSet):
> >     serializer_class = PersonSerializer
> >     queryset = Person.objects.all()
> > 
> >     def list(self, request):
> >         search = request.GET.get('search')
> >         queryset = self.queryset
> >         if search:
> >             queryset = queryset.filter(
> >                 Q(name__icontains=search) &
> >                 Q(age__lte=33) &
> >                 Q(color__color_name__icontains=search)
> >             )
> >         serializer = self.serializer_class(queryset, many=True)           # PersonSerializer(queryset, many=True)
> >         return Response({'status': 200, 'data': serializer.data})
> > ```
> > 
> > <br>
> >
> > **Output &nbsp;:**
> > ```
> > {
> >     "status": 200,
> >     "data": [
> >         {
> >             "id": 1,
> >             "name": "Avinash Mishra",
> >             "age": 32,
> >             "color": 1,
> >             "color_info": {
> >                 "color_name": "RED",
> >                 "hex_code": "#ff0000"
> >             }
> >         },
> >         {
> >             "id": 3,
> >             "name": "Radha Jha",
> >             "age": 29,
> >             "color": 3,
> >             "color_info": {
> >                 "color_name": "GREEN",
> >                 "hex_code": "#008000"
> >             }
> >         }
> >     ]
> > }
> > ```

<br>

> 🔤 &nbsp;**String Field Filters** &nbsp;( <code>CharField</code>, <code>TextField</code> )
> 
> 
> You can use these with <code>name</code> , <code>color__color_name</code> , etc.
> | Lookup        | Description                    | Example                        |
> | ------------- | ------------------------------ | ------------------------------ |
> | `exact`       | Exact match (case-sensitive)   | `name__exact='Rohan'`          |
> | `iexact`      | Exact match (case-insensitive) | `name__iexact='rohan'`         |
> | `contains`    | Substring (case-sensitive)     | `name__contains='oh'`          |
> | `icontains`   | Substring (case-insensitive)   | `name__icontains='oh'`         |
> | `startswith`  | Starts with (case-sensitive)   | `name__startswith='Ro'`        |
> | `istartswith` | Starts with (case-insensitive) | `name__istartswith='ro'`       |
> | `endswith`    | Ends with (case-sensitive)     | `name__endswith='n'`           |
> | `iendswith`   | Ends with (case-insensitive)   | `name__iendswith='N'`          |
> | `in`          | Belongs to list                | `name__in=['Rohan', 'Mohan']`  |
> | `regex`       | Regular expression match       | `name__regex=r'^[A-Z][a-z]+$'` |
> | `iregex`      | Case-insensitive regex match   | `name__iregex=r'^[a-z]+$'`     |


<br>

> 🔢 &nbsp;**Numeric Field Filters** ( <code>IntegerField</code> , <code>FloatField</code> , etc. )
> 
> 
> Use these with fields like <code>age</code>.
> | Lookup  | Description           | Example            |
> | ------- | --------------------- | ------------------ |
> | `exact` | Exact match           | `age__exact=21`    |
> | `lt`    | Less than             | `age__lt=18`       |
> | `lte`   | Less than or equal    | `age__lte=25`      |
> | `gt`    | Greater than          | `age__gt=30`       |
> | `gte`   | Greater than or equal | `age__gte=18`      |
> | `in`    | In a list             | `age__in=[18, 21]` |


<br>

> 📅 &nbsp;Date/Time Field Filters ( <code>DateField</code> , <code>DateTimeField</code> )
>
> 
> | Lookup           | Description                  |
> | ---------------- | ---------------------------- |
> | `date__year`     | Filter by year               |
> | `date__month`    | Filter by month              |
> | `date__day`      | Filter by day                |
> | `date__week_day` | Day of the week (1 = Sunday) |
> | `date__range`    | Range of dates               |

<br>

> <h4>✅ &nbsp;Add Custom Filtering Operators &nbsp;(including related fields)</h4>
> 
> <br>
> 
> Update the <code>list()</code> method in the <code>PeopleViewSet</code> to handle related fields properly using <code>__</code> lookup &nbsp;**:**
> 
> 
> > <code>person_api/home/views.py</code>
> > ```
> > from django.db.models import Q
> > 
> > 
> > class PeopleViewSet(viewsets.ModelViewSet):
> >     serializer_class = PersonSerializer
> >     queryset = Person.objects.all()
> > 
> >     def list(self, request):
> > 	
> >         name = request.GET.get('name')
> >         age_min = request.GET.get('age_min')
> >         age_max = request.GET.get('age_max')
> >         color_name = request.GET.get('color')
> > 
> >         queryset = self.queryset
> > 
> >         if name:
> >             queryset = queryset.filter(Q(name__icontains=name))
> > 
> >         if age_min:
> >             queryset = queryset.filter(age__gte=age_min)
> > 
> >         if age_max:
> >             queryset = queryset.filter(age__lte=age_max)
> > 
> >         if color_name:
> >             queryset = queryset.filter(color__color_name__iexact=color_name)
> > 
> >         serializer = self.serializer_class(queryset, many=True)           # PersonSerializer(queryset, many=True)
> >         return Response({'status': 200, 'data': serializer.data})
> > ```
> 
> <br>
>
> > <code>GET</code> &nbsp;&nbsp;http://localhost:8000/api/people/?color=red
> > 
> > 
> > **Output &nbsp;:**
> > ```
> > {
> >     "status": 200,
> >     "data": [
> >         {
> >             "id": 1,
> >             "name": "Avinash Mishra",
> >             "age": 32,
> >             "color": 1,
> >             "color_info": {
> >                 "color_name": "RED",
> >                 "hex_code": "#ff0000"
> >             }
> >         },
> >         {
> >             "id": 22,
> >             "name": "Bechan Mishra",
> >             "age": 54,
> >             "color": 1,
> >             "color_info": {
> >                 "color_name": "RED",
> >                 "hex_code": "#ff0000"
> >             }
> >         }
> >     ]
> > }
> > ```
> 
> <br>
> 
> > <code>GET</code> &nbsp;&nbsp;http://localhost:8000/api/people/?color=red&name=avi
> > 
> > 
> > **Output &nbsp;:**
> > ```
> > {
> >     "status": 200,
> >     "data": [
> >         {
> >             "id": 1,
> >             "name": "Avinash Mishra",
> >             "age": 32,
> >             "color": 1,
> >             "color_info": {
> >                 "color_name": "RED",
> >                 "hex_code": "#ff0000"
> >             }
> >         }
> >     ]
> > }
> > ```

<br>

<h3>⬛ &nbsp;Filterset &nbsp;class</h3>

<br>

In Django REST Framework, <code>filterset_class</code> allows you to **create reusable, customizable filtering logic** using **Django Filter**. <br>This is far cleaner than writing raw <code>.filter()</code> queries in your views.

<br>

> 🔸 &nbsp;Install <code>django-filter</code> &nbsp;(if not already installed) :
> ```
> pip install django-filter
> ```
> 
> <br>
> 
> 🔸 &nbsp;And in <code>person_api/core/settings.py</code> , add :
> ```
> INSTALLED_APPS = [
>     ●●●
>     'django_filters',
> ]
> ```
> 
> <br>
> 
> 🔸 &nbsp;The <code>Person</code> model and the related models like <code>Color</code> are already in place. &nbsp;So nothing to do on that file.
> 
> <br>
> 
> 🔸 &nbsp;Create a new file <code>person_api/home/filters.py</code> 
> ```
> from django_filters import rest_framework as filters
> from .models import Person
> 
> 
> class PersonFilter(filters.FilterSet):
>     name = filters.CharFilter(lookup_expr='icontains')
>     min_age = filters.NumberFilter(field_name='age', lookup_expr='gte')
>     max_age = filters.NumberFilter(field_name='age', lookup_expr='lte')
>     color = filters.CharFilter(field_name='color__color_name', lookup_expr='icontains')
> 
>     class Meta:
>         model = Person
>         fields = ['name', 'min_age', 'max_age', 'color']
> ```
> 
> <br>
> 
> 🔸 &nbsp;<code>person_api/home/views.py</code> &nbsp;**(ViewSet)**
> ```
> ●●●
> from django_filters.rest_framework import DjangoFilterBackend
> from .filters import PersonFilter
> 
> 
> class PeopleViewSet(viewsets.ModelViewSet):
>     queryset = Person.objects.select_related('color').all()
>     serializer_class = PersonSerializer
> 
>     filter_backends = [DjangoFilterBackend]
>     filterset_class = PersonFilter
> ```
> 
> <br>
> 
> 🔸 &nbsp;The corresponding **router viewset** for <code>PeopleViewSet</code> is already set in <code>person_api/api/urls.py</code> <br>
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; So nothing to do on that side.

<br>

> Now you can veirfy your filters.
>
> <br>
>
> > <code>GET</code> &nbsp;&nbsp;http://localhost:8000/api/people/?name=mishra
> > ```
> > [
> >     {
> >         "id": 1,
> >         "name": "Avinash Mishra",
> >         "age": 32,
> >         "color": 1,
> >         "color_info": {
> >             "color_name": "RED",
> >             "hex_code": "#ff0000"
> >         }
> >     },
> >     {
> >         "id": 2,
> >         "name": "Aman Mishra",
> >         "age": 27,
> >         "color": null,
> >         "color_info": null
> >     },
> >     {
> >         "id": 22,
> >         "name": "Bechan Mishra",
> >         "age": 54,
> >         "color": 1,
> >         "color_info": {
> >             "color_name": "RED",
> >             "hex_code": "#ff0000"
> >         }
> >     },
> >     {
> >         "id": 25,
> >         "name": "Bina Mishra",
> >         "age": 50,
> >         "color": 2,
> >         "color_info": {
> >             "color_name": "BLUE",
> >             "hex_code": "#0000ff"
> >         }
> >     }
> > ]
> > ```
> 
> <br>
>
> > <code>GET</code> &nbsp;&nbsp;http://localhost:8000/api/people/?name=mishra&min_age=30
> > ```
> > [
> >     {
> >         "id": 1,
> >         "name": "Avinash Mishra",
> >         "age": 32,
> >         "color": 1,
> >         "color_info": {
> >             "color_name": "RED",
> >             "hex_code": "#ff0000"
> >         }
> >     },
> >     {
> >         "id": 22,
> >         "name": "Bechan Mishra",
> >         "age": 54,
> >         "color": 1,
> >         "color_info": {
> >             "color_name": "RED",
> >             "hex_code": "#ff0000"
> >         }
> >     },
> >     {
> >         "id": 25,
> >         "name": "Bina Mishra",
> >         "age": 50,
> >         "color": 2,
> >         "color_info": {
> >             "color_name": "BLUE",
> >             "hex_code": "#0000ff"
> >         }
> >     }
> > ]
> > ```
> 
> <br>
> 
> > <code>GET</code> &nbsp;&nbsp;http://localhost:8000/api/people/?name=mishra&min_age=30&max_age=52
> > ```
> > [
> >     {
> >         "id": 1,
> >         "name": "Avinash Mishra",
> >         "age": 32,
> >         "color": 1,
> >         "color_info": {
> >             "color_name": "RED",
> >             "hex_code": "#ff0000"
> >         }
> >     },
> >     {
> >         "id": 25,
> >         "name": "Bina Mishra",
> >         "age": 50,
> >         "color": 2,
> >         "color_info": {
> >             "color_name": "BLUE",
> >             "hex_code": "#0000ff"
> >         }
> >     }
> > ]
> > ```
> 
> <br>
> 
> > <code>GET</code> &nbsp;&nbsp;http://localhost:8000/api/people/?name=mishra&min_age=30&max_age=52&color=blu
> > ```
> > [
> >     {
> >         "id": 25,
> >         "name": "Bina Mishra",
> >         "age": 50,
> >         "color": 2,
> >         "color_info": {
> >             "color_name": "BLUE",
> >             "hex_code": "#0000ff"
> >         }
> >     }
> > ]
> > ```

<br>

<h3>⬛ &nbsp;Summary of Different Kinds of Filters</h3>

<br>

| Method             | Type           | Power    | When to Use                            |
| ------------------ | -------------- | -------- | -------------------------------------- |
| Manual `.get()`    | Function &nbsp;/&nbsp; Class | Basic    | Quick filters in small APIs            |
| `filterset_fields` | ViewSet &nbsp;/&nbsp; Class Based View  | Moderate | For simple exact filters               |
| `filterset_class`  | ViewSet &nbsp;/&nbsp; Class Based View  | Advanced | For range, icontains, date filters, etc. |

<br>

<h1>Hard Delete &nbsp;&nbsp;&nbsp;vs&nbsp;&nbsp;&nbsp; Soft Delete</h1>

<br>

<h3>1️⃣ &nbsp;What is Hard Delete ?</h3>

<br>

🔸 &nbsp;**<ins>Definition</ins> :**

> **Hard delete** means **permanent removal** of the record from the database table.

Once hard deleted :
- The data is **physically gone** from the database.
- Cannot be recovered unless you have backups.

<br>

🔸 &nbsp;**<ins>Example</ins> :**

> > Imagine you have a <code>Person</code> table :
> > 
> > | id | name | age | color |
> > | -- | ---- | --- | ----- |
> > | 1  | John | 25  | Red   |
> > | 2  | Mike | 30  | Blue  |
> <br>
> 
> > If you run :
> > ```
> > Person.objects.get(id=1).delete()
> > ```
> <br>
> 
> > Now, the row with <code>id=1</code> is permanently deleted.
> >
> > New table content :
> > | id | name | age | color |
> > | -- | ---- | --- | ----- |
> > | 2  | Mike | 30  | Blue  |

<br>

🔸 &nbsp;**<ins>Key Points</ins> :**
- Simple to implement.
- No special logic required.
- Good for data that will never be needed again.
- Not suitable for audit/compliance/legal data where recovery may be required.

<br>

<h3>2️⃣ &nbsp;What is Soft Delete ?</h3>

<br>

**🔸 &nbsp;<ins>Definition</ins> :**

**Soft delete** means marking the record as deleted but keeping it in the database.

Instead of deleting physically :

- You add a **"deletion marker field"** (e.g., <code>is_deleted=True</code>).

- Data remains in the database, but system treats it as deleted.

<br>

**🔸 &nbsp;<ins>Example Schema</ins> :**

| id | name | age | color | is\_deleted |
| -- | ---- | --- | ----- | ----------- |
| 1  | John | 25  | Red   | False       |
| 2  | Mike | 30  | Blue  | False       |

After soft delete :
```
person = Person.objects.get(id=1)
person.is_deleted = True
person.save()
```

Now :
| id | name | age | color | is\_deleted |
| -- | ---- | --- | ----- | ----------- |
| 1  | John | 25  | Red   | True        |
| 2  | Mike | 30  | Blue  | False       |

<br>

**🔸 &nbsp;<ins>Benefits</ins> :**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;✅&nbsp; Recovery is possible ( simply set <code>is_deleted=False</code> ) <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;✅&nbsp; Great for auditing, compliance, legal, safety-critical data. <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;✅&nbsp; Useful for user-facing apps where "trash bin" or "undo delete" is needed. <br>

<br>

**🔸 &nbsp;<ins>Drawbacks</ins> :**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;●&nbsp; Requires additional logic in : <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;◾️&nbsp; Queries (must always filter is_deleted=False) <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;◾️&nbsp; ORM manager overrides <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;◾️&nbsp; Business logic <br>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;●&nbsp; Data volume keeps growing

<br>

<h3>3️⃣ &nbsp;When to Use Hard vs Soft Delete ?</h3>

<br>

| Scenario                                                   | Recommendation  |
| ---------------------------------------------------------- | --------------- |
| Legal compliance required (GDPR, audits, medical data)     | **Soft Delete** |
| Application offers “Recycle Bin” / restore feature         | **Soft Delete** |
| Sensitive data where accidental delete must be recoverable | **Soft Delete** |
| Temporary inactive users                                   | **Soft Delete** |
| Logs, temporary cache, expired tokens                      | **Hard Delete** |
| Low-value, high-volume data (click logs, telemetry)        | **Hard Delete** |
| Simple admin tools without restore needs                   | **Hard Delete** |

<br>

<h3>4️⃣ &nbsp;How to Implement Soft Delete in Django ?</h3>

<br>

This solution does not involve creating separate APIs for <ins>bulk hard delte</ins> , <ins>bulk soft delete</ins> and <ins>bulk restore</ins>. &nbsp;⭐<br>
It's just a <ins>**ORM-based method chaining**</ins> which is perfect for internal code (service layer, management commands, admin panels, batch jobs). 

<br>

**🔸 &nbsp;<ins>Step 1</ins> &nbsp;➔&nbsp; Add** <code>is_deleted</code> **field to model**

<br>

<code>person_api/home/models.py</code>
```
class Person(models.Model):
    name = models.CharField(max_length=100)
    age = models.IntegerField()
    color = models.ForeignKey(Color, null=True, blank=True, on_delete=models.CASCADE)
    is_deleted = models.BooleanField(default=False)
```

Now run this on the terminal :
```
py manage.py makemigrations
py manage.py migrate
```

<br>

**🔸 &nbsp;<ins>Step 2</ins> &nbsp;➔&nbsp; Override model's** <code>delete()</code> **method**

<br>

> <code>person_api/home/models.py</code>
> ```
> class Person(models.Model):
>     ●●●
>     is_deleted = models.BooleanField(default=False)
> 
>     def delete(self, using=None, keep_parents=False):        # soft delete
>         self.is_deleted = True
>         self.save()
> ```

<br>

> You'll also have to restore a soft-deleted person. To do it : <br>
> <code>person_api/home/models.py</code>
> ```
> class Person(models.Model):
>     ●●●
>     is_deleted = models.BooleanField(default=False)
> 
>     def delete(self, using=None, keep_parents=False):        # soft delete
>         self.is_deleted = True
>         self.save()
>
>     def restore(self):                                       # restores the soft-deleted person
>         self.is_deleted = False
>         self.save()
> ```

<br>

> Now that you have over-ridden the <code>delete()</code> method for **soft delete**, you'll have to write a method for **hard delete** also : <br>
> <code>person_api/home/models.py</code>
> ```
> class Person(models.Model):
>     ●●●
>     is_deleted = models.BooleanField(default=False)
> 
>     def delete(self, using=None, keep_parents=False):        # soft delete
>         self.is_deleted = True
>         self.save()
>
>     def restore(self):                                       # restores the soft-deleted person
>         self.is_deleted = False
>         self.save()
> 
>     def hard_delete(self):                                   # removes the person physically from the Person table
>         super().delete()
> ```

<br>

**🔸 &nbsp;<ins>Step 3</ins> &nbsp;➔&nbsp; (optional but recommended) &nbsp;: &nbsp;Create a custom model manager**

<br>

<code>person_api/home/models.py</code>
```
class SoftDeleteManager(models.Manager):
    def get_queryset(self):
        return super().get_queryset().filter(is_deleted=False)


class Person(models.Model):
    ●●●
    is_deleted = models.BooleanField(default=False)

    objects = SoftDeleteManager()                                # Default manager: returns only active 
    all_objects = models.Manager()                               # Full manager: returns all (including deleted)

    def delete(self, using=None, keep_parents=False):            # soft delete
        self.is_deleted = True
        self.save()

    def restore(self):                                           # restores the soft-deleted person
        self.is_deleted = False
        self.save()

    def hard_delete(self):                                       # removes the person physically from the Person table
        super().delete()
```

<br>

**🔸 &nbsp;<ins>Step 4</ins> &nbsp;➔&nbsp; Use your manager carefully**

<br>

&nbsp;&nbsp;Default &nbsp;&nbsp;:&nbsp;&nbsp; <code>Person.objects.all()</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;⟶&nbsp;&nbsp; returns only non-deleted records <br>
&nbsp;&nbsp;Use &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;:&nbsp;&nbsp; <code>Person.all_objects.all()</code> &nbsp;&nbsp;&nbsp;⟶&nbsp;&nbsp; returns everything

<br>

**🔸 &nbsp;<ins>Step 5</ins> &nbsp;➔&nbsp; Optional &nbsp;(if you want to show the** <code>is_deleted</code> **field)**

<br>

```
class PersonSerializer(serializers.ModelSerializer):

    ●●●

    class Meta:
        model = Person
        # fields = '__all__'
        fields = ['id', 'name', 'age', 'color', 'color_info', 'is_deleted']           # custom order
    
    ●●●
```

<br>

<h3>5️⃣ &nbsp;Queries in Soft Delete</h3>

<br>

| Operation                                 | Code                                                                 |
| ----------------------------------------- | -------------------------------------------------------------------- |
| &nbsp;&nbsp;Normal list&nbsp;&nbsp;       | &nbsp;&nbsp;`Person.objects.all()`&nbsp;&nbsp;                       |
| &nbsp;&nbsp;Including deleted&nbsp;&nbsp; | &nbsp;&nbsp;`Person.all_objects.all()`&nbsp;&nbsp;                   |
| &nbsp;&nbsp;Only deleted&nbsp;&nbsp;      | &nbsp;&nbsp;`Person.all_objects.filter(is_deleted=True)`&nbsp;&nbsp; |

<br>

> Run <code>Python manage.py shell</code> in the terminal to checkout these following lines&nbsp;:
> 
> 
> | Code                                            | Output                                                                                    |
> |-------------------------------------------------|-------------------------------------------------------------------------------------------|
> | <code>p1 = Person.all_objects.get(id=XX)</code>&nbsp;&nbsp; | returns the person with the <code>id = XX</code>                              |
> | <code>p1.is_deleted</code>                      | returns <code>False</code>                                                                |
> | <code>p1.delete()</code>                        | **soft deletes** the person i.e, <code>person.is_deleted = True</code>                    |
> | <code>p1.is_deleted</code>                      | returns <code>True</code>                                                                 |
> | <code>p1.restore()</code>                       | **restores** the person i.e, <code>person.is_deleted = False</code>                       |
> | <code>p1.is_deleted</code>                      | returns <code>False</code>                                                                |
> | <code>p1.hard_delete()</code>                   | **removes** the person physically                                                         |
> | <code>Person.all_objects.get(id=XX)</code>      | **ERROR** &nbsp;➜&nbsp; <code>home.models.Person.DoesNotExist: Person matching query does not exist.</code>&nbsp; |

<br>

<h3>6️⃣ &nbsp;How to Implement Bulk Soft Delete ?</h3>

<br>

> Let’s say you want to **delete multiple persons at once**.
>
> <br>
> 
> Run the following code in the shell &nbsp;➜&nbsp; <code>Python manage.py shell</code> &nbsp;**:**
> ```
> ids_to_delete = [34, 37, 38]
> Person.objects.filter(id__in=ids_to_delete).update(is_deleted=True)
> ```
>
> <br>
> 
> Note that it works even if some of these **ids** are not present.
>
> <br>
> 
> But this solution is perfect for internal code (service layer, management commands, admin panels, batch jobs). It's actually a **ORM-based method chaining** and does not involve creating separate APIs for <ins>bulk hard delte</ins> , <ins>bulk soft delete</ins> and <ins>bulk restore</ins>. &nbsp;⭐


<br>

<h4>✅ &nbsp;Will &nbsp;<code>persons.delete()</code>&nbsp; in the &nbsp;<code>bulk-delete</code>&nbsp; api also work for <ins>bulk soft delete</ins> ? &nbsp; If not, why ?</h4>

<br>

> ```
> persons = Person.objects.filter(id__in=ids_to_delete)
> ```
> 
> Since you have already overridden the <code>delete()</code> method of the <code>Person</code> model, you may want to verify if <code>persons.delete()</code> also does the **bulk soft delete** like <code>persons.update(is_deleted=True)</code> ? &nbsp;⭐

<br>

> ∎ &nbsp;You can check it through the bulk delete api ( http://localhost:8000/api/person/bulk-delete/ ) which uses <code>persons.delete()</code>
> 
> ∎ &nbsp;However, you may quickly verify it through the <code>Django shell</code> also. <br>
> > <code>python manage.py shell</code>
> > ```
> > ids_to_delete = [41, 42, 43, 44]
> > Person.objects.filter(id__in=ids_to_delete).delete()         # returns the number of persons deleted
> > p = Person.all_objects.get(id=41)                            # ERROR : person does not exist ➜ hard deleted
> > ```

<br>

> 🔶 &nbsp;Now the question is &nbsp;-&nbsp; why does <code>Person.objects.filter(id__in=ids_to_delete).delete()</code> <ins>not do the **Bulk Soft Delete**</ins> ?
> 
> <br>
> 
> Because when you call <code>.delete()</code> on a **queryset**, Django does **bulk hard delete**. &nbsp;**⭐** <br>
> It does **NOT** call the instance method <code>delete()</code> on each object. <br>
> This is how <ins>Django ORM delete</ins> works.
>
> <br>
>
> > If you call <code>instance.delete()</code> , Django calls :
> > ```
> > def delete(self, using=None, keep_parents=False):
> >     # model level delete
> > ```
> 
> <br>
> 
> > But if you call <code>queryset.delete()</code> , Django directly executes SQL like :
> > ```
> > DELETE FROM person WHERE id IN (1, 2, 3);
> > ```
> >
> > 👉 &nbsp; **It bypasses your model’s** <code>delete()</code> **method.**

<br>

<h4>✅ &nbsp;Solution for Bulk Soft Delete</h4>

<br>

> There are 3 solutions &nbsp;**:**
> 
> <br>
> 
> - Either you use **ORM-based method chaining** to make <code>Person.objects.filter(id__in=[41, 42, 43]).delete()</code> work for **bulk soft delete** (which is currently a **bulk hard delete**) so that you can _use the already created this bulk-delete api &nbsp;http://localhost:8000/api/person/bulk-delete/ &nbsp;for **bulk soft delete**_.
> 
> <br>
> 
> - Or create <ins>_separate APIs_</ins> for &nbsp; - &nbsp;**Bulk Soft Delete** , &nbsp;**Bulk Hard Delete** &nbsp;&&nbsp; **Bulk Restore**. 
> 
> <br>
> 
> - Or you can use a <ins>**hybrid** of the above 2 solutions</ins> taking best from each of them. &nbsp;⭐

<br>

<h1>Bulk Soft Delete</h1>

<br>

<h2>⬛ &nbsp;Objectives &nbsp;:</h2>

<br>

| API                             | Behavior                                 |
| ------------------------------- | ---------------------------------------- |
| `http://localhost:8000/api/person/`                  | Standard CRUD for active persons         |
| `http://localhost:8000/api/person/all/` <br><br> `http://localhost:8000/api/person/?include_deleted=true` <br> `http://localhost:8000/api/person/?include_deleted=false`<br> | Separate endpoint for Admin only &nbsp;:&nbsp; returns <ins>alive</ins> + <ins>soft-deleted</ins> <br><br> Toggle via query param to make it even more flexible <br>instead of creating a separate endpoint |
| `http://localhost:8000/api/person/bulk-soft-delete/` | Soft delete bulk                         |
| `http://localhost:8000/api/person/bulk-hard-delete/` | Hard delete bulk                         |
| `http://localhost:8000/api/person/bulk-restore/`     | Restore bulk                             |

<br>

<h2>⬛ &nbsp;Hybrid Solution &nbsp;:</h2>

<br>

The Hybrid Solution combines &nbsp;**:**

✅ &nbsp;Model level reusable soft delete logic (with ORM chaining)

✅ &nbsp;API endpoints for controlled public-facing operations

✅ &nbsp;ORM flexibility for internal operations

✅ &nbsp;Full control for &nbsp;**:** <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ● &nbsp;soft delete      <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ● &nbsp;hard delete      <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ● &nbsp;restore          <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ● &nbsp;bulk operations  <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ● &nbsp;toggle filter for retrieving persons who were soft-deleted (and hard deleted both)  <br>

<br>

<h3>1️⃣ &nbsp;&nbsp;<code>person_api/home/models.py</code></h3>

<br>

```
from django.db import models



class SoftDeleteQuerySet(models.QuerySet):
    def delete(self):
        return super().update(is_deleted=True)

    def restore(self):
        return super().update(is_deleted=False)
		
	def hard_delete(self):
        return super().delete()



class SoftDeleteManager(models.Manager):
    def get_queryset(self):
        return SoftDeleteQuerySet(self.model, using=self._db).filter(is_deleted=False)



class Color(models.Model):
    ●●●



class Person(models.Model):
    name = models.CharField(max_length=100)
    age = models.IntegerField()
    color = models.ForeignKey(Color, null=True, blank=True, on_delete=models.CASCADE, related_name="color")
    is_deleted = models.BooleanField(default=False)

    objects = SoftDeleteManager()                             # Default manager : returns only active
    all_objects = SoftDeleteQuerySet.as_manager()             # All objects (active + deleted)

    def delete(self, using=None, keep_parents=False):         # instance delete for single soft delete
        self.is_deleted = True
        self.save()

    def restore(self):                                        # restores the soft-deleted person
        self.is_deleted = False
        self.save()

    def hard_delete(self):                                    # removes the person physically from the Person table
        super().delete()
```

◼️ &nbsp;Now soft-delete behavior is fully encapsulated at model/queryset level.

<br>

<h3>2️⃣ &nbsp;&nbsp;<code>person_api/home/serializers.py</code></h3>

<br>

```
from rest_framework import serializers
from .models import Person



class PersonSerializer(serializers.ModelSerializer):
    class Meta:
        model = Person
        # fields = '__all__'
        fields = ['id', 'name', 'age', 'color', 'color_info', 'is_deleted']
    
    ●●●
```

<br>

<h3>3️⃣  &nbsp;&nbsp;<code>person_api/home/views.py</code>  &nbsp;—&nbsp;  Hybrid API views</h3>

<br>

These APIs give you public control while still allowing you to use ORM directly wherever needed.

<br>

**🔸 &nbsp;Bulk Soft Delete API**

<br>

```
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status
from .models import Person



class BulkSoftDeleteAPIView(APIView):
    def post(self, request):
        ids = request.data.get('ids', [])
        Person.objects.filter(id__in=ids).delete()
        return Response({"message": "Soft deleted successfully."}, status=status.HTTP_200_OK)
```

<br>

**🔸 &nbsp;Bulk Restore API**

<br>

```
class BulkRestoreAPIView(APIView):
    def post(self, request):
        ids = request.data.get('ids', [])
        Person.all_objects.filter(id__in=ids).restore()
        return Response({"message": "Restored successfully."}, status=status.HTTP_200_OK)
```

<br>

**🔸 &nbsp;Bulk Hard Delete API**

<br>

```
class BulkHardDeleteAPIView(APIView):
    def post(self, request):
        ids = request.data.get('ids', [])
        Person.all_objects.filter(id__in=ids).hard_delete()
        return Response({"message": "Hard deleted successfully."}, status=status.HTTP_200_OK)
```

<br>

**🔸 &nbsp;Why using <code>post()</code> for all these bulk operations - <code>bulk-soft-delete</code> or <code>bulk-restore</code> or <code>bulk-hard-delete</code> &nbsp;?**

<br>

> You could have used &nbsp;**:**
> - <code>PATCH</code> &nbsp;for <ins>**Bulk Soft Delete**</ins> or <ins>**Bulk Restore**</ins> and 
> - <code>DELETE</code> &nbsp;for <ins>**Bulk Hard Delete**</ins>
>
> like you have used earlier &nbsp;**:**
>
> - <code>POST</code> &nbsp;&nbsp;&nbsp;&nbsp;request &nbsp;in &nbsp;<code>BulkPersonCreateView</code> &nbsp;for&nbsp; <code>/api/person/bulk-create/</code>
> - <code>PUT</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;request &nbsp;in &nbsp;<code>BulkPersonUpdateView</code> &nbsp;for&nbsp; <code>/api/person/bulk-update/</code>
> - <code>DELETE</code> &nbsp;request &nbsp;in &nbsp;<code>BulkPersonDeleteView</code> &nbsp;for&nbsp; <code>/api/person/bulk-delete/</code>
>
> and it worked fine, so why do people often use <code>POST</code> request for these kind of bulk operations in enterprise or production grade APIs ?
> 
> <br>
> 
> > Because &nbsp;**:**
> > 
> > - `PATCH` request generally assumes partial update on existing resource
> > 
> > - Here `/api/person/bulk-soft-delete/` is actually <ins>not a resource URL</ins> &nbsp;—&nbsp; it’s a **bulk action endpoint**
> > 
> > - There is no resource called `bulk-soft-delete` , it's actually an **action/operation** , so it's common to model such operation-specific endpoints as `POST`
> > 
> > - The same is true for `/api/person/bulk-soft-delete/`

<br>

> Industry common practice (for bulk actions) &nbsp;**:**
> 
> > ✅ POST — for action endpoints (e.g., bulk-soft-delete, bulk-restore, send-email, sync-data, export-csv etc.)
> > 
> > ✅ PATCH — for partial updates of resource (per-record)
> 
> DRF community (and many big APIs like Stripe, Shopify, etc.) recommend `POST` for bulk operations. ⭐

<br>

> | Scenario                         | Recommended                                         |
> | -------------------------------- | --------------------------------------------------- |
> | Soft Delete of **single person** | `PATCH /api/person/{id}/ { is_deleted: true }`      |
> | Soft Delete of **bulk persons**  | `POST /api/person/bulk-soft-delete/ { ids: [...] }` |

<br>

> Rule of Thumb &nbsp;:
> - If your URL represents a resource, use **RESTful** verbs ( `GET`, `POST`, `PUT`, `PATCH`, `DELETE` )
> - If your URL represents an action or operation, then `POST` is safest

<br>

**🔸 &nbsp;Regular DRF ViewSet for normal CRUD operations &nbsp;:**

<br>

```
from rest_framework import viewsets
from .models import Person
from .serializers import PersonSerializer


class PersonViewSet(viewsets.ModelViewSet):
    queryset = Person.objects.all()                # Only non-deleted
    serializer_class = PersonSerializer
```

<br>

> But let's not use it in this case since you already implemented explicit function-based views : <br>
> 
> ✔️ `person(request)`    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;➜&nbsp;&nbsp;   `/api/person/` &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;➜&nbsp;&nbsp;  `list` , `create` , `update` , `patch` , `delete` &nbsp;(single object) <br>
> 
> ✔️ `person_detail(id)`   &nbsp;&nbsp;➜&nbsp;&nbsp;   `/api/person/<id>/`  &nbsp;&nbsp;➜&nbsp;&nbsp;  `retrieve` , `update` , `patch` , `delete` &nbsp;(by ID)

<br>

So, you don’t need the <code>PersonViewSet</code> &nbsp;❌ <br>
However, if you want to benefit from <ins>filtering</ins >, <ins>pagination</ins> and <ins>permissions</ins> with minimal code, you may use the <code>PersonViewSet</code>

<br>

<h3>4️⃣   &nbsp;&nbsp;<code>person_api/api/urls.py</code></h3>

<br>

```
from django.urls import path, include
from rest_framework.routers import DefaultRouter
from home.views import (
    index, person, person_detail, login, Persons, PeopleViewSet,
    BulkPersonCreateView, BulkPersonUpdateView, BulkPersonDeleteView,
    BulkSoftDeleteAPIView, BulkHardDeleteAPIView, BulkRestoreAPIView
)


router = DefaultRouter()
router.register(r'people', PeopleViewSet, basename='people')
urlpatterns = router.urls


urlpatterns = [
    path('index/', index),                                                  # Function based view
    path('person/', person),                                                # Function based view
    path('person/<int:id>/', person_detail),                                # Function based view
    path('login/', login),                                                  # Function based view
    path('persons/', Persons.as_view()),                                    # Class based view
    path('', include(router.urls)),                                         # ModelViewSet
    path('person/bulk-create/', BulkPersonCreateView.as_view()),            # Class based view
    path('person/bulk-update/', BulkPersonUpdateView.as_view()),            # Class based view
    path('person/bulk-delete/', BulkPersonDeleteView.as_view()),            # Class based view         # Hard Delete
    path('person/bulk-soft-delete/', BulkSoftDeleteAPIView.as_view()),      # Class based view         # Soft Delete
    path('person/bulk-hard-delete/', BulkHardDeleteAPIView.as_view()),      # Class based view         # Hard Delete
    path('person/bulk-restore/', BulkRestoreAPIView.as_view()),             # Class based view
]
```

<br>

<h3>5️⃣  &nbsp;&nbsp;Let's now verify these APIs</h3>

<br>

> `POST` &nbsp;&nbsp; http://localhost:8000/api/person/bulk-soft-delete/ &nbsp;&nbsp;➜&nbsp;&nbsp; `{ "ids": [71, 72, 73] }`
> <br>
> 
> **Output &nbsp;:**  &nbsp;&nbsp;➜&nbsp;&nbsp; `{ "message": "Soft deleted successfully." }`
>
> <br>
> 
> Let's now verify it through `shell` : 
>
> <code>py manage.py shell</code>                                                                <br>
> > <code>Person.all_objects.get(id=71).is_deleted</code>  &nbsp;&nbsp;&nbsp;&nbsp;===>&nbsp;&nbsp;&nbsp; True  &nbsp;&nbsp;&nbsp;===>&nbsp;&nbsp;&nbsp; **soft deleted**   <br>
> > <code>Person.all_objects.get(id=72).is_deleted</code>  &nbsp;&nbsp;&nbsp;&nbsp;===>&nbsp;&nbsp;&nbsp; True  &nbsp;&nbsp;&nbsp;===>&nbsp;&nbsp;&nbsp; **soft deleted**   <br>
> > <code>Person.all_objects.get(id=73).is_deleted</code>  &nbsp;&nbsp;&nbsp;&nbsp;===>&nbsp;&nbsp;&nbsp; True  &nbsp;&nbsp;&nbsp;===>&nbsp;&nbsp;&nbsp; **soft deleted**   <br>

<br>
&nbsp;

> `POST` &nbsp;&nbsp; http://localhost:8000/api/person/bulk-restore/ &nbsp;&nbsp;➜&nbsp;&nbsp; `{ "ids": [71, 73] }`
> <br>
> 
> **Output &nbsp;:**  &nbsp;&nbsp;➜&nbsp;&nbsp; `{ "message": "Restored successfully." }`
>
> <br>
> 
> Let's now verify it through `shell` : 
>
> <code>py manage.py shell</code>                                                                <br>
> > <code>Person.all_objects.get(id=71).is_deleted</code>  &nbsp;&nbsp;&nbsp;&nbsp;===>&nbsp;&nbsp;&nbsp; False  &nbsp;&nbsp;&nbsp;===>&nbsp;&nbsp;&nbsp; **restored**   <br>
> > <code>Person.all_objects.get(id=72).is_deleted</code>  &nbsp;&nbsp;&nbsp;&nbsp;===>&nbsp;&nbsp;&nbsp; True  &nbsp;&nbsp;&nbsp;&nbsp;===>&nbsp;&nbsp;&nbsp; soft deleted   <br>
> > <code>Person.all_objects.get(id=73).is_deleted</code>  &nbsp;&nbsp;&nbsp;&nbsp;===>&nbsp;&nbsp;&nbsp; False  &nbsp;&nbsp;&nbsp;===>&nbsp;&nbsp;&nbsp; **restored**   <br>

<br>
&nbsp;

> `POST` &nbsp;&nbsp; http://localhost:8000/api/person/bulk-restore/ &nbsp;&nbsp;➜&nbsp;&nbsp; `{ "ids": [71, 72, 73] }`
> <br>
> 
> **Output &nbsp;:**  &nbsp;&nbsp;➜&nbsp;&nbsp; `{ "message": "Hard deleted successfully." }`
> 
> <br>
> 
> Let's now verify it through `shell` : 
>
> <code>py manage.py shell</code>                                                                <br>
> > <code>Person.all_objects.get(id=71).is_deleted</code>  &nbsp;&nbsp;&nbsp;&nbsp;===>&nbsp;&nbsp;&nbsp; Person matching query <ins>does not exist</ins>  &nbsp;&nbsp;&nbsp;===>&nbsp;&nbsp;&nbsp; **hard deleted**   <br>
> > <code>Person.all_objects.get(id=72).is_deleted</code>  &nbsp;&nbsp;&nbsp;&nbsp;===>&nbsp;&nbsp;&nbsp; Person matching query <ins>does not exist</ins>  &nbsp;&nbsp;&nbsp;===>&nbsp;&nbsp;&nbsp; **hard deleted**   <br>
> > <code>Person.all_objects.get(id=73).is_deleted</code>  &nbsp;&nbsp;&nbsp;&nbsp;===>&nbsp;&nbsp;&nbsp; Person matching query <ins>does not exist</ins>  &nbsp;&nbsp;&nbsp;===>&nbsp;&nbsp;&nbsp; **hard deleted**   <br>

<br>

<h3>6️⃣  &nbsp;&nbsp;ORM usage for internal code</h3>

<br>

For any service layer, scheduled tasks, admin panels &nbsp;**:**

<br>

> ✅ &nbsp;**Soft delete internally &nbsp;:**
> 
> `Person.all_objects.filter(id=74).delete()` &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;⟶&nbsp;&nbsp;&nbsp; single soft delete  &nbsp;&nbsp;&nbsp;&nbsp;⟶&nbsp;&nbsp;&nbsp;&nbsp; works fine &nbsp;✔️   <br>
> `Person.objects.filter(id__in=[75,76,77]).delete()` &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;⟶&nbsp;&nbsp;&nbsp; bulk soft delete  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;⟶&nbsp;&nbsp;&nbsp;&nbsp; works fine &nbsp;✔️    <br>

<br>

> ✅ &nbsp;**Restore internally &nbsp;:**
> 
> `Person.all_objects.filter(id=74).restore()` &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;⟶&nbsp;&nbsp;&nbsp; single soft delete  &nbsp;&nbsp;&nbsp;&nbsp;⟶&nbsp;&nbsp;&nbsp;&nbsp; works fine &nbsp;✔️  <br>
> `Person.objects.filter(id__in=[75,76,77]).restore()` &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;⟶&nbsp;&nbsp;&nbsp; bulk soft delete&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;⟶&nbsp;&nbsp;&nbsp;&nbsp; works fine &nbsp;✔️

<br>

> ✅ &nbsp;**Hard delete internally &nbsp;:**
> >
> <code>Person.all_objects.filter(id=74).hard_delete()</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;⟶&nbsp;&nbsp;&nbsp; single hard delete  &nbsp;&nbsp;&nbsp;&nbsp;⟶&nbsp;&nbsp;&nbsp;&nbsp; works fine &nbsp;✔️  &nbsp;&nbsp;&nbsp;&nbsp;⟶&nbsp;&nbsp;&nbsp;&nbsp; returns &nbsp;`1` <br>
> 
> <br>
> 
> <code>Person.objects.filter(id__in=[75,76,77]).hard_delete()</code>  &nbsp;&nbsp;&nbsp;&nbsp;⟶&nbsp;&nbsp;&nbsp;&nbsp; bulk hard delete  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;⟶&nbsp;&nbsp;&nbsp;&nbsp; ERROR &nbsp;❌
> > ```
> > models.Person.MultipleObjectsReturned: get() returned more than one Person -- it returned 3!
> > ```
>
> <br>
> 
> **🔶 &nbsp;Why this error ?**
> 
> You may now think why `bulk-soft-delete` and `bulk-restore` work, but `bulk-hard-delete` does not (**in ORM**), even though they all seem similar on surface.
> 
> That's because the `delete()` and `restore()` methods in the `SoftDeleteQueryText` are actually calling `.update()` method in the background.
> > - `.update()` executes a **single raw SQL query**.
> > - works perfectly on bulk data because Django just <ins>builds a SQL statement on the whole queryset</ins>.
> > - No `.get()` or model-level logic is involved
> 
> 
> Whereas `hard_delete()` is an instance method. 
> 
> You’re trying to call it on a queryset (QuerySet object), but queryset doesn’t know about your custom `hard_delete()` method. 
> 
> Because the statement `super().delete()` in the `hard_delete()` method of the `SoftDeleteQueryText` does not call the default queryset’s `delete()` method. 
> 
> Instead, this internally calls the model’s `delete()` method for each object — BUT — it uses `.get()` to fetch them one-by-one.
> 
> And `.get()` fails when it finds multiple objects. 
> 
> > ⚠️ That’s why you're seeing &nbsp;**:**
> > ```
> > MultipleObjectsReturned: get() returned more than one Person -- it returned 3!
> > ```
> 
> <br>
> 
> **🔶 &nbsp;<ins>Solution 1</ins> &nbsp;➜&nbsp; Iterate on queryset (most common way)** 
> ```
> qs = Person.all_objects.filter(id__in=[75, 76, 77])
> for person in qs:
>     person.hard_delete()
> ```
> This is very safe and fully respects your custom `hard_delete()` logic. &nbsp;👈
> 
> <br>
> 
> **🔶 &nbsp;<ins>Solution 2</ins> &nbsp;➜&nbsp; If you want bulk hard delete behavior directly on QuerySet &nbsp;:**
> 
>  Don't use `super().delete()` for hard delete. <br>
>
> Instead, call the base QuerySet’s `.delete()` method using the unbound base class. 
> 
> > ```
> > class SoftDeleteQuerySet(models.QuerySet):
> >     def hard_delete(self):
> >         return models.QuerySet.delete(self)
> > ```
> > This <ins>bypasses</ins> any custom model-level `.delete()` logic (like your soft-delete override), and just performs raw SQL deletion for all records in queryset. &nbsp;👈
> 
> <br>
> 
> **🔶 &nbsp;<ins>Solution 3</ins> &nbsp;➜&nbsp; If you want both <ins>instance-level</ins> & <ins>queryset-level</ins> deletion routes through your model’s `hard_delete()` method &nbsp;:**
> 
> **➀** &nbsp;Define instance-level `hard_delete()` on model (as you already have).
>
> **➁** &nbsp;Define queryset-level `hard_delete()` like this &nbsp;**:** 
> 
> > ```
> > class SoftDeleteQuerySet(models.QuerySet):
> >     def hard_delete(self):
> >         for obj in self:
> >             obj.hard_delete()
> > ```
> > This way, both single-object and bulk queryset call the <ins>same logic</ins> . And it applies instance-level hard_delete to each one. &nbsp;👈

<br>

✅ &nbsp;**Summary**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;∎&nbsp; `.update()` is safe and fast for bulk ops — perfect for soft delete and restore.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;∎&nbsp; `super().delete()` on queryset might internally use `.get()` or not bypass your model’s `delete()` override.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;∎&nbsp; To do actual hard delete, call the original base class’s `delete()` like &nbsp;&nbsp;➜&nbsp;&nbsp; <code>models.QuerySet.delete(self)</code>

<br>

| Use Case              | Method                             | Fast ?    | Safe for bulk ? | Calls model logic ? |
| --------------------- | ---------------------------------- | --------- | --------------- | ------------------- |
| Soft delete (bulk)    | `.update()`                        | ✅ Fast   | ✅ Yes         | ❌ No              |
| Restore (bulk)        | `.update()`                        | ✅ Fast   | ✅ Yes         | ❌ No              |
| Hard delete (bulk)    | `QuerySet.delete()`                | ✅ Fast   | ✅ Yes         | ❌ No              |
| Hard delete (per obj) | `for obj in qs:`<br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`obj.hard_delete()` | ❌ Slower | ✅ Yes (safe)  | ✅ Yes             |

<br>

✅ &nbsp;**Now your usage will work exactly as you expect in ORM :**

> `Person.all_objects.filter(id__in=[57, 59, 60]).hard_delete()` <br>
> deletes all these 3 persons from the database and returns <code>(3, {'home.Person': 3})</code>

> `Person.all_objects.filter(id__in=[57, 59, 61]).hard_delete()` <br>
> deletes only 1 person from the database and returns `(1, {'home.Person': 1})` since `57` & `59` already hard deleted.

<br>

<h3>7️⃣  &nbsp;&nbsp;Add a feature to <code>GET</code> all persons (<ins>including</ins> hard deleted)</h3>

<br>

> In production systems we often want :
> > &nbsp;&nbsp;∎&nbsp; Normal &nbsp;`GET` &nbsp;➜&nbsp; only alive (non-deleted) records <br>
> > &nbsp;&nbsp;∎&nbsp; Admin &nbsp;&nbsp;`GET` &nbsp;➜&nbsp; return both deleted and non-deleted records
> 
> <br>
> 
> ✅ &nbsp; &nbsp;**`person_api/home/models.py` &nbsp;(already done, unchanged)**
> 
> <br>
> 
> > We already have &nbsp;:
> > ```
> > class Person(models.Model):
> >     ●●●
> >     objects = SoftDeleteManager()                          # Default manager (alive only)
> >     all_objects = SoftDeleteQuerySet.as_manager()          # Full queryset (alive + deleted)
> > ```
> 
> <br>
> 
> ✅ &nbsp; &nbsp;**`person_api/home/serializers.py` &nbsp;(no change needed)**
> 
> <br>
> 
> > We can reuse the same serializer since the `is_deleted` field are already in model.
> > ```
> > class PersonSerializer(serializers.ModelSerializer):
> >     class Meta:
> >         model = Person
> >         fields = '__all__'
> > ```
> > ■ &nbsp;If you want you can also make `is_deleted` `read_only_fields` in serializer for safety.
> 
> <br>
> 
> ✅ &nbsp; &nbsp;**`person_api/home/view.py`**
> 
> <br>
> 
> > Create <ins>new API endpoint</ins> for **Admin** &nbsp;(Get all persons including deleted)
> > ```
> > # /api/person/all/
> > class AdminPersonListAPIView(APIView):
> >     def get(self, request):
> >         persons = Person.all_objects.all()
> >         serializer = PersonSerializer(persons, many=True)
> >         return Response(serializer.data, status=status.HTTP_200_OK)
> > ```
> 
> <br>
> 
> ✅ &nbsp; &nbsp;**Update &nbsp;`person_api/api/urls.py`**
> 
> <br>
> 
> > ```
> > from django.urls import path, include
> > from rest_framework.routers import DefaultRouter
> > from home.views import (
> >     index, person, person_detail, login, Persons, PeopleViewSet,
> >     BulkPersonCreateView, BulkPersonUpdateView, BulkPersonDeleteView,
> >     BulkSoftDeleteAPIView, BulkHardDeleteAPIView, BulkRestoreAPIView, AdminPersonListAPIView       # 👈
> > )
> > 
> > 
> > router = DefaultRouter()
> > router.register(r'people', PeopleViewSet, basename='people')
> > urlpatterns = router.urls
> > 
> > 
> > urlpatterns = [
> >     path('index/', index),                                                  
> >     path('person/', person),                                                
> >     path('person/<int:id>/', person_detail),                                
> >     path('login/', login),                                                  
> >     path('persons/', Persons.as_view()),                                    
> >     path('', include(router.urls)),                                         
> >     path('person/bulk-create/', BulkPersonCreateView.as_view()),            
> >     path('person/bulk-update/', BulkPersonUpdateView.as_view()),            
> >     path('person/bulk-delete/', BulkPersonDeleteView.as_view()),            # Hard Delete
> >     path('person/bulk-soft-delete/', BulkSoftDeleteAPIView.as_view()),      # Soft Delete
> >     path('person/bulk-hard-delete/', BulkHardDeleteAPIView.as_view()),      # Hard Delete
> >     path('person/bulk-restore/', BulkRestoreAPIView.as_view()),             
> >     path('person/all/', AdminPersonListAPIView.as_view()),                  # 👈
> > ]
> > ```
> 
> <br>
> 
> > With this &nbsp;**:** <br>
> > &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;◼️&nbsp; **Public** &nbsp;&nbsp;GET&nbsp; `/api/person/` &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;➜&nbsp;&nbsp; returns only active persons <br>
> > &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;◼️&nbsp; **Admin** &nbsp;GET&nbsp; `/api/person/all/` &nbsp;&nbsp;➜&nbsp;&nbsp; returns both deleted and non-deleted persons

<br>

> ⭐ &nbsp;**Bonus Improvement** &nbsp;&nbsp;-&nbsp;&nbsp; <ins>optional query param for admins to have full control to toggle visibility</ins>
>
> <br>
> 
> If you want even more flexibility, you can make the **"include deleted" toggle** available via <ins>query param</ins> instead of separate endpoint. <br>
> 🔸 `/api/person/?show_deleted=true` <br>
> 🔸 `/api/person/?show_deleted=false`
> 
> <br>
> 
> > Had you the `PersonViewSet` class, you could have easily handled this toggle visibility by using <code>get_queryset()</code>
> > > `person_api/api/views.py`
> > > ```
> > > class PersonViewSet(viewsets.ModelViewSet):
> > >     serializer_class = PersonSerializer
> > > 
> > >     def get_queryset(self):
> > >         include_deleted = self.request.query_params.get('include_deleted', 'false').lower() == 'true'
> > >         if include_deleted:
> > >             return Person.all_objects.all()
> > >         return Person.objects.all()
> > > ```
> > This is much more elegant, fully reusable, no separate admin API needed.
> 
> <br>
>
> > But since you're using **function-based views (FBVs)** for the base API &nbsp;`/api/person/`
> > 
> > You can just handle the query parameter `include_deleted` directly inside the `GET` block of your `person()` view.
> >
> > > `person_api/api/views.py`
> > > ```
> > > # /api/person/
> > > @api_view(['GET', 'POST', 'PUT', 'PATCH', 'DELETE'])
> > > def person(request):
> > >     if request.method == 'GET':
> > >         include_deleted = request.query_params.get('include_deleted', 'false').lower() == 'true'
> > >         if include_deleted:
> > >             # objs = Person.all_objects.all()
> > >             objs = Person.all_objects.select_related('color').filter(color__isnull = False)
> > >         else:
> > >             # objs = Person.objects.all()
> > >             objs = Person.objects.select_related('color').filter(color__isnull = False)
> > >         serializer = PersonSerializer(objs, many = True)
> > >         return Response(serializer.data)
> > > ```
> > >
> > > <br>
> > >
> > > GET &nbsp; `/api/person/?show_deleted=true`    &nbsp;&nbsp;➜&nbsp;&nbsp;  All (including soft-deleted)
> > > 
> > > GET &nbsp; `/api/person/?show_deleted=false`   &nbsp;&nbsp;➜&nbsp;&nbsp;  Only active
> > > 
> > > GET &nbsp; `/api/person/`                      &nbsp;&nbsp;➜&nbsp;  Defaults to active

<br>

> If you want to **debug** &nbsp;**:**
> ```
> print("Show deleted:", show_deleted)
> print("Returned records:", objs.count())
> ```






