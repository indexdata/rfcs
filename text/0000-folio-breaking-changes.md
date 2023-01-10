
- Start Date: 2022-10-21
- RFC PR: 
- FOLIO Issue: 


# Folio Breaking Changes

## Summary
This RFC's purpose is to deliver to developers and other effected groups a guide of changes within Folio either as a breaking change or otherwise.

## Motivation

- To help different Folio implementors know with every release what significant changes have been added.
- An extensive guide of those significant changes grouping them into breaking and non-breaking changes.

## Detailed Explanation/Design

### Specifics

The RFC aims to classify the changes made at the folio module and interface level as breaking changes and non-breaking changes. To elaborate, how or what change in a module or a part of interface can cause componenet failures and how these changes should be communicated to the implementors, developers, etc.



#### __What constitutes as a breaking change ?__

Throughout this RFC we will be discussing about changes in terms of the following three categories

- What is a breaking change at __module__ level?
- What is a breaking change at the __interface__ level?
- What constitutes as a breaking change in the __behavioral__ level? 

- Three kinds of breaking changes have been defined :
 |         Changes                 |                Examples                 |
 | ------------------------------- | --------------------------------------- |
 | __Infrastructural change__      | A module requires Kafka, Postgres, etc. |
 | __Non-infrastructural changes__ | Modules need Java v17, Node v16, etc.   |
 | __Configurational changes__     | Change to environmental variables, etc. |
  
 
#### __Guide to what constitutes as breaking changes.__

- Module Version changes(versioned independent of the interface):
    - What is a __breaking__ change with respect to a module version change ?
        -  In UI modules :
            |          Use Cases                      | Classification (Breaking/Non-breaking) |
            | --------------------------------------- | -------------------------------------  |
            | __New__ minimum version change Okapi    | __Breaking Change__                    |
            | __New__ interface version change Okapi  | __Breaking Change__                    |
            | __New__ peer dependency                 | __Breaking Change__                    |
            | __Version Change__ peer dependency      | __Breaking Change__                    |
            | __Public Route__ Changes                | __Breaking Change__                    |
            
        - In Backend modules :
            |          Use Cases                           | Classification (Breaking/Non-breaking) |
            | -------------------------------------------- | -------------------------------------  |
            | __Dropping__ support for interface version   | __Breaking Change__                    |
            | Removing __any__ interface                   | __Breaking Change__                    |
            | Changing __minor__ version of an interface   | __Breaking Change__                    |
            | __New operational__ requirement              | __Breaking Change__                    |
            | __New__ interface addition w.r.t. Okapi      | __Breaking Change__                    |
            |  __Major__ change within an interface        | __Breaking Change__                    |
            | Module __stops__ providing an interface      | __Breaking Change__                    |
            | __Runtime__ environment changes              | __Breaking Change__                    |

    - Things to consider while dealing with Module version changes:
        - The language here needs to be fleshed out with examples and reasoning, i.e. there is both a functional API (the provided interfaces) and an operational API (the runtime requirements) and changes to either of these must be reflected in a module’s semver. NB: make a general statement about the goal here, and then provide select examples). ()
        
- Interface Version changes(Interface versions mostly describe the communication protocol, behavioral changes are mostly captured at the module version leve):
    - What is a __breaking__ change with respect to a interface version change ?
        - Changes in the API :
        |                  Use Case                    | Classification (Breaking/Non-breaking)  |
        | -------------------------------------------- | --------------------------------------- |
        | __Removing__ an endpoint                     | __Breaking Change__                     |
        | __Changing__ the response content type       | __Breaking Change__                     |
        | __Adding__ or __removing__ HTTP status code  | __Breaking Change__                     |

        - Changes to the Data model:
        |                  Use Case                    | Classification (Breaking/Non-breaking)  |
        | -------------------------------------------- | --------------------------------------- |
        | __Removing__ a required field                | __Breaking Change__                     |
           

#### __Guide to what constitutes as non-breaking changes.__

- Module Version Change :
    - What is a __non-breaking__ change with respect to a module version change ?
        - In UI Modules :
        |                      Use Case                           | Classification (Breaking/Non-breaking)  |
        | ------------------------------------------------------- | --------------------------------------- |
        | Addition of a __new direct__ dependency                 | __Non-Breaking Change__                 |
        | Addition of a __new development__ dependency            | __Non-Breaking Change__                 |
        | Changing version of an __existing__ direct dependency   | __Non-Breaking Change__                 |
        | Changing version of __existing__ development dependency | __Non-Breaking Change__                 |
        | Addition of __new__ version of existing OKAPI interface | __Non-Breaking Change__                 |

        - In Backend Modules :
        |                      Use Case                     | Classification (Breaking/Non-breaking)  |
        | --------------------------------------------------| --------------------------------------- |
        | Adding a trailing __'0'__ to an interface version | __Non-Breaking Change__                 |
        | Bumping the __minor__ version of an interface     | __Non-Breaking Change__                 |
        | Addition of __new__ interface                     | __Non-Breaking Change__                 |
        | __Adding__ new code optimisations                 | __Non-Breaking Change__                 |
        | __Adding__ new functionalities                    | __Non-Breaking Change__                 |

- Interface Version Change :
    - What is a __non-breaking__ change with respect to a interface version change ?
        - Non breaking changes in the __API__ :
        |               Use Case            | Classification (Breaking/Non-breaking)  |
        | ----------------------------------| --------------------------------------- |
        | Adding a new  __endpoint__        | __Non-Breaking Change__                 |
        
        - Non breaking changes to the __Data Model__ :
        |               Use Case            | Classification (Breaking/Non-breaking)  |
        | ----------------------------------| --------------------------------------- |
        | Adding a __new__ optional field   | __Non-Breaking Change__                 |
        | __Removing__ an optional field    | __Non-Breaking Change__                 |

### Clarifications

- Interfaces only represent the communication protocol, i.e. the shape of a request/response to/from a given endpoint. Behavior changes are part of a module’s version, e.g. if circulation adds additional actions when receiving a request at /checkout such as sending notifications or checking fees/fines but continues to send the same response, the interface version will not change. 


## Risks and Drawbacks

#### __Risk Scenarios__ 

A detailed RFC and ADR is made with guide to recognise and communicate probable breaking changes to the effected developers, etc, but  after all these efforts, the guide is used by no-one. 

## Rationale and Alternatives

## Unresolved Questions

- How to provide the guide with respect to the Behavioral changes ?
- What kind of change is dropping a minor/patch version of a runtime dependency? (Note that a dev doesn’t get to set the java version of the platform because container config happens independent of module development)
- How to provide a clear guidance as to how the proposal should be implemented?
