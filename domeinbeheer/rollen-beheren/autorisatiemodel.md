---
description: >-
  Koppeltaal 2.0 has an authorization model. This means that an application
  cannot simply see all data in a domain. What can be viewed is determined with
  the authorization model.
---

# Autorisation model

## 1. Authenticate

When an application has [joined a domain](../domein-toetreden.md), the application will have been assigned a `client_id`. This `client_id` is included in the `access_token` that is required to communicate with the Koppeltaal server. This way, the Koppeltaal server knows which application is performing a request and therefore the associated permissions.

## 2. Resource ownership

The Koppeltaal server automatically adds a `resource-origin` extension to every `DomainResource` that is created. This extension references to a specific `Device` resource that has a 1-on-1 relation with the `client_id`. This way, the origin of a resource can always be found. This is an essential part of the authorisation model.

## 3. Role and permissions

Every application in a domain is assigned a single role. A role maps to multiple permissions. A permission has the following 3 properties:

### Resource&#x20;

A permission always applies to a single [FHIR Domain Resource](https://www.hl7.org/fhir/r4/domainresource.html#bnr).&#x20;

### Action&#x20;

A CRUD-level (create, read, update, delete) action.&#x20;

### Scope&#x20;

The `resource-owner` scope. The following scopes are supported:

| Scope   | Description                                                                                                                                        |
| ------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| Own     | The permission only applies to resources (selected resource type of the permission) whose `resource-origin` matches the authenticated application. |
| All     | The permission applies to all resources (selected resource type of the permission) in the domain.                                                  |
| Granted | The permission applies to resources (selected resource type of the permission) whose `resource-origin` matches the selected application(s).        |
