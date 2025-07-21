
# Hetzner Object Storage – AWS CLI Usage Guide

## 📘 Introduction

This guide shows how to use the AWS CLI with Hetzner Object Storage.  
You will learn how to configure the CLI, list buckets, enable versioning, view object versions, and restore deleted files using version IDs.


---

## 🔧 1. Configure AWS CLI for Hetzner

```bash
aws configure --profile hetzner
```

Provide the following when prompted:

- **AWS Access Key ID**: `<your-access-key>`
- **AWS Secret Access Key**: `<your-secret-key>`
- **Default region name**: `us-east-1` (or any placeholder, not used by Hetzner)
- **Default output format**: `json` (or your preferred format)

---

## 📂 2. List All Buckets

```bash
aws --profile hetzner \
  --endpoint-url https://hel1.your-objectstorage.com \
  s3api list-buckets
```

---

## 🔄 3. Enable Versioning on a Bucket

```bash
aws --profile hetzner \
  --endpoint-url https://hel1.your-objectstorage.com \
  s3api put-bucket-versioning \
  --bucket <BUCKETNAME> \
  --versioning-configuration Status=Enabled
```

---

## 📋 4. Check Bucket Versioning Status

```bash
aws --profile hetzner \
  --endpoint-url https://hel1.your-objectstorage.com \
  s3api get-bucket-versioning \
  --bucket <BUCKETNAME>
```

---

## 🕒 5. List All Object Versions

```bash
aws --profile hetzner \
  --endpoint-url https://hel1.your-objectstorage.com \
  s3api list-object-versions \
  --bucket <BUCKETNAME>
```

---

## 🔑 6. Get Key and Version ID for Restore

You can retrieve all versions of objects in a bucket and extract the `Key` and `VersionId` like this:

```bash
aws --profile hetzner \
  --endpoint-url https://hel1.your-objectstorage.com \
  s3api list-object-versions \
  --bucket <BUCKETNAME>
```

Sample output:

```json
{
    "Versions": [
        {
            "ETag": "\"abcd1234...\"",
            "Size": 12345,
            "StorageClass": "STANDARD",
            "Key": "ticket_attachments/2025/7/21/file.png",
            "VersionId": "3HL4kqtJlcpXroDTDmjVBH40Nrjfkd9z",
            "IsLatest": true,
            "LastModified": "2025-07-21T13:00:00.000Z"
        }
    ]
}
```

---

## ♻️ 7. Restore Deleted File by Deleting Newer Version

To **restore a deleted or overwritten file**, delete the latest version (if needed):

```bash
aws --profile hetzner \
  --endpoint-url https://hel1.your-objectstorage.com \
  s3api delete-object \
  --bucket <BUCKETNAME> \
  --key "ticket_attachments/2025/7/21/file.png" \
  --version-id "3HL4kqtJlcpXroDTDmjVBH40Nrjfkd9z"
```

> ⚠️ Replace the `--key` and `--version-id` with actual values from your version listing.

---

## 📌 Notes

- Always specify `--profile hetzner` and `--endpoint-url` when using AWS CLI with Hetzner.
- You can upload or retrieve files using `aws s3 cp` or `aws s3api` commands with the same endpoint.
