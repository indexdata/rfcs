
- 2023-09-18
- RFC PR: (leave this empty)
- FOLIO Issue: (leave this empty)

# Folio distributed vs. centralized configuration

## Summary

FOLIO relies on a tenant aware extendable microservice system. It is highly configurable and adoptable on all kind of requirements. This RFC shall give a guideline for developers, where configurations shall be put.

## Motivation

FOLIO had no strict guidelines for developers, how and where to store the configuration values. This led to a situation where different approaches where developed by different developer teams.
This makes it difficult for implementers, administrators and users to configure the system appropriate, as they may have to search the documentation how to read or write these configuration values.
Basically there are two competing concepts where to store the values: distributed in the owning modules or centralized is a special module that offers a configuration store for other modules.
This RFC shall give guidelines for developers, where configuration values shall be set.

## Detailed Explanation/Design

This RFC deals with configurations that configure the behaviour of the Folio tenant. Among all different kinds of configurations, this RFC does not deal with the following configurations, as their location and method to set/get them should not be changed:

* Settings stored in Okapi's /_/env APIs.
* Settings stored in Infrastructure (Kubernetes / Rancher) config maps and secrets.
* Settings stored in module container environment variables.
* Settings stored in the stripes front-end (stripes.config.js, etc.)

### mod-configuration will be dropped until Quesnelia release

mod-configuration is deprecated due to security problems since March 2022. It shall not be used any more to add new configuration variables. Modules still using mod-configuration have to move to other solutions until the Quesnelia release.

### Distributed configuration is preferred

Distributed configuration means that each module stores its configuration values itself, and offers API endpoints to query and store these values. Distributed configuration in a microservice architecture has some advantages:

* The modules can validate the values according to format and dependencies
* Modules do not depend on a configuration module, hence a better separation of microservices can be achieved
* Since all API endpoints have to be documented, a basic documentation of possible configuration variables is mandatory
* Configuration values can be cached, since no other module can change values.
* Access to configuration values can effectively controlled by permissions defined in the module.
* Write-only configuration values are possible, like credentials. The module can offer other operators than reading values like comparing hashes (possible in central configuration too?)
* Modules can handle upgrade of configuration variable names or values during module upgrades more flexible

Even when there are also some drawbacks on distributed configuration, it is the preferred way to configure backend modules in FOLIO.

### When to use central configuration

mod-settings solves the security problems of mod-configuration. It is the preferred module if configuration variables shall be stored centrally. It is not recommended to develop specialized modules for other central configuration store.

Centralized configuration can be used for:

* Non-sensitive information, that are used by more than one module or are completely independent of any module like locale settings
* Settings that are specific to a user

While these configurations can also be stored in a module, the developer can decide where these values shall be stored.

### Migration

Since most modules already store configuration values in a distributed way, only some cases need to be addressed.
For locale properties and other properties still residing exclusively in mod-configuration, the access to these properties has to be moved to the mod-settings API until the Quesnelia release. Therefore a mod-configuration module offering only READ and DELETE APIs will run in Quesnelia and the modules still using mod-configuration have to transfer their properties to mod-settings or to a distributed configuration. Migrated configurations in mod-configuration have to be deleted. This can be done during module upgrades.
mod-configuration will be removed in the release following the Quesnelia release.

## Risks and Drawbacks

Migrations from old mod-configuration can fail, and therefore tenant upgrades may fail. 

## Rationale and Alternatives

Both central and distributed configurations were discussed. 
While pure central configuration offer easier access to the complete configuration of a tenant, this is not desireable sice:
* it is a antipattern in a microservice architecture 
* validation of values is not possible
* differentiating configuration data and application data is not easy: are circulation rules configuration data or application data?

A pure distributed configuration has the following drawbacks:
* for centrally used configuration values, like locale settings, a owning module has to be found. It may not be intuitive finding out, which module holds these configurations.

Therefore a distributed configuration with some exceptions has been considered.

## Unresolved Questions

Future developments like consortia extensions and the discussion about application formalization may need to update this RFC to clarify how configurations are stored.
