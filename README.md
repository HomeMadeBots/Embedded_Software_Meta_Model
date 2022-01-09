# Embedded_Software_Meta_Model

This repo defines a meta-model allowing to model an embedded software loaded on
a single core microcontroller.  
The meta-model is framework and language independent.  
It is a set of meta-classes, relations and rules dedicated to the modeling of an
embedded software.

It covers the physical software architecture and the software detailed design.
Functionnal architecture is not covered.

The firsts elements defined on this page are abstract meta-classes. They allow
to define basic concepts by grouping common properties of elements defined
latter which are specializations from these abstract elements.

Note that the rules are inherited. If an element specializes an other element,
the rules defined for the based element aplly to the specialized element.

## Meta-classes list

* [Software_Element](#software_element)
* [Type](#type)
* [Variable](#variable)
* [Parameter](#parameter)
* [Package](#package)
* [Component_Type](#component_type)
* [Interface](#interface)
* [Client_Server_Interface](#client_server_interface)
* [Event_Interface](#event_interface)
* [Ports](#ports)
* [OS_Operation](#os_operation)
* [Configuration](#configuration)
* [Basic_Type](#basic_type)
* [Enumerated_Type](#enumerated_type)
* [Physical_Type](#physical_type)
* [Array_Type](#array_type)
* [Record_Type](#record_type)

## Software_Element

### Description

Many meta-classes of this meta-model derive from **Software_Element**.  
It is an abstract meta-class that defines the common attributes shared by all
the meta-classes that inherit from it.

### Definition

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

## Type

### Definition

![Type](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.github.com/HomeMadeBots/Embedded_Software_Meta_Model/master/Diagrams/Type_Dgm.puml)

The abstract meta-class **Type** is specialized into several sub meta-classes.

![Type specializations](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.github.com/HomeMadeBots/Embedded_Software_Meta_Model/master/Diagrams/Type_Specializations_Dgm.puml)

See following chapters for details.

## Variable

![Variable](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.github.com/HomeMadeBots/Embedded_Software_Meta_Model/master/Diagrams/Variable_Dgm.puml)

### Rules

#### VAR_1 (Error)
A **Variable** shall reference one and only one **Type**.  
_By definition._

## Parameter

### Definition

![Parameter](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.github.com/HomeMadeBots/Embedded_Software_Meta_Model/master/Diagrams/Parameter_Dgm.puml)

### Attributes

| Attributes | Type | Description |
| :-- | :-- | :-- |
| _Stream_ | {INPUT, OUTPUT} | Stream of a parameter, either provided to, either result of a subroutine. |

### Remarks

* The stream of a parameter is either **INPUT** or **OUPUT**.
It does mean that "IN/OUT" parameters can not be modeled. This is because from
the (abtract) point of view of the physical software architecture, a data is
either necessary to perform a subroutine, either a result of a subroutine.
"IN/OUT" concept is programming language specific.

## Package

### Description

An embedded software is the aggregation of many software elements.  
To enable the reuse of these software elements it is suitable to group them in a
consistent set.

A package allows to group software elements.

### Definition

![Package](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.github.com/HomeMadeBots/Embedded_Software_Meta_Model/master/Diagrams/Package_Dgm.puml)

All the kinds of elements which can be aggregated by a **Package** are not
modeled on this diagram.

### Rules

#### PROJ_1 (Warning)
**Packages** aggregated within a project should not be involved in dependency
cycles.  
_Cyclic dependencies lead to hardly maintainable software._

#### PKG_1 (Warning)
A **Package** shall contain at least one **Software_Element**.  
_An empty package is useless._

### Remarks

* A package is the atomic reusable item of a software model.

* It should gather cohesive software elements.

## Component_Type

### Description

A software component is a part of a software which implements one or several
software functions.

The software architect shall decompose the embedded software in several software
components and allocate each software function to one and only one software
component.

To fit object oriented design concept, a software component shall be seen as two
linked elements : the software component prototype (the object) and the software
component type (the class).  
Thereby, if the same software functions are needed several times within the same
software project, it is possible to instantiate the same component type several
times.

### Definition

![Component_Type](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.github.com/HomeMadeBots/Embedded_Software_Meta_Model/master/Diagrams/Component_Type_Dgm.puml)

### Remarks

* A software component shall be as consistent as possible, i.e. the implemented
functions shall rely on the same topic.

* We distinguish 3 kinds of software component types :  
  1. The multi-instanciable software components. These components can be re-used
on several projects and a single project can use several instances of the same
component (e.g. : push button sensor).
  2. The configurable singleton software components. These components can be
re-used on several projects, but each project has only one instance of them
(e.g. : cryptographic server).
  3. The project specific singleton software components. These components are
designed especially for a given project (cannot be reused) and it has only one
instance of them (e.g. : system specific function manager).

* The software functions implemented by a component type should be "high level"
functions. It means domain or product specific functions, opposed to "basic"
software functions which are implemented by generic classes or libraries
(detailed design).

## Interface

### Description

To realize its role (its software functions) a software component may need to
use functions from the other software components of the embedded software.  
The communication between the software components shall be done following
predefined patterns. These patterns are the software interfaces.  
An interface is a formal description of the communication pattern between
software component types. An interface defines a contract.

The software architect shall define which interface shall be realized and are
used by each software component to ensure its role.

### Definition

![Interface](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.github.com/HomeMadeBots/Embedded_Software_Meta_Model/master/Diagrams/Interface_Dgm.puml)

## Client_Server_Interface

### Definition

![Client_Server_Interface](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.github.com/HomeMadeBots/Embedded_Software_Meta_Model/master/Diagrams/Client_Server_Interface_Dgm.puml)

### Rules

#### CSIF1_1 (Error)

A **Client_Server_Interface** shall provide at least one operation.  
_An empty communication patern in a nonsense._

## Event_Interface

### Definition

![Event_Interface](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.github.com/HomeMadeBots/Embedded_Software_Meta_Model/master/Diagrams/Event_Interface_Dgm.puml)

## Ports

### Description

The software components hide the implementation of the functionality they
provide and simply expose very well defined connection points, called ports.  
Through a "provider" port, a software component realizes one interface.  
Through a "requirer" port, a software component uses one interface (realized by
an other software component).

### Definition

![Ports](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.github.com/HomeMadeBots/Embedded_Software_Meta_Model/master/Diagrams/Port_Dgm.puml)

### Rules

#### PORT_1 (Error)
A **Port** shall have one and only one contract.  
_By definition._

### Remarks

* The port mechanism allows to finely managed dependencies between components :
a component can only depend on a subset of operations provided by an other one
and not on all the realized interfaces.
* It also allows to realize, or use, the same interface several times.

## OS_Operation

### Description

The software component types can provide public operations that shall be called
by the operating system of the microcontroller.

### Definition

![OS_Operation](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.github.com/HomeMadeBots/Embedded_Software_Meta_Model/master/Diagrams/OS_Operation_Dgm.puml)

### Remarks

* An **OS_Operation** does not have any argument.

## Configuration

### Description

The software component type can provide public (read-only) attributes that allow
to configure it.  
The value of these attributes shall be set at object creation (component type
instantiation) and cannot be modify later (at run time).

### Definition

![Configuration](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.github.com/HomeMadeBots/Embedded_Software_Meta_Model/master/Diagrams/Configuration_Dgm.puml)

### Rules

#### CONF_1 (Warning)

The **Type** referenced by a **Configuration** should not be a **Record_Type**.  
_Configuration shall be as simple as possible._

#### CONF_2 (Warning)

The **Type** referenced by a **Configuration** shall not be an **Array_Type** of
**Record_Type**.  
_Configuration shall be as simple as possible._

#### CONF_3 (Warning)

The **Type** referenced by a **Configurationr** shall not be an **Array_Type**
of **Array_Type**.  
_Configuration shall be as simple as possible._

#### CONF_4 (Error)

The _Default\_Value_ of a **Configuration** shall not be empty.

#### CONF_5 (Error)

The _Default\_Value_ of a **Configuration** shall be consistent with
its referenced **Type**.

### Remarks

* **Configuration** shall be use to create configurable (either
mutli-instantiable, either singleton) **Component_Types**.

## Basic_Type

### Description

An embedded software is implemented on a microcontroller.  
A basic type is a type that can be natively handled by a microcontroller.

### Definition

![Basic_Type](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.github.com/HomeMadeBots/Embedded_Software_Meta_Model/master/Diagrams/Basic_Type_Dgm.puml)

The meta-model defines some **Basic_Types** that can be used to model data :
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

### Definition

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

### Definition

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
microcontroller to be able to apply the conversion rule._

#### PHY_4 (Error)
The _Offset_ of a **Physical_Type** shall be set to a decimal value.  
_The _Offset_ shall be consistent with a physical value._

## Array_Type

### Definition

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

### Definition

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