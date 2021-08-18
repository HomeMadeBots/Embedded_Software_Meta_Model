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

### Descriptions

![Package](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.github.com/HomeMadeBots/Embedded_Software_Meta_Model/master/Diagrams/Package_Dgm.puml)

### Rules

| ID | Description | Rationale | Criticality |
| :-: | :-- | :-- | :-: |
| PROJ_1 | **Packages** aggregated within a project should not be involved in dependency cycles. | Cyclic dependencies lead to hardly maintainable software. | Warning |
| PKG_1 | A **Package** shall contain at least one **Software_Element**. | An empty package is useless. | Warning |
