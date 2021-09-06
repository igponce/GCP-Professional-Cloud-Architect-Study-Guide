# Cloud Storage

Cloud Storage is and *object* storage solution.

This means, it cannot be used as the underlying storage for a disk image.

Object storage is way cheaper than block storage. 

Block storage needs very low latency and high throughput between the compute nodes and the storage itself, which is expensive.

Object storage needs just good communications with the rest of the infrastructure, but it latency and access time requirements are _relaxed_.

# Terminology

- (Bucket)[glossary.md#bucket]
- Blob
- Retention period
- Storage Class
- Notification



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

Data in **encrypted in transit** : all communications from Google to you are encrypted using TLS.

The data will be always **encrypted at rest**, either with a google-managed key or a key that your provide yourself. 

Google rotates encryption keys periodically when the key is "old". This is transparent to you.

## Data versioning

## Best Practices
# How to speed up a file upload?

If you want to upload files faster to Google Cloud Storage you should use the `-m` switch when you run `gsutil cp`. This makes use of sereveral processes in parallel speeding up the file upload.

```
gsutil cp -m <files> gs://bucket/path/
```

When you have several different files to upload, a good idea is to use a random prefix like `gs://bucket/random_prefix/filename`.

This makes you use different cloud storage ingestion servers (yes, there's a big balancer in front of it and one of the key factors that determine which storage server should get the request is the blob name).

## Use buckets not "directories"

Content stored in Cloud Storage looks like it is part of a filesystem. It isn't.
When you see a '/' (slash) character as part of a blob name, there is no directory or folder behind.
This is just part of the blob name.

When you want to know what's stored "behind" a given directory, by executing something like this:

```
gsutil ls gs://bucket/part_of/the/blob/**
```

The gsutil tool has to list under the hood *all* blob names in the bucket and apply a filter.
This takes time and resources.

If you need to use _a lot_ information stored under a "directory" in cloud storage consider using a separate bucket for this purpose.
This will allow you to apply a storage class for that information, and will be able to copy or import data quickly using a transfer service instead of reading files from cloud storage, then writing files again to cloud storage.
