- Start Date: 2019-10-28
- Relevant Team(s): Ember Data
- RFC PR: (after opening the RFC PR, update this with a link to it and update the file name)
- Tracking: (leave this empty)

# Deprecate ember-data store's default injection

## Summary

ember-data's `store` is currently injected by default through an initializer.

This RFC proposes to remove the default `store` injection from that initializer.
Instead, users will have to inject the `store` manually.

## Motivation

Using the `store` property either in a route or in a controller can feel magical
✨. Indeed, it doesn't need to be injected to be present in the route or the
controller context.

This RFC wants to address that.

## Transition Path

To complete the removal, we will need a transition phase during which a
deprecation will be triggered (in the initializer, meaning once at app boot).

Prior to the deprecation, we would create a codemod to ease the migration.

But the first step could be to update the documentation to teach users to inject
the store manually (more details below).

The default injection would eventually be removed in `@ember-data/store@4`.

To sum-up, the release plan would be:
- first step: update the documentation + create the codemod
- second step: add the deprecation
- third step: remove the deprecation and the default injection (4.0)

### Codemod

A codemod will be created to add the service injection in all routes and
controllers. Scoping the service injection to routes and controllers that
actually use the `store` service would be pretty hard. Indeed, it would mean
looping over javascript _and_ handlebars files...

The codemod will be added to
[ember-data-codemod](https://github.com/ember-codemods/ember-data-codemod)

### Deprecation

A regular deprecation will be added to
[the initializer](https://github.com/emberjs/data/blob/v3.15.0-alpha.1/packages/-ember-data/addon/setup-container.js#L43).

The [deprecation app](https://github.com/ember-learn/deprecation-app) will be
updated with the new deprecation.

## How We Teach This

The replacement will not need to be specifically taught. The deprecation should
be enough.

Though, the guides will need to be updated.
[Here](https://guides.emberjs.com/v3.12.0/models/pushing-records-into-the-store/#toc_pushing-records)
for instance. ember-data inline documentation will need an update too.
[Here](https://api.emberjs.com/ember-data/3.12/classes/Store/methods/createRecord?anchor=findAll)
for instance.

Note: as mentioned above, updating the documentation would actually be the first
task to complete this RFC.

## Drawbacks

Should we list here the fact that the service will have to be manually injected?
I won't list it here as a drawback since it is the goal of the RFC 😉.

## Alternatives

The status quo.

## Unresolved questions

n / a
