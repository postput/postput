# Alibaba reference

## Integrate an Alibaba bucket with Postput

### Create it with config files
Copy/paste the [`custom-alibaba.json` ](custom-alibaba.json) config file into the [`data/storage/custom`](https://github.com/postput/api/tree/master/data/storage/custom) directory, then change secret values accordingly.

### Create it with the admin

Access http://localhost:2002

### Create it with postgresql

````sql
INSERT INTO "Storage" ( name, uuid, "isDefault", "typeId", data, config, "creationDate", "updatedOn" ) VALUES ('my_customs_alibaba_files', '60f3799e-0eb5-4785-b566-99bab123ba1d', true, 1, '{}', '{ "custom": { "region": "oss-eu-central-1", "accessKeyId": "mqsodfjnumqsojdfnmqsdnfm", "accessKeySecret": "dsfmkjnqsmdjfnmqlskdnfmkljqnsdflkj", "bucket": "testpostput" }, "allowUpload": true, "urls": ["http://localhost:2003/", "https://www.my-other-domain.com"] }', NOW(), NOW())
````
