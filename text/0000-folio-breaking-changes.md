- Start Date: 2022-10-21
- RFC PR: https://github.com/folio-org/rfcs/pull/5
- FOLIO Issue: N/a


# Folio Breaking Changes

## Summary
This RFC's purpose is to deliver to developers and other affected groups general guidelines for determining if a change within FOLIO is either breaking or non-breaking. 

## Motivation

Help FOLIO developers and other affected groups know, with every release, what breaking changes have been made.

## Detailed Explanation/Design

### Terms

For the purposes of this RFC the following terms should be understood by these definitions.

- **Behavioral Change**: A functional change to the implementation which does not correspond to a change in the interface.

- **Breaking Change**: A change that requires a Major Version Change as per Semantic Versioning's requirements.

    - *See [Semantic Versioning 2.0.0](https://semver.org/#semantic-versioning-200)*

- **Communication Protocol**: 

- **Data Model Change**: A change in implementation that will require the addition, modification or removal of tables, columns or JSON structures in the storage layer.

- **Direct Dependency (UI)**: A package or module that is explicitly required by a particular project in order to function at runtime.

- **Development Dependency (UI)**: A package or module that is explicitly required by a particular project in order to be built.

- **Interface**: An API described by RAML or OpenAPI documentation that has at least one implementation in FOLIO.

- **Implementation**: Code which is invoked when interacting with an Interface.

- **Operational Change**: A change in an implementation that affects system operators. Such as...

    |         Changes                 |                Examples                              |
    | ------------------------------- | ---------------------------------------------------- |
    | __Infrastructural changes__     | An implementation requires Kafka, Postgres, etc.     |
    | __Non-infrastructural changes__ | An implementations requires Java v17, Node v16, etc. |
    | __Configurational changes__     | A change to environmental variables, etc.            |

### Specifics

This RFC defines changes made in FOLIO at the implementation or interface level as breaking and non-breaking. Not all possible changes can be enumerated here, so this document is not intended to provide an exhaustive list of changes. If a particular change is not covered by this document, developers are encouraged to seek the guidance of the TC. Because both implementations and interfaces are versioned independently of each other, this document will address breaking and non-breaking changes at both the implementation and interface levels.

Additional distinctions must be made for implementation and interface changes. For implementation changes we will differentiate between backend and front end. For interface changes we will differentiate between the communication protocol and the data model. 

Behavioral changes, though significant and worth addressing, are outside the scope of this RFC.

#### __What constitutes as a breaking change ?__
Throughout this RFC we will be discussing changes in terms of the following three categories

- What is a breaking change at the __implementation__ level?
- What is a breaking change at the __interface__ level?

* We will not address what constitutes as a breaking change in the __behavioral__ level, though this will be an important topic to broach in the future.

#### __Implementation Version changes(versioned independent of the interface)__

-  In UI Implementations:

    |          Use Cases                                      | __Breaking Change__  | __Non-Breaking__   |
    | --------------------------------------------------------| -------------------- | ------------------ |
    | __New__ minimum version change of an Okapi interface    |  <center>X</center>  |                    |
    | __New__ interface version change of an Okapi interface  |  <center>X</center>  |                    |
    | __New__ peer dependency addition                        |  <center>X</center>  |                    |
    | __Version Change__ of a peer dependency                 |  <center>X</center>  |                    |
    | __Public Route__ removal                                |  <center>X</center>  |                    |
    | __Public Route__ addition                               |                      | <center>X</center> |
    | Addition of a __new direct__ dependency                 |                      | <center>X</center> |
    | Addition of a __new development__ dependency            |                      | <center>X</center> |
    | Changing version of an __existing__ direct dependency   |                      | <center>X</center> |
    | Changing version of __existing__ development dependency |                      | <center>X</center> |
    | Addition of __new__ version of existing OKAPI interface |                      | <center>X</center> |
    
- In Backend Implementations :
    |          Use Cases                                | __Breaking Change__  | __Non-Breaking__ |
    | --------------------------------------------------| ---------------------| ---------------  |
    | __Dropping__ support for interface version        |  <center>X</center>  |                  |
    | Removing __any__ interface                        |  <center>X</center>  |                  |
    | Changing __minor__ version of an interface        |  <center>X</center>  |                  |
    | __New operational__ requirement                   |  <center>X</center>  |                  |
    | __New__ interface addition w.r.t. Okapi           |  <center>X</center>  |                  |
    |  __Major__ change within an interface             |  <center>X</center>  |                  |
    | implementation __stops__ providing an interface   |  <center>X</center>  |                  |
    | __Runtime__ environment changes                   |  <center>X</center>  |                  |
    | Adding a trailing __'0'__ to an interface version |                      |<center>X</center>|
    | Bumping the __minor__ version of an interface     |                      |<center>X</center>|
    | Addition of __new__ interface                     |                      |<center>X</center>|
    | __Adding__ new code optimisations                 |                      |<center>X</center>|
    | __Adding__ new functionalities                    |                      |<center>X</center>|

        
#### __Interface Version changes__ 

- Changes in the communication protocal :
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

- A detailed RFC and ADR is made with guidelines to recognise and communicate probable breaking changes to the effected developers, etc, but after all these efforts, the guide is used by no-one 

## Rationale and Alternatives

__[TO DO]: Complete this section__

- Continue without a common guidence
- Semantic Versioning

## Unresolved Questions

- How to provide the guidance for versioning Behavioral changes?
- What kind of change would result from the dropping of a minor/patch version of a runtime dependency? (Note that a dev doesn’t get to set the java version of the platform because container config happens independent of implementation development)
- How to provide a clear guidance as to how the proposal should be implemented?
- Changes to public routes expressed within the UI can be considered breaking. This has not been addressed by this RFC.
- Some changes to shared components within the UI could be considered breaking. This has not been addressed by this RFC.
