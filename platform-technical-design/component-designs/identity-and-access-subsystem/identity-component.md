---
description: A description of the identity component
---

# identity

The identity component represents the user within the platform. All other records within the system will refer to the identity provided by this component, whether for building the relationship graph, registering exchange of value or auditing data access.

## ​C​ritical requirements

* Must provide of unique identifier - the Person ID \(PID\).
* Must store core profile, including name and email address
* Should encryption personal data

## ​Implementation

This will be provided by the an off-the-shelf Identity Provider \(IdP\). This could be a cloud-native IdP, or could be provided by an IdP such as [Keycloak](https://www.keycloak.org/). The benefit of using such an off-the-shelf provider is the knowledge that security design has been considered.

The design of the core profile data is to be finalised, but should include at least:

* Full name
* email address

This seems like very little, but much of the platform profile data will be stored in the graph as additional datasets and attributes. Keeping it minimal \(for MVP\) means that it can be extended as required.

