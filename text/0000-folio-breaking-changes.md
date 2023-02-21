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

- **Endpoint**: The combination of a path, an HTTP Method and both optional and required query parameters.

- **Interface**: An API described by RAML or OpenAPI documentation that has at least one implementation in FOLIO.

- **Implementation**: Code which is invoked when interacting with an Interface.

- **Operational Change**: A change in an implementation that affects system operators. Such as...

    |         Changes                 |                Examples                              |
    | ------------------------------- | ---------------------------------------------------- |
    | __Infrastructural changes__     | An implementation requires Kafka, Postgres, etc.     |
    | __Non-infrastructural changes__ | An implementation requires Java v17, Node v16, etc. |
    | __Configurational changes__     | A change to environmental variables, etc.            |

### Specifics

This RFC defines changes made in FOLIO at the implementation or interface level as breaking and non-breaking. Not all possible changes can be enumerated here, so this document is not intended to provide an exhaustive list of changes. If a particular change is not covered by this document, developers are encouraged to seek the guidance of the TC. Because both implementations and interfaces are versioned independently of each other, this document will address breaking and non-breaking changes at both the implementation and interface levels.

Additional distinctions must be made for implementation and interface changes. For implementation changes we will differentiate between backend and front end. For interface changes we will differentiate between the communication protocol and the data model. 

Behavioral changes, though significant and worth addressing, are outside the scope of this RFC.

#### __What constitutes as a breaking change ?__
Throughout this RFC we will be discussing changes in terms of the following two categories

- What is a breaking change at the __implementation__ level?
- What is a breaking change at the __interface__ level?

We will not address what constitutes as a breaking change in the __behavioral__ level, though this will be an important topic to broach in the future.

#### __Implementation Version changes__

-  OKAPI Interface Consuming Implementations:
    |          Use Cases                                      | __Breaking Change__  | __Non-Breaking__   |
    | --------------------------------------------------------| -------------------- | ------------------ |
    | The introduction of a newly consumed OKAPI interface    |  <center>X</center>  |                    |
    | The removal of an already consumed OKAPI interface      |                      | <center>X</center> | 
    | The introduction of an additional version of an already consumed OKAPI interface | | <center>X</center> |
    | The removal of an an an additional version of an already consumed OKAPI Interface | <center>X</center> | | 
    | A change of the minimum version of an OKAPI interface already consumed |  <center>X</center> |      |

-  In UI Implementations:

    |          Use Cases                                      | __Breaking Change__  | __Non-Breaking__   |
    | --------------------------------------------------------| -------------------- | ------------------ |
    | The addition of a public route                               |                 | <center>X</center> |
    | The removal of a public route                                |  <center>X</center>  |               |
    | The change of a public route                                 |  <center>X</center>  |               |
    | The addition of a new peer dependency                        |  <center>X</center>  |               |
    | The removal of an existing peer dependency                   |                      | <center>X</center> |
    | A change in the version of an existing peer dependency       |  <center>X</center>  |               |
    | The addition of a new direct dependency                      |                 | <center>X</center> |
    | A removal of an existing direct dependency                   |                 | <center>X</center> |
    | A change in the version of an existing direct dependency     |                 | <center>X</center> |
    | The addition of a new development dependency                 |                 | <center>X</center> |
    | The removal of an existing development dependency            |                 | <center>X</center> |
    | A change in the version of an existing development dependency|                 | <center>X</center> |

- In Backend Implementations:
    |          Use Cases                                | __Breaking Change__  | __Non-Breaking__ |
    | --------------------------------------------------| ---------------------| ---------------  |
    | An operational change                             |  <center>X</center>  |                  |
    | The addition of a newly provided interface        |                      |<center>X</center>|
    | A major change within an interface                |  <center>X</center>  |                  |
    | The removal of a provided interface               |  <center>X</center>  |                  |
    | A change in the runtime environment               |  <center>X</center>  |                  |
    | The addition of a trailing '0' to an interface version |                 |<center>X</center>|
    | The increase of the minor version of a provided interface |              |<center>X</center>|
    | The decrement of the minor version of a provided interface |<center>X</center>|             |
    | The addition of in-code optimisations             |                      |<center>X</center>|

        
#### __Interface Version changes__ 

- Changes in the communication protocol:
    |                  Use Case                        | __Breaking Change__ |  __Non-Breaking__  |
    | ------------------------------------------------ | ------------------- | ------------------ |
    | The removal of an endpoint                       | <center>X</center>  |                    |
    | The addition of a new endpoint                   |                     | <center>X</center> |
    | The change of an existing endpoint's path        | <center>X</center>  |                    |
    | The addition of an optional query parameter to an existing endpoint  | | <center>X</center> |
    | The addition of a required query parameter to an existing endpoint   | <center>X</center> | |
    | The removal of an existing endpoint's query parameter  | <center>X<center> |                |
    | The change of an existing endpoint's HTTP method | <center>X</center>  |                    |
    | The change of an existing endpoint response's content type | <center>X</center> |           |
    | The change of an existing endpoint request's content type | <center>X</center> |            |
    | The addition or removal of an HTTP status code from an existing endpoint|<center>X</center>||
    

- Changes to the Data model:
    |                  Use Case                    | __Breaking Change__ |  __Non-Breaking__  |
    | -------------------------------------------- | ------------------- | ------------------ |
    | The removal of a required field              |  <center>X</center> |                    |
    | The addition of a new optional field         |                     | <center>X</center> |
    | The removal an optional field                |                     | <center>X</center> |
           
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
