
ORM &nbsp;➜&nbsp; <code>Person.objects.all()</code> &nbsp;➜&nbsp; [1, 2, 3, 4] &nbsp;➜&nbsp; <code>QuerySet</code><br>
whcih cannot be exposed to the frontend.
We need to expose the data only in the JSON format. To achive this, we need a <code>serializer</code>

<br>

<div>
    <h1>Why &nbsp;Serializer &nbsp;?</h1>
</div>


<pre>
    ORM  ➜  <code>Person.objects.all()</code>  ➜  [1,2,3,4]  ➜  QuerySet format  ➜  which cannot be exposed to the frontend
                                                                    
</pre>

<br>

> To expose data to the frontend in only JSON format &nbsp;&nbsp;&nbsp;:&nbsp;&nbsp;&nbsp; **QUERYSET** &nbsp;format &nbsp;➜&nbsp; <code>Serializer</code> &nbsp;➜&nbsp; **JSON** &nbsp;format
