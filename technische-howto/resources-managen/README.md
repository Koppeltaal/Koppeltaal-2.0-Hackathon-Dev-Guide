# Managing resources

It is very important to understand that **applications** connect to the Koppeltaal Server, and not the **end users**. Koppeltaal ensures that applications are only allowed to perform operations on resources to which they are authorised. At the user level, the applications themselves are responsible for keeping track of which operations are allowed to be performed.&#x20;

So this means, for example, that an application may request all tasks to which they are authorised. However, the Koppeltaal Server does not know to whom this resource is shown (after all, it is provided to an application). So it is up to the application to ensure that the tasks of e.g. `Patient/123` are only shown to authorised users.
