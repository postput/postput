# Amazon S3 storage reference

## Integrate an S3 bucket in with Postput

### Create it with config files
Copy/paste the [`custom-s3.json`](https://raw.githubusercontent.com/postput/postput/master/doc/S3/custom-s3.json) config file into the [`data/storage/custom`](https://github.com/postput/api/tree/master/data/storage/custom) directory, then change its values accordingly.

### Create it with the admin
Alternatively, you can create a storage via the postput admin.
if you launch the postput stack with docker-compose, the admin should be accessible at: http://localhost:2002

### Create it with postgresql

````sql
INSERT INTO "Storage" ( name, uuid, "isDefault", "typeId", data, config, "creationDate", "updatedOn" ) VALUES ('my_customs_3_files', 'fd600d4d-ce30-4940-951d-26aeb12c70bf', true, 1, '{}', '{"custom":{"keyId":"AKXXXXXXXXXXXXXXXXXX","key":"XCKlXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX","container":"mycontainer","region":"us-east-1"},"allowUpload":true,"urls":["http://localhost:2000/"]}', NOW(), NOW())
````
