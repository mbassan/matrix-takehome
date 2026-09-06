## Uploads API

### `POST /upload-batch`

Creates an upload batch and one file record per file.

- Request: files with system data (`name`, `type`, `size`) and optional file-type metadata
- Response: `uploadBatchId`, `uploadBatchIdempotencyKey`, and each file's `fileId` and `fileIdempotencyKey`

```json
// Request
{
  "files": [
    {
      "name": "sweep-042.radar",
      "type": "application/x-radar",
      "size": 5368709120,
      "metadata": { "mission": "atlantic-survey-2026" }
    },
    {
      "name": "briefing.pdf",
      "type": "application/pdf",
      "size": 2485760
    }
  ]
}
```

```json
// Response
{
  "uploadBatchId": "ub_9f1c2a",
  "uploadBatchIdempotencyKey": "ubik_7a3e9c",
  "files": [
    {
      "fileId": "file_1a2b3c",
      "fileIdempotencyKey": "fik_6e5d4c"
    },
    {
      "fileId": "file_4d5e6f",
      "fileIdempotencyKey": "fik_2b1a0f"
    }
  ]
}
```

### `POST /upload-batch/:uploadBatchId/presign`

Returns presigned S3 URLs for the next sub-batch of multipart chunks.

- Request: `chunks[]`, each containing `fileId` and `chunkId`
- Response: entries containing `fileId`, `chunkId`, `signedUrl`, and S3 `uploadId`
- Limit: at most `PRESIGNED_URL_BATCH_SIZE` chunks per request

```json
// Request
{
  "chunks": [
    { "fileId": "file_1a2b3c", "chunkId": 0 },
    { "fileId": "file_1a2b3c", "chunkId": 1 },
    { "fileId": "file_4d5e6f", "chunkId": 0 }
  ]
}
```

```json
// Response
{
  "chunks": [
    {
      "fileId": "file_1a2b3c",
      "chunkId": 0,
      "uploadId": "s3upload_abc123",
      "signedUrl": "https://bucket.s3.amazonaws.com/file_1a2b3c?partNumber=1&uploadId=abc123&X-Amz-Signature=..."
    },
    {
      "fileId": "file_1a2b3c",
      "chunkId": 1,
      "uploadId": "s3upload_abc123",
      "signedUrl": "https://bucket.s3.amazonaws.com/file_1a2b3c?partNumber=2&uploadId=abc123&X-Amz-Signature=..."
    },
    {
      "fileId": "file_4d5e6f",
      "chunkId": 0,
      "uploadId": "s3upload_def456",
      "signedUrl": "https://bucket.s3.amazonaws.com/file_4d5e6f?partNumber=1&uploadId=def456&X-Amz-Signature=..."
    }
  ]
}
```

### `PATCH /upload-batch/:uploadBatchId`

Reports aggregated upload progress.

- Request: `uploadedBytes` and `uploadedChunks`
- The backend completes the S3 multipart upload after all chunks are present

```json
// Request
{
  "uploadedBytes": 3221225472,
  "uploadedChunks": [
    { "fileId": "file_1a2b3c", "chunkId": 0, "etag": "\"e1f2a3\"" },
    { "fileId": "file_1a2b3c", "chunkId": 1, "etag": "\"b4c5d6\"" }
  ]
}
```

```json
// Response
{
  "uploadBatchId": "ub_9f1c2a",
  "status": "IN_PROGRESS"
}
```
