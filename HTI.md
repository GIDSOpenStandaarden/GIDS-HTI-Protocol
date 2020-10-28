# GIDS Health Tools Interoperability 
HTI:core version 0.3

Document version: 0.3  
Date: 22-6-2020  
Author(s): roland@headease.nl  

## Goals and Rationale
The GIDS Health Tool Interoperability protocol (HTI) is inspired by the IMS - Learning Tool Interoperability (LTI) which has had a tremendous proven impact on the relation 
between learner management systems and learning tool providers. The objectives of LTI are: 
 1. To provide a mash-up style deployment model which is easy to configure by URL, key and secret. 
 1. To define a protocol that allows for SSO which preserves the learning context and roles within that context. 
 1. To make links to external applications portable by defining shared data elements.

LTI has simplified the integration of external tools into learner management systems, and a bright new landscape of tool providers has emerged. 
The key concepts the LTI being successful has been:
 1. The core the LTI standard is simple and clear.
 1. The LTI standard in its core form is easy to integrate because it makes use of existing technologies and standards.
 1. The core standard can be extended by profiles; within LTI there are profiles for reading roster information and writing results.

The HTI standard applies these concepts when it comes to defining a successful launch protocol. The key differences are:
 1. Use of JWT instead of OAuth 1.x. JWT is an IETF standard and is more advanced than OAuth 1.x.
 1. Aligned to the, in healthcare widely used, HL7 FHIR standard.
 1. The launch message contains no personal data.
 1. More restrictions on security, privacy and sharing data.
 
The HTI standard has the following goals.

#### Ease of software implementation. 
The protocol should be easy to implement. This is done by making use of a simple core specification and allowing for more complexity and additional functionality in the form of
profiles, and the use of existing standards. 
#### Ease of use and configuration. 
The protocol should be easy to configure from both the module and portals' side. In essence, an exchange of endpoint URL and public key pair should be sufficient.
#### Scalable, decentral, and point to point.
The architecture should not rely on external or central services and should be point-to-point in the sense that parties should be able to connect without relying on other 
parties and it should scale infinitely.
#### Secure
The protocol should mitigate against most common attacks by using proven technologies like JWT and JWE.
#### Privacy by Design
The protocol encourages user consent when exchanging personal information and is designed to minimize the exchange of personal information.
#### Align with existing standards
The standard makes use of existing technologies and standards, such as JWT, JWE and HL7 FHIR.


## About this document
This document makes use of the Requirement Levels as defined in [rfc2119](https://www.ietf.org/rfc/rfc2119.txt), commonly referred to as the MoSCoW method. A summary of the MoSCoW requirement can be found in the 
section  Implementation checklist. We have tried to make use of illustrations to visually enrich the concepts and relations between concepts of the standard. Examples are used 
whenever possible. Besides this document, a testsuite is made available, and reference implementations are published to github to help developers in their implementation journey. 

## Architecture
The health tool interoperability standard (HTI) connects portals with modules like e-health treatments, (serious)games of any other type of functionality. This standard defines 
the concept of launch that entails both a transition from the portal to the module and a start of a new session on the module’s side. The HTI:core standard defines the 
communication protocol. the message format and the message exchange that is required to start a module from a portal in a domain. 

<IMG 1>

### Concepts
 * Portal, the service that links to the module, that is, an application like a tool, a game, or a treatment.
 * Module provider, the service that delivers applications like tools, games, and/or treatments to the portal.
 * Message, the package exchanged between portal and module that contains the relevant launch information. 
 * Domain, the scope of the data shared in the launch.

### Domain model
The launch message consists of a task object. The task object consists of the following fields:
 * A reference to the user.
 * A reference to the definition of the task.
Schematically, the relationship between the entities is displayed below:

<IMG 2>

### Assumptions
There are a number of assumptions on which the HTI:core standard is based. These assumptions are of relevance to the implementation of the standard as they function both as 
the starting point and as guidance to the (technical) decisions made in the realization of this standard.

#### Personal data
First and foremost, the message must not contain any personal information. The message does contain a reference to the user and the task at hand, but this reference must never 
be traceable to the real user. The user MUST be identified by a persistent pseudo identifier. The identifier MUST be unique in the domain. The portal MUST use a different 
identifiers for a user in each domain. The message MUST NOT contain any other identifier like a name, email account or social identification number. The persistent pseudo 
identifier MUST be randomly chosen and MUST contain enough entropy to block brute-force attacks.

<IMG 3>

#### Self-managed identities
As a consequence of the fact that no personal data may be exchanged by the launch, the module provider MAY query the user for information it needs. This data MAY be linked to the 
persistent pseudo identifier, so the user does not have to provide the same information again. This way the user controls what information it provides to the module provider on 
module launch.

<IMG 4>
<IMG 5>
<IMG 6>

#### Domains and scope of the persistent user identifier
The user identifier **MUST** be unique in each domain, and **MUST NOT** be shared between domains. If the relations between portal and module are between the same two legal 
entities **and** if there is a clear need to link the users identities between those relations **then** the portal **MAY** use one domain for multiple relations of the same user. 
Otherwise each relation **MUST** have each own domain. 

<IMG 7>

#### User consent
The HTI:core specification specifically prohibits the exchange of personal data, however specific profiles that extend the HTI:core standard are allowed to exchange personal data. 
Thereby the HTI:core standard states that, If one of the systems in the domain transfers information from one system to another system in the domain, the user **MUST** provide 
consent. When asking consent ,the user **MUST** be informed about all of the following:
 * The source of the data.
 * What data is shared.
 * With whom it will be shared.
 * For what period the data will be shared.
 * For what purpose the data will be shared.
The user **MUST** be informed in a short and straightforward message, that **MUST** be in understandable language of maximum B1 level of the CEFR framework. Preferably the users’ 
primary language SHOULD be used.  It MUST be clear to the user what is asked and for what purpose.
Please note that the notion of a domain does not imply that consent can be shared between module providers and portals with domain level consent. 

#### Profiles
The HTI:core standard defines the core part of the standard, consisting of the module launch. The HTI:core standard MAY be extended with profiles. These profiles MAY do the following:
 * ***Extend*** the specification by adding or modifying fields to the existing data model.
 * ***Replace*** specific parts of the standard, such as the way information is exchanged between systems.
 * ***Define*** additional functions, such as the exchange of information outside the scope of the launch.
A HTI profile MUST be documented properly and be the profile MUST clear about the relationship between HTI:core and the profile. The profile MUST identify themselves with a namespaced identifier, starting with “HTI:”, for example: HTI:smart_on_fhir. The HTI core standard is identified by HTI:core. It is encouraged to create profiles as specific as possible, and create multiple profiles if necessary.

#### To summarize:
 * The launch message must not contain personal data.
 * The module system may identify the user by its own means.
 * All involved must have explicit users’ consent if personal data is disclosed to other parties.
 * The standard may be extended with profiles, the core specification is defined as HTI:core.
 
 ## Implementation guide
The HTI:core standard defines the exchange of a message from the portal to the module. This exchange consists of the following parts:
 * ① The contents of the message: the FHIR task object.
 * ② The serialization of the message into the JWT message format.
 * ③ The exchange of the message, how it is exchanged between the portal and the module.

<IMG 5>

### Semantic roles and responsibilities
As the HTI:core specification exists of three parts, these parts each have different roles and responsibilities. These are:
 * The ① FHIR task object MUST only contain information about the functional task, the definition of the task, and the people involved. 
 * The ② JWT message MUST only contain information about the sending system, the recipient system and the message itself.
 * The ③ exchange of the message MUST NOT contain any information that falls in the categories defined by ① and ②. For example, it is not allowed to refer to a treatment by encoding one in the launch URL.
The diagram below summarizes these concepts.

<IMG 6>

### ① The FHIR task object
The message consists of a FHIR Task resource. This resource is part of the FHIR STU3 standard and documented [here](https://www.hl7.org/fhir/STU3/task.html). Please note that the STU3 MUST be used, not the latest version.

#### Identifiers and references
By the FHIR standard, each object has a type (resourceType) and identifier (id). When an object is serialized, the id and type are required fields, the diagram below show an example, the resource type and identifier are displayed in the next example:

```json
{
    "resourceType": "Task",
    "id": "a5e57fd0"
}
```
A reference to an object consists of a combination of type and identifier. References to objects in the FHIR standard are notated as follows:
```
resourceType/id
```
In the example below, show an example of such a reference as `ActivityDefinition/a5e58200`:
```json
{
    "resourceType": "Task",
    "id": "a5e57fd0",
    "definitionReference": {
      "reference": "ActivityDefinition/a5e58200"
    }
  }
```

<IMG 7>

<IMG 8>

<IMG 9>
