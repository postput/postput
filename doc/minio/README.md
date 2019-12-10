# Minio reference

## Integrate a Minio bucket with Postput

### Create it with config files
Copy/paste the [`custom-minio.json` ](custom-scaleway.json) config file into the [`data/storage/custom`](https://github.com/postput/api/tree/master/data/storage/custom) directory, then change secret values accordingly.

### Create it with the admin

Access http://localhost:2002

### Create it with postgresql

````sql
INSERT INTO "Storage" ( name, uuid, "isDefault", "typeId", data, config, "creationDate", "updatedOn" ) VALUES ('my_customs_minio_files', '5141322a-047c-4fb0-9b88-ec1b0ae4ea61', true, 1, '{}', '{ "custom": { "endpoint": "http://172.17.0.2:9000", "bucket": "testpostput", "accessKeyId": "xxxxxxxxxxxxxxxxxxxxxxx", "secretAccessKey": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX", "s3ForcePathStyle": true, "signatureVersion": "v4" }, "allowUpload": true, "urls": ["http://localhost:2000/", "https://www.my-other-domain.com"] }', NOW(), NOW())
````
