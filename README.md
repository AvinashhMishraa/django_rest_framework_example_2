
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
> ∎ &nbsp;<code>self.instance</code> exists if it's an update ( <code>PUT</code>, <code>PATCH</code> ) <br>
>
> ∎ &nbsp;We fallback to self.instance.field if a field is missing from data ( i.e., not part of the <code>PATCH</code> ) <br>
>
> ∎ &nbsp;This works for <code>POST</code>, <code>PUT</code>, and <code>PATCH</code> safely <br>
>
> ∎ &nbsp;Let's see why it works ? <br>
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

<div>
    <h1>Serializing Foreign Key</h1>
</div>

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
>     }
> ]
> ```

<br>

> But what if we want only the <code>color_name</code> in the output and not any other field of the <code>Color</code> model? <br>
>
> In that case, we can't use <code>depth</code> because it serializes <ins>**all the fields**</ins> of the <code>Color</code> model. &nbsp;⚠️ <br>
>
> We have to remove <code>depth = 1</code> from <code>PersonSerializer</code> class and then create a class of <code>ColorSerializer</code> which will help Django Rest Framework to understand the structure of the data that needs to be passed ?
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
> >         fields = ['color_name']
> > 
> > 
> > class PersonSerializer(serializers.ModelSerializer):
> >     
> >     color = ColorSerializer()
> > 
> >     class Meta:
> >         model = Person
> >         fields = '__all__'
> >         # depth = 1
> > 
> >     def validate(self, data):
> >      	  ●●●
> > ```
> <br>
> 
> ```
> [
>     {
>         "id": 1,
>         "color": {
>             "color_name": "RED"
>         },
>         "name": "A1",
>         "age": 25
>     },
>     {
>         "id": 3,
>         "color": {
>             "color_name": "BLUE"
>         },
>         "name": "A3",
>         "age": 40
>     }
> ]
> ```

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
> >     color = ColorSerializer()
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
> ```
> [
>     {
>         "id": 1,
>         "name": "A1",
>         "age": 25,
>         "color": {
>             "color_name": "RED"
>         }
>     },
>     {
>         "id": 3,
>         "name": "A3",
>         "age": 40,
>         "color": {
>             "color_name": "BLUE"
>         }
>     }
> ]
> ```

<br>

> And if you want to display other fields too like the <code>id</code> of the color : <br>
> <br>
> 
> > <code>person_api/home/serializers.py</code>
> > ```
> > ●●●
> >
> > class ColorSerializer(serializers.ModelSerializer):
> >     
> >     class Meta:
> >         model = Color
> >         fields = ['color_name', 'id']
> >
> > ●●●
> > ```
> <br>
> 
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
>     }
> ]
> ```

<br>

<h3>Difference &nbsp;between &nbsp;using &nbsp;<ins><code>depth</code> &nbsp;in &nbsp;a &nbsp;serializer &nbsp;class</ins> &nbsp;&&nbsp; <ins>nested serializer</ins></h3>

<br>

> <code>depth</code> is a shortcut to include **related models** in the API response automatically. It is used in a **serializer class** to serialize **all fields** of different related models. It automatically follows foreign key and reverse relationships and includes the full nested objects.
> <br><br>
> 
> Whereas calling the serializer class (let say model **'A'**) inside another serializer class (let say model **'B'**) gives you the control of customization and allows you to chose as to which fields of the model **'B'** you want to serialize. In short, it gives **full control** over how related objects are serialized.
>
> 

