
ORM &nbsp;➜&nbsp; <code>Person.objects.all()</code> &nbsp;➜&nbsp; [1, 2, 3, 4] &nbsp;➜&nbsp; <code>QuerySet</code><br>
whcih cannot be exposed to the frontend.
We need to expose the data only in the JSON format. To achive this, we need a <code>serializer</code>

<br>

<pre>
    ORM  ---->  <code>Person.objects.all()</code>  ---->  [1,2,3,4]  ---->  QuerySet    
                                                                                ↓
</pre>

