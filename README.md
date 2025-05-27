
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
&nbsp;It modifies the function behavior such that it becomes capable of handling all the APIs.

<br>

> <code>person_api/home/views.py</code>
> ```
> from rest_framework.decorators import api_view
> from rest_framework.response import Response
>
> @api_view(['GET'])
> def index(request):
>     my_resume = {
>         "name" : "Avinash Kumar Mishra",
>         "skills" : ["Python", "SQL", "Django Rest Framework", "PySpark", "PowerBI", "AWS"],
>         "education" : ["B.Tech - CSE", "M.Tech - CSE"]
>     }
>     return Response(my_resume)
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

> To resolve this problem &nbsp;&nbsp;&nbsp;&nbsp;➜&nbsp;&nbsp;&nbsp;&nbsp; <code>person_api/home/view.py</code> &nbsp;**:** <br>
> ```
> @api_view(['GET', 'POST'])
> ```

<br>

> <code>POST</code> &nbsp;&nbsp;&nbsp;&nbsp;➜&nbsp;&nbsp;&nbsp;&nbsp; http://localhost:8000/api/index/ &nbsp;&nbsp;&nbsp;&nbsp;➜&nbsp;&nbsp;&nbsp;&nbsp; No error. &nbsp;Same response as in the case of <code>GET</code>

<br>

> So looking at the response, how to distinguish which method is called <code>GET</code> or <code>POST</code> ?
> ```
> @api_view(['GET', 'POST'])
> def index(request):
>     my_resume = {
>         "name" : "Avinash Kumar Mishra",
>         "skills" : ["Python", "SQL", "Django Rest Framework", "PySpark", "PowerBI", "AWS"],
>         "education" : ["B.Tech - CSE", "M.Tech - CSE"]
>     }
>     if request.method == 'GET' :
>         print("You hit a GET method")
>         return Response(my_resume)
>     elif request.method == 'POST' :
>         print("You hit a POST method")
>         return Response(my_resume)
> ```

<br>

> Now hit those urls and check the logs in the **terminal**. <br>
> <code>GET</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;➜&nbsp;&nbsp;&nbsp;&nbsp; http://localhost:8000/api/index/ &nbsp;&nbsp;&nbsp;&nbsp;➜&nbsp;&nbsp;&nbsp;&nbsp; **You &nbsp;hit &nbsp;a &nbsp;GET &nbsp;method** <br>
> <code>POST</code> &nbsp;&nbsp;&nbsp;➜&nbsp;&nbsp;&nbsp;&nbsp; http://localhost:8000/api/index/ &nbsp;&nbsp;&nbsp;&nbsp;➜&nbsp;&nbsp;&nbsp;&nbsp; **You &nbsp;hit &nbsp;a &nbsp;POST &nbsp;method**

<br>

<div>
    <h1>Why Serializer ?</h1>
</div>

<br>

⭐ &nbsp;Database &nbsp;&nbsp;**⟷**&nbsp;&nbsp; <code>Django Query</code> &nbsp;&nbsp;**⟷**&nbsp;&nbsp; **QUERYSET** &nbsp;format &nbsp;&nbsp;**⟷**&nbsp;&nbsp; <code>Serializer</code> &nbsp;&nbsp;**⟷**&nbsp;&nbsp; **JSON** &nbsp;format

> - If you execute <code>Person.objects.all()</code> in the ORM and check it's datatype, you will get to see that the data is in <code>QuerySet</code> format, which cannot be exposed to the frontend. <br>
>
> - To expose data to the frontend in only JSON format &nbsp;&nbsp;&nbsp;**⋮**&nbsp;&nbsp;&nbsp; **QUERYSET** &nbsp;format &nbsp;&nbsp;**⟷**&nbsp;&nbsp; <code>Serializer</code> &nbsp;&nbsp;**⟷**&nbsp;&nbsp; **JSON** &nbsp;format

<br>
