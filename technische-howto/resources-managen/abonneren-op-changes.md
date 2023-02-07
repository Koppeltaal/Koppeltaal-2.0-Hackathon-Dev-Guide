---
description: >-
  It is often important to be notified of changes to specific resources. Within
  Koppeltaal it is possible to be notified of changes on a very accurate level.
---

# Subscribing to changes

{% hint style="warning" %}
The notification mechanism has a maturity level 3. We know that FHIR is working hard to improve the notification mechanism. In the future version, it is very likely that this functionality will change. For example, currently there is no notification when a resource is deleted.
{% endhint %}

### Subscribe to changes

Applications can create their own notification using the [`Subscription`](https://www.hl7.org/fhir/r4/subscription.html) resource. This must adhere to the [Koppeltaal 2.0 profile](https://simplifier.net/koppeltaalv2.0/kt2subscription). The `Subscription`, like the other resources, can be managed through the [CRUD Operations](crud-operaties/).

#### Criteria

The `Subscription.criteria` field can be filled by applications themselves. With each change, the Koppeltaal server will check if there are `Subscriptions` where the criteria matches the created/changed `Resource`. If so, a `POST` notification will be sent to the `Subscription.channel.endpoint` for the matched subscriptions.&#x20;

#### Channel

The `Subscription.channel` is used to specify how and where the notification should go. In the POC, Koppeltaal will only support the `rest-hook` type without a payload.

{% hint style="info" %}
The Koppeltaal Server applies "Notification Narrowing"; similar to [Search Narrowing](../../domeinbeheer/rollen-beheren/search-narrowing.md) but for notifications. However, when something goes wrong in this process, the notification WILL be sent. In addition, an application may have multiple `Subscriptions` that match the set `criteria`.&#x20;

Applications should therefore note that a notification does not always result in a`Resource` to be retrieved.

Applications should also note that subscriptions might not be received. Perhaps the webhook endpoint was unavailable, or the process runs into an error somewhere. When syncing state is important, it is important to keep the before-mentioned in mind and implement a mechanism that still synchronises all data.
{% endhint %}

### Limitations

There are differences between the FHIR `Subscription` and the Koppeltaal 2.0 profile. For example, Koppeltaal does not allow the creation of a `Subscription` where the `Subscription.payload` is turned on. This way, the notification is purely a "ping" to indicate that something has changed, but not WHAT content has changed. This choice was made to avoid accidentally sending sensitive information to the wrong endpoint.

### Example Subscription

```json
{
  "resourceType": "Subscription",
  "meta": {
    "profile": [
      "http://koppeltaal.nl/fhir/StructureDefinition/KT2Subscription"
    ]
  },
  "status": "active",
  "reason": "Process finished Tasks",
  "criteria": "Task?status=ready",
  "channel": {
    "type": "rest-hook",
    "endpoint": "https://koppeltaal-testteam6.nl/fictief-subscription",
    "header": [
      "X-KTSubscription: TaskReady",
      "Authorization: Bearer <TOKEN>"
    ]
  }
}
```

