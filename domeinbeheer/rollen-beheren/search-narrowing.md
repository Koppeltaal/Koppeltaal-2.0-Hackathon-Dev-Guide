# Search Narrowing

{% hint style="info" %}
Permissions bound to a role are set as OAuth scopes on the `access_token` used to communicate with the Koppeltaal server. Therefore, the `access_tokens` should be short lived. Revoking a permissions will not be instantly applied. This can take up to 5 minutes.
{% endhint %}

When an application is allowed to read resources with an `Own` or `Granted` scope, it means that not all resources are allowed to be returned by the Koppeltaal server. To keep this as simple as possible for platforms that use Koppeltaal, "Search Narrowing" will be applied to these requests. Search narrowing means that the client can simply request `GET /Patient` to retrieve "all" `Patient` resources. The Koppeltaal server ensures that only the `Patient` resources for which the application is authorised are returned.

{% embed url="https://drive.google.com/file/d/1bJFTUdJPhLLOxm-iSk33K8raf9Ebm3JN/view" %}
Search Narrowing
{% endembed %}

