- Start Date: 2022-10-21
- RFC PR: 
- FOLIO Issue: 


# Folio Breaking Changes

## Summary
This RFC's purpose is to deliver to developers and other effected groups general guidlines for determining if a changes within Folio is either a breaking or non-breaking change.

## Motivation

- To help different Folio implementors know with every release what significant changes have been added.
- An extensive guide of those significant changes grouping them into breaking and non-breaking changes.

## Detailed Explanation/Design

### Specifics

This RFC aims to outline possible changes made in FOLIO at the implementation or interface level as breaking and non-breaking changes. Because both implementations and interfaces are versioned independently of each other, this document will address breaking and non breaking changes at both the implementation and interface levels.

Additional distinctions must be made between backend vs. UI changes, for implementation versioning, and API vs Data Model changes at the interface level. Behavioral changes, though signigicant and worth addressings, are outside the scope of this RFC. 


#### __What constitutes as a breaking change ?__
Throughout this RFC we will be discussing changes in terms of the following three categories

- What is a breaking change at the __implementation__ level?
- What is a breaking change at the __interface__ level?

* We will not address what constitutes as a breaking change in the __behavioral__ level, though this will be an important topic to broach in the future.

- Types of changes which can be considered breaking are:
    |         Changes                 |                Examples                 |
    | ------------------------------- | --------------------------------------- |
    | __Infrastructural change__      | A implementation requires Kafka, Postgres, etc. |
    | __Non-infrastructural changes__ | implementations need Java v17, Node v16, etc.   |
    | __Configurational changes__     | Change to environmental variables, etc. |

#### __Implementation Version changes(versioned independent of the interface)__

-  In UI Implementations:

    |          Use Cases                      | __Breaking Change__  | __Non-Breaking__ |
    | --------------------------------------- | -------------------- | ---------------- |
    | __New__ minimum version change of an Okapi interface    |  <center>X</center>  |                  |
    | __New__ interface version change of an Okapi interface  |  <center>X</center>  |                  |
    | __New__ peer dependency addition        |  <center>X</center>  |                  |
    | __Version Change__ of a peer dependency      |  <center>X</center>  |                  |
    | __Public Route__ addition                |    | <center>X</center>                 |
    | __Public Route__ removal                |  <center>X</center>  |                  |
    | Addition of a __new direct__ dependency                 |      |<center>X</center>|
    | Addition of a __new development__ dependency            |      |<center>X</center>|
    | Changing version of an __existing__ direct dependency   |      |<center>X</center>|
    | Changing version of __existing__ development dependency |      |<center>X</center>|
    | Addition of __new__ version of existing OKAPI interface |      |<center>X</center>|
    
- In Backend Implementations :
    |          Use Cases                           | __Breaking Change__  | __Non-Breaking__ |
    | -------------------------------------------- | ---------------------| ---------------  |
    | __Dropping__ support for interface version   |  <center>X</center>  |                  |
    | Removing __any__ interface                   |  <center>X</center>  |                  |
    | Changing __minor__ version of an interface   |  <center>X</center>  |                  |
    | __New operational__ requirement              |  <center>X</center>  |                  |
    | __New__ interface addition w.r.t. Okapi      |  <center>X</center>  |                  |
    |  __Major__ change within an interface        |  <center>X</center>  |                  |
    | implementation __stops__ providing an interface      |  <center>X</center>  |                  |
    | __Runtime__ environment changes              |  <center>X</center>  |                  |
    | Adding a trailing __'0'__ to an interface version |                 |<center>X</center>|
    | Bumping the __minor__ version of an interface     |                 |<center>X</center>|
    | Addition of __new__ interface                     |                 |<center>X</center>|
    | __Adding__ new code optimisations                 |                 |<center>X</center>|
    | __Adding__ new functionalities                    |                 |<center>X</center>|

        
#### __Interface Version changes__ 

- Changes in the API :
    |                  Use Case                    | __Breaking Change__ |  __Non-Breaking__  |
    | -------------------------------------------- | ------------------- | ------------------ |
    | __Removing__ an endpoint                     | <center>X</center>  |                    |
    | __Changing__ the response content type       | <center>X</center>  |                    |
    | __Adding__ or __removing__ HTTP status code  | <center>X</center>  |                    |
    | Adding a new  __endpoint__                   |                     | <center>X</center> |

- Changes to the Data model:
    |                  Use Case                    | __Breaking Change__ |  __Non-Breaking__  |
    | -------------------------------------------- | ------------------- | ------------------ |
    | __Removing__ a required field                |  <center>X</center> |                    |
    | Adding a __new__ optional field              |                     | <center>X</center> |
    | __Removing__ an optional field               |                     | <center>X</center> |
           
### Clarifications

- Interfaces only represent the communication protocol, i.e. the shape of a request/response to/from a given endpoint. Behavior changes are part of a implementation’s version, e.g. if circulation adds additional actions when receiving a request at /checkout such as sending notifications or checking fees/fines but continues to send the same response, the interface version will not change. 


## Risks and Drawbacks

#### __Risk Scenarios__ 

A detailed RFC and ADR is made with guide to recognise and communicate probable breaking changes to the effected developers, etc, but  after all these efforts, the guide is used by no-one. 

## Rationale and Alternatives

__[TO DO]: Complete this section__

## Unresolved Questions

- How to provide the guidance for versioning Behavioral changes?
- What kind of change would result from the dropping of a minor/patch version of a runtime dependency? (Note that a dev doesn’t get to set the java version of the platform because container config happens independent of implementation development)
- How to provide a clear guidance as to how the proposal should be implemented?
