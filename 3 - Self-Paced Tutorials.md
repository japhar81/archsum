**Using our OpenShift cluster**, follow these tutorials (in order);

Preliminary Step:

Since many users are doing these labs on the same Openshift cluster, we all need to have unique project names. Create a new project with your unique username in it for these three tutorials:

```bash
oc new-project myproject-<your-username>
```

In any of the following exercises, when asked to create a project named "myproject" or "demo-wildfly", just use your "myproject-\<your-username\>" project instead.

One more note on the tutorials. They are using a very slightly older version of Openshift than we have installed in-house. So, the console UI and outputs, in some cases, may be a bit different. Everything is functionally the same, but don't be surprised if you see different wording here or there.

Now, on to the tutorials:

1. [https://learn.openshift.com/introduction/resource-objects/][1]
2. [https://learn.openshift.com/introduction/deploying-python/][2]
3. [https://learn.openshift.com/introduction/port-forwarding/][3]

Once you are comfortable with these concepts, put it all together, and deploy a full Java application, with a backend DB. This tutorial provides a good reference on how to do so; [http://www.mastertheboss.com/soa-cloud/openshift/java-ee-example-application-on-openshift][4]

[1]: https://learn.openshift.com/introduction/resource-objects/
[2]: https://learn.openshift.com/introduction/deploying-python/
[3]: https://learn.openshift.com/introduction/port-forwarding/
[4]: http://www.mastertheboss.com/soa-cloud/openshift/java-ee-example-application-on-openshift
