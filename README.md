# Embedded_Software_Meta_Model

This repo defines a meta-model allowing to model an embedded software.  
The meta-model is framework and language independent.  
It is a set of meta-classes, relations and rules dedicated to the modeling of an
embedded software.

It covers the the physical software architecture and the software detailed
design.

## Meta-classes list

* [Software_Element](#software_element)
* [Package](#package)
* [Data_Type](#data_type)
* [Basic_Type](#basic_type)
* [Enumerated_Type](#enumerated_type)
* [Physical_Type](#physical_type)
* [Array_Type](#array_type)
* [Record_Type](#record_type)

## Software_Element

Many meta-classes of this meta-model derive from Software_Element.  
It is an abstract meta-class that defines the common attributes shared by all
the meta-classes that inherit from it.

### Description

![Software_Element](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.github.com/HomeMadeBots/Embedded_Software_Meta_Model/master/Diagrams/Software_Element_Dgm.puml)

### Attributes

| Attributes | Type | Description |
| :-- | :-- | :-- |
| _Name_ | string of charaters | Name of the **Software_Element**, it shall refer to its role. |
| _Description_ | string of charaters | Description of the **Software_Element**. |

### Rules

#### ELMT_1 (Error)
The _Name_ of any **Software_Element** shall be fulfilled.  
_It can be used to create the symbol of the corresponding implementation
element._

#### ELMT_2 (Error)
The _Name_ of a **Software_Element** is a string of characters. Allowed
characters are a to z, A to Z, 0 to 9 and underscore. It shall start with a
letter.  
_To comply with most of programming language allowed symbols._

#### ELMT_3 (Warning)
The _Description_ of a **Software_Element** shall be fulfilled.  
_To ensure high level of understanding. Allows to easily maintain or reuse the
elements._

#### ELMT_4 (Error)
The aggregates of a given **Software_Element** shall have a different _Name_.  
_To ensure model transformation (including implementation).
Some meta-models do not allow that two elements having the same parent have the
same identifier (e.g. : AUTOSAR)._

## Package

### Description

![Package](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.github.com/HomeMadeBots/Embedded_Software_Meta_Model/master/Diagrams/Package_Dgm.puml)

### Rules

#### PROJ_1 (Warning)
**Packages** aggregated within a project should not be involved in dependency
cycles.  
_Cyclic dependencies lead to hardly maintainable software._

#### PKG_1 (Warning)
A **Package** shall contain at least one **Software_Element**.  
_An empty package is useless._

## Data_Type

### Description

![Data_Type](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.github.com/HomeMadeBots/Embedded_Software_Meta_Model/master/Diagrams/Data_Type_Dgm.puml)

## Basic_Type

### Description

![Basic_Type](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.github.com/HomeMadeBots/Embedded_Software_Meta_Model/master/Diagrams/Basic_Type_Dgm.puml)

The meta-model defines some **Basic_Types**. They are types that can be natively
handled by a micro-controller.
* **Basic_Boolean_Type**
  * **boolean** : boolean.
* **Basic_Integer_Type**
  * **uint8** : unsigned integer on 8 bits.
  * **uint16** : unsigned integer on 16 bits.
  * **uint32** : unsigned integer on 32 bits.
  * **uint64** : unsigned integer on 64 bits.
  * **sint8** : signed integer on 8 bits.
  * **sint16** : signed integer on 16 bits.
  * **sint32** : signed integer on 32 bits.
  * **sint64** : signed integer on 64 bits.
* **Basic_Floating_Point_Type**
  * **float32** : decimal encoded on 32 bits.
  * **float64** : decimal encoded on 64 bits.
* **Basic_Characters_Type**
  * **character** : single character.
  * **characters_string** : string of characters.
* **Basic_Array_Type**
  * **uint8_array** : array of uint8 (unknown size).

## Enumerated_Type

### Description

![Enumerated_Type](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.github.com/HomeMadeBots/Embedded_Software_Meta_Model/master/Diagrams/Enumerated_Type_Dgm.puml)

### Rules

#### ENUM_1 (Error)
An **Enumerated_Type** shall aggregate at least one **Enumeral**.  
_To ensure model implementation, many programming language do not allow an
enumerated data type without enumeral._

#### ENUM_2 (Warning)
An **Enumerated_Type** should aggregate at least two **Enumerals**.  
_An enumerated data type which can take only value one is useless._

#### ENUM_3 (Error)
The _Name_ of an **Enumeral** is a string of characters. Allowed characters are
a to z, A to Z, 0 to 9 and underscore. It shall begin with a letter.   
_To ensure model implementation, to comply with most of programming language
allowed symbols._

#### ENUM_4 (Warning)
The _Description_ of an **Enumeral** shall be fulfilled.  
_To ensure high level of understanding. Allows to easily maintain or reuse the
type._

## Physical_Type

### Description

![Physical_Type](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.github.com/HomeMadeBots/Embedded_Software_Meta_Model/master/Diagrams/Physical_Type_Dgm.puml)

### Rules

#### PHY_1 (Error)
The _Resolution_ of a **Physical_Type** shall be set to a non-null decimal
value.  
_The _Resolution_ shall be consistent with a physical value.  
A _Resolution_ equal to 0 would mean that the type allows to model a constant
equal to the _Offset__.


#### PHY_2 (Warning)
The _Unit_ of a **Physical_Type** shall be set.  
_To ensure high level of understanding. Allows to easily maintain or reuse the
type._

#### PHY_3 (Error)
The _Base_Type_Ref_ of a **Physical_Type** shall be a **Basic_Integer_Type**.  
_A physical data type shall rely on a native integer data type from the
micro-contoller to be able to apply the conversion rule._

#### PHY_4 (Error)
The _Offset_ of a **Physical_Type** shall be set to a decimal value.  
_The _Offset_ shall be consistent with a physical value._

## Array_Type

### Description

![Array_Type](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.github.com/HomeMadeBots/Embedded_Software_Meta_Model/master/Diagrams/Array_Type_Dgm.puml)

### Rules

#### ARR_1 (Error)
The _Multiplicity_ of an **Array_Type** shall be a strictly positive integer
value.  
_Nonsense._

#### ARR_2 (Warning)
The _Multiplicity_ of an **Array_Type** should be greater than 1.  
_An array of one element is not really an array, just a kind of "typedef" to
redefine an existing type._

#### ARR_3 (Error)
The association with a **Data_Type** (through _Base_Type_Ref_) is mandatory.  
_Obvious._


#### ARR_4 (Error)
The _Base_Type_Ref_ of an **Array_Type** shall not be itself.  
_Nonsense._

## Record_Type

### Description

![Record_Type](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.github.com/HomeMadeBots/Embedded_Software_Meta_Model/master/Diagrams/Record_Type_Dgm.puml)

### Rules

#### REC_1 (Error)
A **Record_Type** shall aggregate at least one **Field**.  
_To ensure model implementation, many programming language do not allow a
structured data type without field._

#### REC_2 (Warning)
A **Record_Type** should aggregate at least two **Fields**.  
_A structure with only one field is useless._

#### REC_3 (Error)
A **Field** shall not reference its owner.  
_To ensure model implementation, some programming language do not allow
auto-reference._