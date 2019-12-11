# IBM reference

## Integrate an IBM bucket with Postput

### Create it with config files
Copy/paste the [`custom-ibm.json` ](custom-ibm.json) config file into the [`data/storage/custom`](https://github.com/postput/api/tree/master/data/storage/custom) directory, then change secret values accordingly.

### Create it with the admin

Access http://localhost:2002

### Create it with postgresql

````sql
INSERT INTO "Storage" ( name, uuid, "isDefault", "typeId", data, config, "creationDate", "updatedOn" ) VALUES ('my_customs_ibm_files', '9504906b-b6c0-4e32-a3bb-77514e35219f', true, 1, '{}', '{ "custom": { "endpoint": "https://s3.eu-de.cloud-object-storage.appdomain.cloud", "apiKeyId": "piqsdjfùpoijqs*ùdfpokqs*$podfkùpqoskdfùopkqsdf", "ibmAuthEndpoint": "https://iam.cloud.ibm.com/identity/token", "serviceInstanceId": "crn:v1:bluemix:public:cloud-object-storage:global:a/qùspdofjùqspodjfùosqjkdfùmlksqdmùlfk:sdfsdf-sdf-qsdf-aa0e-qsdfsqdfqsdf::", "bucket": "testpostput" }, "allowUpload": true, "urls": ["http://localhost:2003/", "https://www.my-other-domain.com"] }', NOW(), NOW())
````
