# How stuff works in Google Cloud

Or... in other words, how much is going to cost me this stuff, and how can I control this?

Outline:

- Global and regional services.
- Billing structure: organizations, projects, and folders.
- Undestanding IAM


# Global and Regional Services

Before we start deep down in services, we need to know the difference between global, regional services.

Maybe this is a bit too soon to see stuff like this; don't worry, you'll revisit this section later and you'll understand it perfectly.

Some GCP services are "global" in the sense they don't run on a specific part of the world.

A good example of a globally available service is the DNS. There is no concept fo a "local" DNS. The DNS service can be accessed anywere, right?

Another good example of global service is IAM. You will know a bit about IAM soon. By now, just think about permissions and identity: it makes sense to have a a global view of identity and permissions (this is a global service). 
Think the opposite: what kiond of nightmare would be having permissions and identities scattered all over the cloud.



