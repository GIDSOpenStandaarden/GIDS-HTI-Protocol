# GIDS Health Tools Interoperability Changelog

---
## HTI:core version 2.0
Document version: 2.0.0
Date: 30-01-2023

This is a breaking change. The FHIR Task object turns out to be used in completely ways over different contexts. As the
FHIR Task object plays a central role in HTI, this has become troublesome. The main points are:

 * There is little to none consensus over the meaning of the most used fields (for, owner).
 * The above outcome has resulted in the observation that the usage of the FHIR task from one domain to another seems impossible.
 * The standard has changed not only the structure of the reference to the activityDefinition between FHIR R3 and R4, but also the contents of the field (from reference to the value of the uri).

In order to resolve this, the FHIR Task object will be replaced with a set of extra defined fields, except for the Task.for,
this field will be mapped to the sub field of the JWT token.

Documentation updates:
 * Issue #18: Request: let go of the nested FHIR task and use own defined fields instead

---
## HTI:core version 1.1
Document version: 1.1.1
Date: 27-09-2021

This is a documentation only update. The restriction on additional the FHIR fields has been relaxed. The intention
has always been to be able to use the FHIR resource as it is used in the domain, however, it should not contain
personal information. Somehow the documentation has been more rigid and stated that this could ONLY be done if there
has been an additional profile defined. This might be an overkill and has been relaxed.

The `exp` field has been clarified some more, it contained a fairly useless statement about transfer times. The
definition has been simplified; no longer valid after this timestamp. Period.

Documentation updates:
 * Issue #6: Explanation for the exp field in the JWT is a bit unclear
 * Issue #12: Remove the restriction of additional FHIR fields

---
## HTI:core version 1.1
Document version: 1.1
Date: 08-06-2021

HTI uses a FHIR `Task` in the launch. Before, HTI would define the user launching a `Task` with the `Task.for` field. Whilst
persisting a `Task`, the `Task.for` field will always be the `Resource` this `Task` is intended for.
The goal of this change is to make sure this `Task` can be interpreted equally for clients that are
and are not using a FHIR store. Therefore the "launching user" has been moved from `Task.for` to
the JWT [sub claim](https://datatracker.ietf.org/doc/html/rfc7519#section-4.1.2).

This also caused the `HTI:3rdparty` profile to become  irrelevant. Therefore, it has been removed.

The above are breaking changes. However, as HTI is not used in production-like situations yet, we
decided to keep this a "1.x" version.

---
## HTI:core version 1.0
Document version: 1.0
Date: 26-4-2021

Upgraded from v0.4 to 1.0 as we've introduced a breaking change. These are the changes provided in 1.0:

### Spec changes
* The FHIR Task can now be provided with a specific FHIR version by providing the `fhir-version` attribute;
* The default version of the FHIR Task is now R4 instead of STU3 (breaking change);
* Changed documentation to point to FHIR R4 documentation instead of STU3;
* Changed STU3 code samples with R4 code samples;

### Styling improvements
* Fixed link pointing to incorrect external source;
* Fixed link not pointing to internal anchor;
* Fixed multiline table layout;
* Changed links for HTI test suite from `edia-tst.eu` to `gidsopenstandaarden.org` as  these are now hosted by GIDS;
* Added this changelog.
