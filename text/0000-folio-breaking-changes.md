
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

- What is a breaking change at module level?
- What is a breaking change at the interface level?
- What constitutes as a breaking change in the behavioral level? 

#### __Guide to what constitutes as breaking changes.__

- Module Version changes(versioned independent of the interface):
    - What is a breaking change with respect to a module version change ?
        -  In UI modules :
            - Okapi interface version change - both new minimum version and new interface version
            - Addition of a new peer dependency
            - Version change of an existing peer dependency or dependencies.
            - Change in public routes.
        - In Backend modules :
            - Changes to both required and optional interface dependencies - eg. dropping support for an interface version, i.e., replacing __version 3.2__ with __version 3.3__ is a breaking change
            - Removing any interface is considered a breaking change.
            - Changing the minor version of an interface is also considered as a breaking change.
            - New operational requirement (e.g. requiring postgres or kafka) is a breaking change (mod-authtoken used to be self contained but now requires DB storage)
            - With respect to Okapi version change a new minimum version change is considered a breaking change as well as a new interface addition is also considered a breaking change
        - It is also considered as a breakinch when there is a major change within in an interface
        - If a module stops providing an interface, is also considered as a breaking change
        - Runtime environment changes are also breaking changes.
    - Things to consider while dealing with Module version changes:
        - The language here needs to be fleshed out with examples and reasoning, i.e. there is both a functional API (the provided interfaces) and an operational API (the runtime requirements) and changes to either of these must be reflected in a module’s semver. NB: make a general statement about the goal here, and then provide select examples). ()
        - Three kinds of breaking changes?
            - Infrastructural change : eg. modules requires Kafka, Postgres, etc.
            - Non-infrastructural changes: eg. modules need Java v17, Node v16, etc.
            - Configurational changes: eg. change to environmental variables.

- Interface Version changes:
    - What is a breaking change with respect to a interface version change ?
        - In UI modules :
            - 

#### __Guide to what constitutes as non-breaking changes.__




## Risks and Drawbacks

#### __Risk Scenarios__ 

A detailed RFC and ADR is made with guide to recognise and communicate probable breaking changes to the effected developers, etc, but  after all these efforts, the guide is used by no-one. 

## Rationale and Alternatives

## Unresolved Questions

- How to provide the guide with respect to the Behavioral changes ?
- What kind of change is dropping a minor/patch version of a runtime dependency? (Note that a dev doesn’t get to set the java version of the platform because container config happens independent of module development)
- How to provide a clear guidance as to how the proposal should be implemented?
