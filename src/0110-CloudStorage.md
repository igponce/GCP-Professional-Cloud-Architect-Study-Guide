# Cloud Storage

Cloud Storage is an *object* storage solution.

Object storage is good to store plain files.
It is easy to write and read files as a whole. You can start reading the file from an offset, but most of the time of you will work with whole files.

If you have to store a disk image, you need block storage. Block storage is optimized for random access patterns, just like an ordinary disk.

Block storage also needs very low latency and high throughput between the compute nodes and the storage itself, which is expensive.

Object storage is way cheaper than block storage. 

Object storage needs just good communications with the rest of the infrastructure, but its latency and access time requirements are _relaxed_.

# Terminology

Some cloud storage concepts you need to know:

- Blob
- (Bucket)[glossary.md#bucket]
- Notification
- Region
- Retention period
- Storage Class


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

   In cloud storage, you cannot tell from the URL  where data is located, or the kind of storage you are using.

   You can remove a bucket, and create a new one with the same name on a different location or a different (usually cheaper) storage type. 

You can see that the URI looks like there is a filesystem, but there isn't.

Under the hood Google maintains a "name" server that maps blob names to where the data it is actually stored. You cannot see where this data is.

Data in **encrypted in transit**: all communications from Google to you are encrypted using TLS.

The data will always be **encrypted at rest**, either with a google-managed key or a key that your provide yourself. 

If you tell Google to manage encryption for you, they will periodically rotate encryption keys. This makes easy for them to migrate data inside their datacenters and, in case of a disk failure, nobody can recover data from the disks.

This google-managed encryption is transparent to you. You always get the data unencrypted when you talk with the Cloud Storage service.

## Object versioning

Cloud Storage can maintain different versions of a blob you store in a bucket.

This works in a bucket basis: you cannot have *some* blobs in a bucket with versioning and others *without* versioning.

This feature is not enabled by default.

Remember that you will be billed for the storage used for both current blob versions and old versions.

### Object versioning example

First we'll create a new bucket in Belgium an we will enable object versioning:

```
# gsutil mb -l europe-west1 gs://test_versioning
# gsutil versioning set on gs://test_versioning
Enabling versioning for gs://test_versioning/...
```

Now we upload the same blob two times with different content:
```
echo "version1" | gsutil cp - gs://test_versioning/hello.txt

echo "version2" | gsutil cp - gs://test_versioning/hello.txt
```

We can see there is just one file in the bucket:
```
# gsutil ls -l gs://test_versioning
        10  2021-09-13T10:24:22Z  gs://test_versioning/hello.txt
```

But we have more than one revisions of the same file:
```
$ gsutil ls -al gs://test_versioning
10  2021-09-13T10:21:47Z  gs://test_versioning/hello.txt#1631528507797756  metageneration=1
10  2021-09-13T10:21:53Z  gs://test_versioning/hello.txt#1631528513817575  metageneration=1
```

If we overwirte the blob with the same content, cloud storage will keep a new version of it:

```
# echo "version2" | gsutil cp - gs://test_versioning/hello.txt

# gsutil ls -al gs://2021test_versioning
10  2021-09-13T10:21:47Z  gs://test_versioning/hello.txt#1631528507797756  metageneration=1
10  2021-09-13T10:21:53Z  gs://test_versioning/hello.txt#1631528513817575  metageneration=1
10  2021-09-13T10:24:22Z  gs://test_versioning/hello.txt#1631528662596486  metageneration=1
```

If we want to retrieve or remove old versions of the objects, we must specify their generation (the number after the `#` character):


```
# gsutil cat gs://test_versioning/hello.txt#1631528507797756
version1
```

## Best Practices
# How to speed up a file upload?

If you want to upload files faster to Google Cloud Storage you should use the `-m` switch when you run `gsutil cp`. This makes use of several processes in parallel speeding up the file upload.

```
gsutil cp -m <files> gs://bucket/path/
```

When you have several different files to upload, it is a good idea is a random prefix like `gs://bucket/random_prefix/filename`.

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
