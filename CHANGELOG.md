# GIDS Health Tools Interoperability Changelog

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