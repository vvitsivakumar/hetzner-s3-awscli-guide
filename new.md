# Hetzner Object Storage – AWS CLI Usage Guide

## 📘 Introduction

This guide shows how to use the AWS CLI with Hetzner Object Storage.  
You'll learn how to configure the CLI, list buckets, enable versioning, view object versions, and restore deleted files using version IDs.

---

## 🔧 1. Configure AWS CLI for Hetzner

```bash
aws configure
```

Provide the following when prompted:

- **AWS Access Key ID**: `<your-access-key>`  
- **AWS Secret Access Key**: `<your-secret-key>`  
- **Default region name**: `us-east-1` (any placeholder)  
- **Default output format**: `json` (or your preferred format)

---

## 📂 2. List All Buckets

```bash
aws --endpoint-url https://hel1.your-objectstorage.com s3api list-buckets
```

---

## 🔄 3. Enable Versioning on a Bucket

```bash
aws --endpoint-url https://hel1.your-objectstorage.com s3api put-bucket-versioning --bucket <BUCKETNAME> --versioning-configuration Status=Enabled
```

---

## 📋 4. Check Bucket Versioning Status

```bash
aws --endpoint-url https://hel1.your-objectstorage.com s3api get-bucket-versioning --bucket <BUCKETNAME>
```

---

## 🕒 5. List All Object Versions

```bash
aws --endpoint-url https://hel1.your-objectstorage.com s3api list-object-versions --bucket <BUCKETNAME>
```

---

## 🔑 6. Get Key and Version ID for Restore

Use the command below and find the `Key` and `VersionId` from the output:

```bash
aws --endpoint-url https://hel1.your-objectstorage.com s3api list-object-versions --bucket <BUCKETNAME>
```

Sample Output:

```json
{
  "Versions": [
    {
      "Key": "ticket_attachments/2025/7/21/file.png",
      "VersionId": "3HL4kqtJlcpXroDTDmjVBH40Nrjfkd9z",
      ...
    }
  ]
}
```

---

## ♻️ 7. Restore Deleted File by Deleting Newer Version

```bash
aws --endpoint-url https://hel1.your-objectstorage.com s3api delete-object --bucket <BUCKETNAME> --key "ticket_attachments/2025/7/21/file.png" --version-id "3HL4kqtJlcpXroDTDmjVBH40Nrjfkd9z"
```

---

## 📌 Notes

- Always specify `--endpoint-url` when using AWS CLI with Hetzner.
- You can upload or retrieve files using `aws s3 cp` or `aws s3api` commands with the same endpoint.
