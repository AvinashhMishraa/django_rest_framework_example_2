
ORM &nbsp;➜&nbsp; <code>Person.objects.all()</code> &nbsp;➜&nbsp; [1, 2, 3, 4] &nbsp;➜&nbsp; <code>QuerySet</code><br>
whcih cannot be exposed to the frontend.
We need to expose the data only in the JSON format. To achive this, we need a <code>serializer</code>

<br>

<div>
    <h1>Why Serializer ?</h1>
</div>


>If you execute <code>Person.objects.all()</code> in the ORM and check it's datatype, you will get to see that the data is in <code>QuerySet</code> format, which cannot be exposed to the frontend. <br>
>
>To expose data to the frontend in only JSON format &nbsp;&nbsp;&nbsp;:&nbsp;&nbsp;&nbsp; **QUERYSET** &nbsp;format &nbsp;➜&nbsp; <code>Serializer</code> &nbsp;➜&nbsp; **JSON** &nbsp;format
