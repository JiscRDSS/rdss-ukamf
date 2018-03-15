# UKAMF Integration - On Boarding

## Introduction

This document describes the information and work that a pilot of the RDSS project has to do in order for RDSS to use UKAMF for authentication. During the onboarding of a HEI to RDSS they must identify one or more  Identity Provider(s) (IdP) that users accessing their service must authenticate from. The IdP must be a registered entity with [UK federation] [1].

Where more than one IdPs are required for accessing a service, the UK federation Central Discovery Service (CDS) will be used.

## Scope
This document specifies the requirements for provisioning a service that is to be integrated to UKAMF. This document is for RDSS pilot institutions.

### Versioning

Current version:&nbsp;&nbsp;&nbsp;&nbsp;`0.0.1-SNAPSHOT`

### Comformance

The keywords **MAY**, **MUST**, **MUST NOT**, **NOT RECOMMENDED**, **RECOMMENDED**, **SHOULD** and **SHOULD NOT** are to be interpreted as described in [RFC2219](https://tools.ietf.org/html/rfc2119).

## Identity Provider Definition
One of the following must be defined for a RDSS Service Provider (SP) and configured on the Shibboleth 2 software. The entity identity for an IdP must match the entityId defined and published in [UK federation metadata.xml] [2].

Key | Element ? Attribute | Description / Notes | Example
-- | -- | -- | --
ENTITY-ID | SSO ? entityID | This must correspond to the entityID of an IdP defined in the UK federation metadata xml. | `<SSO entityID="https://idp.uni.ac.uk/idp/shibboleth"> SAML2 SAML1 </SSO> `
CDS_URL | SSO ? discoveryURL | This should only be defined for services that supports users from many IdPs. | `<SSO discoveryProtocol="SAMLDS" discoveryURL="https://wayf.ukfederation.org.uk/DS"> SAML2 SAML1 </SSO>`


## Attributes Requirements
The following core attributes are supported in UK federation. These attributes are provided and presented by an IdP to a SP. The SP uses the provided attributes for authorisation or to make resource entitlements decisions.

Attributes | Description and Use | Mandatory
-- | -- | --
eduPersonScopedAffiliation (ePSA) | A multi-value attribute that indicates the relationship a user has with the organisation. The attribute can contain one or more of the following enumerated values: <ul><li>student</li><li>staff</li><li>faculty</li><li>employee</li><li>member</li><li>affiliate</li><li>alum</li><li>library-walk-in</li></ul><p>Being a multi-value attribute, a user can have one or more value for the attribute. Some services or resources may only require the eduPersonScopedAffiliation attribute to reach an access decision for a resource. A user with an attribute value of member affiliation could either be a student, staff, faculty or employee (not staff or faculty affiliation). This means access policies that allows access to member affiliation should grant access to any user that is either a student or staff or faculty or employee. This is an optional attribute for RDSS SPs</p> | Y
eduPersonTargetedID (ePTID) | A persistent, non-reassigned, privacy-preserving identifier for a principal (i.e. user). This attribute should only be required by RDSS SP that needs to persist and identify a user from session to session, e.g. to support features such as persisting user preferences or personalisation. A service will also require this attribute if it needs to track user activity. | Y
eduPersonPrincipalName (ePPN) | The "NetID" of a principal (i.e. user) for the purposes of inter-institutional authentication. This attribute is a persistent, but non-anonymous (the value often corresponds to the user's login name) identifier, and should be considered as personal information. This is an optional attribute for RDSS SPs. | N
eduPersonEntitlement (ePE) | URI (either URN or URL) that indicates a set of rights or roles to specific resources. It indicates that a user satisfies the conditions that apply for access to a resource. This is a mandatory attribute for access to RDSS SPs. Valid Samvera entitlements: `ir-admin`, `ir-curator`, `ir-user` | Y

Attributes defined in X.520 are optional in UK federation, thus not all institutions will support these attributes. The [X.520 attributes] [3] that could be used for RDSS include:

| X.520 Attributes     | Description and Use    | Mandatory
| :------------- | :------------- | :-------------
| cn      | Common name, e.g. can be used to share a user's full name      | Y
| givenName | Used to share a user's firstname | Y
| sn | Used to share a user's lastname / surname | Y
| telephoneNumber | Used to share telephoneNumber | Y
| mail | Used to share email address | Y
| ou | Organisation unit, e.g. a department | Y


[//]: # (Reference links used in the body of this)

[1]: <https://github.com/joemccann/dillinger>
[2]: <http://metadata.ukfederation.org.uk/ukfederation-metadata.xml>
[3]: <https://www.itu.int/rec/dologin_pub.asp?lang=e&id=T-REC-X.520-201610-I!!PDF-E&type=items>
