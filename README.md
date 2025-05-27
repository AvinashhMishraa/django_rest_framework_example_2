
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

<code>py manage.py startapp home</code>

<br>

<div>
    <h1>@api_view( )</h1>
</div>

<code>@api_view()</code> is a kind of **decorator** which converts your _existing django (python) function into an api view function_. <br>
It modifies the function behavior such that it becomes capable of handling all the APIs.

<br>

<div>
    <h1>Why Serializer ?</h1>
</div>

> - If you execute <code>Person.objects.all()</code> in the ORM and check it's datatype, you will get to see that the data is in <code>QuerySet</code> format, which cannot be exposed to the frontend. <br>
>
> - To expose data to the frontend in only JSON format &nbsp;&nbsp;&nbsp;:&nbsp;&nbsp;&nbsp; **QUERYSET** &nbsp;format &nbsp;&nbsp;➜&nbsp;&nbsp; <code>Serializer</code> &nbsp;&nbsp;➜&nbsp;&nbsp; **JSON** &nbsp;format

<br>
