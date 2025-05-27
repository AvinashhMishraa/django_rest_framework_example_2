```
    py -m venv env
    ./env/Scripts/activate
    pip install django
    django-admin startproject core .
    pip install djangorestframework
```

<div>
    <h1>Why Serializer ?</h1>
</div>

> - If you execute <code>Person.objects.all()</code> in the ORM and check it's datatype, you will get to see that the data is in <code>QuerySet</code> format, which cannot be exposed to the frontend. <br>
>
> - To expose data to the frontend in only JSON format &nbsp;&nbsp;&nbsp;:&nbsp;&nbsp;&nbsp; **QUERYSET** &nbsp;format &nbsp;&nbsp;➜&nbsp;&nbsp; <code>Serializer</code> &nbsp;&nbsp;➜&nbsp;&nbsp; **JSON** &nbsp;format
