# Google Drive API Test

Domain: https://www.googleapis.com

Max File Upload: 5120GB

Quotas: 12000 Query Per User Per 60 Seconds

Complete Documentation: https://developers.google.com/drive/api/reference

## Notes

- The Drive API can be used to access both Private Drive and Shared Drive.
- The Shared Drive can only be accessed if url parameter contain 'supportsAllDrives=true'
- The Drive API Doesn't provide an API to get full path from a file. However, we can get parent field from file which contains the folder id from where the file resides. And we can make recursive call from that and appending the path or do whatever we want.
- We get an error from binary data converted to base64 when uploading base64 encoded binary file.
```
Malformed multipart body.
```
- Only text files work.
- We cannot access the Shared Drive for some reason.

## Upload File

Description:

Upload file with metadata in one request.

### Request

- Endpoint /upload/drive/v3/files
- Method: POST
- Header:
```
Authorization: Bearer {Google OAuth2 Token}
Content-Type: multipart/related; boundary={anything for delimiting the request body e.g metadata & (usually) base64 file content}
```

- Request Body (Raw text):
```
--{boundary}
Content-Type: application/json; charset=UTF-8

{
    "name": "filename",
    "mimeType": "file mimeType"
}
--{boundary}
Content-Type: {file mimeType}
Content-Transfer-Encoding: base64

{base64 encoded file content}

--{boundary}--
```

## Upload File To a Folder (Only one Folder)

- Endpoint /upload/drive/v3/files
- Method: POST
- Header:
```
Authorization: Bearer {Google OAuth2 Token}
Content-Type: multipart/related; boundary={anything for delimiting the request body e.g metadata & (usually) base64 file content}
```

- Request Body (Raw text):
```
--{boundary}
Content-Type: application/json; charset=UTF-8

{
    "name": "filename",
    "mimeType": "file mimeType",
    "parents": ["folder id / shared drive id"]
}
--{boundary}
Content-Type: {file mimeType}
Content-Transfer-Encoding: base64

{base64 encoded file content}

--{boundary}--
```


### Response 200 OK

```json
{
    "kind": "drive#file",
    "id": "1YSU_S6WKGlF8Q6CyKxUcdrnkBM27luOL",
    "name": "abcd.js",
    "mimeType": "text/javascript"
}
```

## Delete File

### Request

- Endpoint /drive/v3/files/{fileId}
- Method: DELETE
- Header:
```
Authorization: Bearer {Google OAuth2 Token}
```

### Response 204 No Content

## Get File

Retrieve file content or file metadata based on url parameter

### Request

- Endpoint /drive/v3/files/{fileId}
- Method: GET
- Header:
```
Authorization: Bearer {Google OAuth2 Token}
```

### Response 200

- if exists urlparam "alt=media"

```
{raw file data}
```

- if not

```
{
    "kind": "drive#file",
    "id": "file id",
    "name": "file-name.pdf",
    "mimeType": "application/pdf"
}
```

## Upload File with Pause support (WIP)

## Update File (WIP)

## Move File To Trash (Endpoint Same as Update File)

## 