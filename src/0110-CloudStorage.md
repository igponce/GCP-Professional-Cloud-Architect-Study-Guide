# Cloud Storage

Cloud Storage is and *object* storage solution.

This means, it cannot be used as the underlying storage for a disk image.

Object storage is way cheaper than block storage. 

Block storage needs very low latency and high throughput between the compute nodes and the storage itself, which is expensive.

Object storage needs just good communications with the rest of the infrastructure, but it latency and access time requirements are _relaxed_.

## Cloud Storage URLs

When you store some files in cloud storage, the URI looks like this:

```
gs://bucket_name/object/name
```

| Part | Meaning |
|------|---------|
| gs:// | This is the URI protocol prefix. For google cloud storage it is just `gs:` |
| bucket_name | This is the name of the bucket (where we put the objects a.k.a. files in Cloud Storage). This must be unique in all google cloud. |
| object/name | The name of the object (or blob name) you put in cloud storage. Extensions and slashes (/) are part of the name. |

**TIP**

   From the URL you cannot tell where data is located, or the kind of storage you are using.

   You can remove a bucket, and create a new one with the same name on a different location or a different (usually cheaper) storage type. 

You can see that the URI looks like there is a filesystem behind. It is not.

Under the hood Google maintains a "name" server that maps blob names to where the data it is actually stored.

The data will be always **encrypted at rest**, either with a google-managed key or a key that your provide yourself. 

Google rotates encryption keys when the key is "old". This is transparent to you.