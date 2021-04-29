# GIDS Health Tools Interoperability Changelog

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