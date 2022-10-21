
- Start Date: 2022-10-21
- RFC PR: 
- FOLIO Issue: 


# Folio Breaking Changes

## Summary

This RFC's purpose is to deliver to developers and other effected groups a classification of changes within Folio either as a breaking change or otherwise.

## Motivation

- To help different Folio implementors know with every release what significant changes have been added.
- Classification of those significant changes into breaking and non-breaking changes.

## Detailed Explanation/Design

### Specifics

The RFC aims to classify the changes made at the folio module and interface level as breaking changes and non-breaking changes. To elaborate, how or what change in a module or a part of interface can cause componenet failures and how these changes should be communicated to the implementors, developers, etc.



#### __What constitutes as a breaking change ?__

- What is a breaking change at module level?
- What is a breaking change at the interface level?

#### __Classifying the breaking changes.__

- Module Version(versioned independent of the interface):
    - What is a breaking change with respect to a module version change ?
        -  In UI modules :
            - Okapi interface version change - both new minimum version and new interface version
            - Addition of a new peer dependency
            - Version change of an existing peer dependency or dependencies.
            - Change in public routes.
        - In Backend modules :
            - Changes to both required and optional interface dependencies - eg. dropping support for an interface version, i.e., replacing __version 3.2__ with __version 3.3__ is a breaking change
            - 





## Risks and Drawbacks

Why should we not do this? 

A genuine and thoughtful consideration to risks and drawbacks is essential for a well-rounded proposal. 

## Rationale and Alternatives

Why is this design the best in the space of possible designs? How does this design integrate (or not) into the existing architecture and practices in Folio?


What other designs have been considered and what is the rationale for not choosing them? 

This section could also include prior art, that is, how other the same problem may have already been solved elsewhere.

## Unresolved Questions

Optional, but suggested for first drafts. What parts of the design are still TBD?

What related issues do you consider out of scope for this RFC that could be addressed in the future independently of the solution that comes out of this RFC?
