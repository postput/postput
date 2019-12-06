# FTP storage reference

## Integrate an (s)FTP server with Postput

### Create it with config files
Copy/paste the [`custom-ftp.json` ](custom-ftp.json) config file into the [`data/storage/custom`](https://github.com/postput/api/tree/master/data/storage/custom) directory, then change secret values accordingly.

### Create it with the admin

Access http://localhost:2002

### Create it with postgresql

````sql
INSERT INTO "Storage" ( name, uuid, "isDefault", "typeId", data, config, "creationDate", "updatedOn" ) VALUES ('my_customs_3_files', 'fd600d4d-ce30-4940-951d-26aeb12c70bf', true, 1, '{}', '{ "custom": { "host": "localhost", "port": 2222, "username": "foo", "root": "upload", "privateKey": "-----BEGIN RSA PRIVATE KEY-----XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX-----END RSA PRIVATE KEY-----" } , "allowUpload": true, "urls": ["http://localhost:2003/", "https://www.my-other-domain.com"] }', NOW(), NOW())
````
