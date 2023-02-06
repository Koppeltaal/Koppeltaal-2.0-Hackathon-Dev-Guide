# Revoke Permission

{% hint style="info" %}
Permissions bound to a role are set as OAuth scopes on the `access_token` used to communicate with the Koppeltaal server. Therefore, the `access_tokens` should be short lived. Revoking a permissions will not be instantly applied. This can take up to 5 minutes.
{% endhint %}

When there is no permission for a `DomainResource` & CRUD action, the application will, by definition, not be allowed to perform this action. Below is a demonstration of what happens when the `Read` permission is removed:

{% embed url="https://drive.google.com/file/d/1HzlgpKz7TLkLNnNmuhJ_KCKkWgxr_j2O/view" %}
Revoke Permission
{% endembed %}

