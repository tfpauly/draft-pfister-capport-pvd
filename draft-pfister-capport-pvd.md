---
title: Using Provisioning Domains for Captive Portal Discovery
abbrev: Captive Portal PvD
docname: draft-pfister-capport-pvd-latest
date:
category: std

ipr: trust200902
keyword: Internet-Draft

stand_alone: yes
pi: [toc, sortrefs, symrefs]

author:
  -
    ins: P. Pfister
    name: Pierre Pfister
    org: Cisco
    street: 11 Rue Camille Desmoulins
    city: Issy-les-Moulineaux 92130
    country: France
    email: pierre.pfister@darou.fr
  -
    ins: T. Pauly
    name: Tommy Pauly
    org: Apple Inc.
    street: One Apple Park Way
    city: Cupertino, California 95014
    country: United States of America
    email: tpauly@apple.com

normative:
  RFC3629:
  RFC7556:
  I-D.ietf-intarea-provisioning-domains:
  I-D.ietf-capport-api:
informative:
  I-D.ietf-capport-architecture:

--- abstract

Devices that connect to Captive Portals need a way to identify that the network is restricted and discover a method for opening up access. This document defines how to use Provisioning Domain Additional Information to discover a Captive Portal API URI.

--- middle

# Introduction

The Captive Portal Architecture {{I-D.ietf-capport-architecture}} defines the interaction model for how client devices, or User Equipment, interacts with a network that is restricted and requires explicit user interaction to allow a device to access the Internet. The first step of this process involves a Provisioning Service communicating with the User Equipment to indicate that the network is captive, and how to get out of captivity. The key piece of information that the Provisioning Service provides is the URI of a JSON-based API that allows the User Equipment to interact with the captive portal. This API is specified in {{I-D.ietf-capport-api}}.

This document defines the mechanism for using Provisioning Domain (PvD) Additional Information as the Captive Portal Provisioning Service. A PvD defines a consistent and usable set of network configurations {{RFC7556}}. A Captive Network is one example of a PvD that has unique properties that a device needs to be aware of when presenting networks to generic applications. Naming specific PvDs and presenting a set of Additional Information for a PvD is defined in {{I-D.ietf-intarea-provisioning-domains}}.

# Captive Portal URI Option {#captive-option}

The Additional Information fetched for a PvD is presented as JSON. This document defines a new key to be used to identify the Captive Portal API URI. As specified in {{I-D.ietf-capport-api}}, this URI MUST have an "https" scheme.

JSON Key:
: captive-api

Description:
: URI of Captive Portal API

Type:
: UTF-8 string {{RFC3629}}

Example:
: "https://captive.example.com/api"

# Client Behavior

When a client device that support PvDs attaches a network, it will discover if there is one or more named PvDs on the network with a Router Advertisement as specified in {{I-D.ietf-intarea-provisioning-domains}}.

If the PvD indicates that it has Additional Information, the client device SHOULD fetch the Additional Information prior to allowing the PvD to be used for generic network access, in case the network is restricted or captive. If the Additional Information contains the "captive-api" key, then the client device can interact with the Captive Portal API before proceeding with using the network. If the Additional Information does not contain the "captive-api" key, then the client  SHOULD assume that the network is not captive, and proceed with using the network.

If the PvD indicates that it has no Additional Information, the client device SHOULD assume that the network is not captive, and proceed with using the network.

It is possible that a misconfigured network will provide a named PvD without explicitly marking the captive option, while still restricting network access and providing a Captive Portal. In this case, connections made by the client device may be blocked or redirected, as occurs in captive network in which there is no explicit provisioning.

# Security Considerations

The Captive Portal PvD option is subject to the same security considerations as any other options provisioned via Router Advertisements and Explicit Provisioning Domains. This information should not be used by client devices to trust the safety or security of a network attachment.

# IANA Considerations

This document adds a new key to the "Additional Information PvD Keys" defined in
{{I-D.ietf-intarea-provisioning-domains}}. See {{captive-option}} for the new key definition.

# Acknowledgements

Thanks to contributions from Eric Vyncke, Mark Townsley, David Schinazi, and Kyle Larose.
