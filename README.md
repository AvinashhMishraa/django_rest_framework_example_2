
ORM &nbsp;&nbsp;➜&nbsp;&nbsp; <code>Person.objects.all()</code> &nbsp;&nbsp;➜&nbsp;&nbsp; [1, 2, 3, 4]  &nbsp;&nbsp;➜&nbsp;&nbsp; it's datatype is <code>QuerySet</code> which cannot be exposed to the frontend. <br>
We need to expose the data only in the JSON format. To achive this, we need a <code>serializer</code>

<br>

<pre>
    ORM  ---->  <code>Person.objects.all()</code>
  
</pre>
