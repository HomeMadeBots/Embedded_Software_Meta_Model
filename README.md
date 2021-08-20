# Embedded_Software_Meta_Model

This repo defines a meta-model allowing to model an embedded software.  
The meta-model is framework and language independent.  
It is a set of meta-classes, relations and rules dedicated to the modeling of an
embedded software.

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

| ID | Description | Rationale | Criticality |
| :-: | :-- | :-- | :-: |
| ELMT_1 | The _Name_ of any **Software_Element** shall be fulfilled. | _Name_ can be used to create the symbol of the corresponding implementation element. | Error |
| ELMT_2 | The _Name_ of a **Software_Element** is a string of characters. Allowed characters are a to z, A to Z, 0 to 9 and underscore. It shall start with a letter. | To comply with most of programming language allowed symbols. | Error |
| ELMT_3 | The _Description_ of a **Software_Element** shall be fulfilled. | To ensure high level of understanding. Allows to easily maintain or reuse the **Software_Elements**. | Warning |
| ELMT_4 | The aggregates of a given **Software_Element** shall have a different _Name_. | To ensure model transformation (including implementation). Some meta-models do not allow that two elements having the same parent have the same identifier (e.g. : AUTOSAR). | Error |

## Package

### Description

![Package](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.github.com/HomeMadeBots/Embedded_Software_Meta_Model/master/Diagrams/Package_Dgm.puml)

### Rules

| ID | Description | Rationale | Criticality |
| :-: | :-- | :-- | :-: |
| PROJ_1 | **Packages** aggregated within a project should not be involved in dependency cycles. | Cyclic dependencies lead to hardly maintainable software. | Warning |
| PKG_1 | A **Package** shall contain at least one **Software_Element**. | An empty package is useless. | Warning |

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

| ID | Description | Rationale | Criticality |
| :-: | :-- | :-- | :-: |
| ENUM_1 | An **Enumerated_Type** shall aggregate at least one **Enumeral**. | To ensure model implementation, many programming language do not allow an enumerated data type without enumeral. | Error |
| ENUM_2 | An **Enumerated_Type** should aggregate at least two **Enumerals**. | An enumerated data type which can take only value one is useless. | Warning |
| ENUM_3 | The _Name_ of an **Enumeral** is a string of characters. Allowed characters are a to z, A to Z, 0 to 9 and underscore. It shall begin with a letter. | To ensure model implementation, to comply with most of programming language allowed symbols. | Error |
| ENUM_4 | The _Description_ of an **Enumeral** shall be fulfilled. | To ensure high level of understanding. Allows to easily maintain or reuse the **Enumerated_Type**. | Warning |

## Physical_Type

### Description

![Physical_Type](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.github.com/HomeMadeBots/Embedded_Software_Meta_Model/master/Diagrams/Physical_Type_Dgm.puml)

### Rules

| ID | Description | Rationale | Criticality |
| :-: | :-- | :-- | :-: |
| PHY_1 | The _Resolution_ of a **Physical_Type** shall be set to a non-null decimal value. | The _Resolution_ shall be consistent with a physical value. A _Resolution_ equal to 0 would mean that the type allows to model a constant equal to the _Offset_. | Error |
| PHY_2 | The _Unit_ of a **Physical_Type** shall be set. | To ensure high level of understanding. Allows to easily maintain or reuse the type. | Warning |
| PHY_3 | The _Base_Type_Ref_ of a **Physical_Type** shall be a **Basic_Integer_Type**. | A physical data type shall rely on a native integer data type from the micro-contoller to be able to apply the conversion rule. | Error |
| PHY_4 | The _Offset_ of a **Physical_Type** shall be set to a decimal value. | The _Offset_ shall be consistent with a physical value. | Error |