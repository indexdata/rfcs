
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

#### __Guide to what constitutes as breaking changes.__

- Module Version changes(versioned independent of the interface):
    - What is a __breaking__ change with respect to a module version change ?
        -  In UI modules :
            - Okapi interface version change - both __new__ minimum version and __new__ interface version changes are breaking changes
            - Addition of a new peer dependency is a breaking change
            - Version change of an existing peer dependency or dependencies is a breaking change
            - Change in public routes is also a breaking change
        - In Backend modules :
            - Changes to both required and optional interface dependencies - eg. dropping support for an interface version, i.e., replacing __version 3.2__ with __version 3.3__ is a breaking change
            - Removing __any__ interface is considered a breaking change.
            - Changing the __minor__ version of an interface is also considered as a breaking change.
            - __New operational__ requirement (e.g. requiring postgres or kafka) is a breaking change (mod-authtoken used to be self contained but now requires DB storage)
            - With respect to Okapi version change a __new__ minimum version change is considered a breaking change as well as a new interface addition is also considered a breaking change
        - It is also considered as a breaking change when there is a __major__ change within an interface
        - If a module __stops__ providing an interface, is also considered as a breaking change
        - Runtime environment changes are also breaking changes.
    - Things to consider while dealing with Module version changes:
        - The language here needs to be fleshed out with examples and reasoning, i.e. there is both a functional API (the provided interfaces) and an operational API (the runtime requirements) and changes to either of these must be reflected in a module’s semver. NB: make a general statement about the goal here, and then provide select examples). ()
        - Three kinds of breaking changes?
            - Infrastructural change : eg. modules requires Kafka, Postgres, etc.
            - Non-infrastructural changes: eg. modules need Java v17, Node v16, etc.
            - Configurational changes: eg. change to environmental variables.

- Interface Version changes(Interface versions mostly describe the communication protocol, behavioral changes are mostly captured at the module version leve):
    - What is a __breaking__ change with respect to a interface version change ?
        - Changes in the API
            - Removing and endpoint
            - Changing the response content type
            - Adding or removing possible HTTP status code
        - Changes to the Data model
            - Removing a required field

#### __Guide to what constitutes as non-breaking changes.__

- Module Version Change :
    - What is a __non-breaking__ change with respect to a module version change ?
        - In UI Modules :
            - Addition of a __new direct__ or __development dependency__
            - Changing __any__ version of an __existing__ direct or development __dependency__
            - Addition of a __new__ version of an OKAPI interface which is already depended upon
        - In Backend Modules :
            - Changes to both required and optional interface dependencies is considered non breaking in following cases :
                - Adding a trailing __'0'__ to an interface version, e.g. changing __'3.2.4'__ to __'3.2.4.0'__
            - Bumping the __minor__ version of a provided interface is not a breaking change
            - Adding any new interfaces is also not a breaking change
            - Implementation changes that neither affect interfaces nor operational requirements are considered as non breaking changes, e.g. __adding new code optimisations__
            - The introduction of new functionality that in no way affects existing functionality or introduces additional operational requirements, e.g. __introduction of a new interface__
- Interface Version Change :
    - What is a __non-breaking__ change with respect to a interface version change ?
        - Non breaking changes in the __API__
            - Adding an __endpoint__ to an API is not a breaking change
        - Non breaking changes to the __Data Model__
            - Adding a __new__ optional field is not a breaking change
            - __Removing__ an optional field when additional properties are allowed is not a breaking change.

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
