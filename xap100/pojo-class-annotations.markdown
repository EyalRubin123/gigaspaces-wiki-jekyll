---
layout: post100
title:  Class Metadata
categories: XAP100
parent: modeling-your-data.html
weight: 110
---

{% summary %}{% endsummary %}



The [GigaSpaces API](./the-gigaspace-interface-overview.html) supports class and field-level decorations with POJOs. These can be specified via annotations on the space class source itself or external xml file accompanied with the class byte code files located within the jar/war. You can define common behavior for all class instances, and specific behavior for class fields.


{%wbr%}

# Persistence

{: .table .table-bordered}
|Syntax     | @SpaceClass(persist=true) |
|Argument   | boolean          |
|Default    | false|
|Description| When a space is defined as persistent, a 'true' value for this annotation persists objects of this type. |


{% togglecloak id=1 %}**Example**{% endtogglecloak %}
{% gcloak 1 %}
{% inittab os_simple_space|top %}
{% tabcontent Annotation %}
{%highlight java%}

@SpaceClass(persist=true)
public class Person {
//
}
{%endhighlight%}
{% endtabcontent %}
{% tabcontent gs.xml %}
{%highlight xml%}
 <class name="examples.model.Person"
     persist="true">
 </class>
}
{%endhighlight%}
{% endtabcontent %}
{% endinittab %}
{% endgcloak %}
{%learn%}./space-persistency.html{%endlearn%}


# Include Properties

{: .table .table-bordered}
|Syntax     | @SpaceClass(includeProperties=IncludeProperties.EXPLICIT) |
|Argument   | [IncludeProperties](http://www.gigaspaces.com/docs/JavaDoc{%currentversion%}/com/gigaspaces/annotation/pojo/SpaceClass.IncludeProperties.html)      |
|Default    | IncludeProperties.IMPLICIT|
|Description| `IncludeProperties.IMPLICIT` takes into account all POJO fields -- even if a `get` method is not declared with a `@SpaceProperty` annotation, it is taken into account as a space field.`IncludeProperties.EXPLICIT` takes into account only the `get` methods which are declared with a `@SpaceProperty` annotation. |

{% togglecloak id=2 %}**Example**{% endtogglecloak %}
{% gcloak 2 %}
{%highlight java%}
@SpaceClass(includeProperties=IncludeProperties.EXPLICIT)
public class Person {
  //
}
{%endhighlight%}
{% endgcloak %}

# FIFO Support

{: .table .table-bordered}
|Syntax     | @SpaceClass(fifoSupport=FifoSupport.OPERATION) |
|Argument   | [FifoSupport]({% javadoc com/gigaspaces/annotation/pojo/FifoSupport %})|
|Default    | FifoSupport.NOT_SET|
|Description| To enable FIFO operations, set this attribute to `FifoSupport.OPERATION`|


{% togglecloak id=3 %}**Example**{% endtogglecloak %}
{% gcloak 3 %}
{%highlight java%}
@SpaceClass(fifoSupport=FifoSupport.OPERATION)
public class Person {

  //
}
{%endhighlight%}
{% endgcloak %}
 {%learn%}./fifo-support.html{%endlearn%}


# Inherit Index

{: .table .table-bordered}
|Syntax     | @SpaceClass(inheritIndexes=false) |
|Argument   | boolean          |
|Default    | true|
|Description| Whether to use the class indexes list only, or to also include the superclass' indexes. {% wbr %}If the class does not define indexes, superclass indexes are used. {% wbr %}Options:{% wbr %}- `false` -- class indexes only.{% wbr %}- `true` -- class indexes and superclass indexes.|



{% togglecloak id=4 %}**Example**{% endtogglecloak %}
{% gcloak 4 %}
{%highlight java%}
@SpaceClass(inheritIndexes=false)
public class Person {

  //
}
{%endhighlight%}
{% endgcloak %}
{%learn%}./indexing.html{%endlearn%}

# Storage Type

{: .table .table-bordered}
|Syntax     | @SpaceClass(storageType=StorageType.BINARY) |
|Argument   | [StorageType]({% javadoc com/gigaspaces/metadata/StorageType %})          |
|Default    | StorageType.OBJECT |
|Description| To determine a default storage type for each non primitive property for which a (field level) storage type was not defined.|


{% togglecloak id=5 %}**Example**{% endtogglecloak %}
{% gcloak 5 %}
{%highlight java%}
@SpaceClass(storageType=StorageType.BINARY)
public class Person {

  //
}
{%endhighlight%}
{% endgcloak %}
{%learn%}./storage-types---controlling-serialization.html{%endlearn%}

# Replication

{: .table .table-bordered}
|Syntax     | @SpaceClass(replicate=false) |
|Argument   | boolean          |
|Default    | true|
|Description| When running in a partial replication mode, a **`false`** value for this property will not replicates all objects from this class type to the replica space or backup space.} |

{% togglecloak id=6 %}**Example**{% endtogglecloak %}
{% gcloak 6 %}
{% inittab os_simple_space|top %}
{% tabcontent Annotation %}
{%highlight java%}
@SpaceClass(replicate=false)
public class Person {

  //
}
{%endhighlight%}
{% endtabcontent  %}
{% tabcontent gs.xml %}
{%highlight xml%}
<class name="com.model.Person"
         replicate="false">
</class>
{%endhighlight%}
{% endtabcontent  %}
{%endinittab%}
{% endgcloak %}


# Compound Index

{: .table .table-bordered}
|Syntax     | @CompoundSpaceIndexes( {%wbr%} {@CompoundSpaceIndex(paths = {"data1", "data2"}) }  {%wbr%}) |
|Argument(s)| string          |
|Values     | attribute name(s)   |
|Description| Indexes can be defined for multiple attributes of a class  |


{% togglecloak id=7 %}**Example**{% endtogglecloak %}
{% gcloak 7 %}
{%highlight java%}
@CompoundSpaceIndexes({ @CompoundSpaceIndex(paths = { "name", "creditLimit" }) })
@SpaceClass
public class User {
     private Long id;
     private String name;
     private Double balance;
     private Double creditLimit;
     private EAccountStatus status;
     private Address address;
     private String[] comment;
}

{%endhighlight%}
{% endgcloak %}
{%learn%}./indexing-compound.html{%endlearn%}