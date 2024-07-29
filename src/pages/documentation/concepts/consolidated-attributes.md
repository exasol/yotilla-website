---
layout: base
sidebar: true
meta:
  title: "Consolidated Attributes"
eleventyNavigation:
  key: "Consolidated Attributes"
  parent: Concepts
  order: 1
---

# Consolidated Attributes

***
<p class="lead">
Business objects and attributes are mapped to the sources in the conceptual model.
Attributes whose data comes from multiple sources are automatically identified and consolidated if they have the sama business name.
These attributes are called <strong>Consolidated Attributes</strong>.
</p>

***

Yotilla only allows one source column in each source to be mapped to a business attribute of a business object. You can however map source columns in different sources to the same business attribute name. This will create a Consolidated Attribute. When a consolidated attribute is created, Yotilla will render a formula that consolidates the values of all the participating mapped source attributes.

The order of the participating source attributes in the consolidated attribute determine the priority. If the first read source attribute has a null value, the value of the next attribute in the order will be used. The default order of the participating source attributes is the order in which they were defined. The order can be changed on the Source page, on the Catalog page, and using TinML.


<div class="callout callout-info" markdown='1'>

#### Limitations

* The data types of the mapped source columns of a consolidated attribute must be compatible. If the data types are not compatible, the consolidated attribute and its containing business object will be invalid. For more information, see Data Type Compatibility.
* Columns from Custom Views cannot participate in a consolidated attribute. Consolidated attributes may however be used in the `USES` clauses of a custom view.
* A consolidated attribute cannot also be a business key. If you define a consolidated attribute as a business key, the consolidation will be removed and the attribute will be automatically renamed.

See also `DEFINE CONSOLIDATION` in the TinML reference documentation.
</div>

***

## Create a Consolidated Attribute
Yotilla will automatically create a consolidated attribute if a source attribute with a name that matches an existing source attribute is detected during a source metadata scan.
You can also manually create a consolidated attribute by renaming an existing source attribute so that it matches another source attribute within the business object.

In the following example, the sources `ARTICLE` and `PRODUCT` both have a source column named `PRODUCT_NAME`.
Both columns have been mapped to the attribute Article Name in the business object Product.
Yotilla has therefore defined this as a consolidated attribute.

Source columns of source `ARTICLE`:
<img src="{{ '/assets/images/documentation/ca-example-1a.png' | url}}" class="img-fluid" alt="Source columns of source ARTICLE" />

Source columns of source `PRODUCT`:
<img src="{{ '/assets/images/documentation/ca-example-2.png' | url}}" class="img-fluid" alt="Source columns of source PRODUCT" />

Both sources also have a source column named `NET_PRICE`. These columns have been mapped to different attribute names and are therefore not consolidated.

To view and change the order of the participating source attributes in the consolidated attribute, click on the icon.
In the dialog window that opens, click and drag the attributes to change the order.

<img src="{{ '/assets/images/documentation/ca-change-order.png' | url}}" class="img-fluid" alt="Change the priority of the consolidation with drag&drop" />

***

## Remove a Consolidated Attribute

To remove (split) a consolidated attribute, rename the participating source attributes so that they have different attribute names.
Yotilla will automatically remove the consolidation.

In the following example, the attribute Article Name has been renamed to Product Name for the second source.

<img src="{{ '/assets/images/documentation/ca-example-1b.png' | url}}" class="img-fluid" alt="Split the consolidated attribute 'Product Name' by renaming it to 'Article Name'" />

The attributes are therefore no longer consolidated.

<img src="{{ '/assets/images/documentation/ca-example-2b.png' | url}}" class="img-fluid" alt="Source columns of source PRODUCT" />

***

## Data Type Compatibility

The base technical data type of all source columns that are part of a consolidated attribute must be compatible, otherwise the consolidated attribute and the containing business object will be invalid.

The precision and scale attributes in the data type do not have to match, but the resulting precision must be within the limits allowed by the target database.

For example:

| Source column 1 data type	 | Source column 2 data type	 | Resulting data type	 | Compatible | Note                              |
|----------------------------|----------------------------|----------------------|------------|-----------------------------------|
| `varchar(100)`             | `varchar(200)`               | 	`varchar(200)`      | `yes`      |                                   |
| `int`	                       | `varchar(200)`	|                      |            | Incompatible data types           |
| `decimal(10,0)` | `decimal(8,2)` |	`decimal(12,2)`|            |                                   |
| `decimal(36,0)` | `decimal(32,4)` |	`decimal(40,4)`|            | Resulting precision is too large  |


For more information, see Data Types.
