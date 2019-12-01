# Amazon S3 storage reference

## Integrate an S3 bucket in with Postput

* Create it with config files
Copy/paste the [`custom.json` ]() config file into the [`data/storage/custom`]() directory, then change its values accordingly.

* Create it with the admin

Access http://localhost:2002

* Create it with postgresql

````sql
INSERT INTO "Storage" ( name, uuid, "isDefault", "typeId", data, config, "creationDate", "updatedOn" ) VALUES ('my_customs_3_files', 'fd600d4d-ce30-4940-951d-26aeb12c70bf', true, 1, '{}', '{"custom":{"keyId":"AKXXXXXXXXXXXXXXXXXX","key":"XCKlXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX","container":"mycontainer","region":"us-east-1"},"allowUpload":true,"urls":["http://localhost:2000/"]}', NOW(), NOW())
````
