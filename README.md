![Docker Cloud Build Status](https://img.shields.io/docker/cloud/build/postput/api)
![Docker Pulls](https://img.shields.io/docker/pulls/postput/api)
![GitHub All Releases](https://img.shields.io/github/downloads/postput/api/total)
![GitHub package.json version](https://img.shields.io/github/package-json/v/postput/api)
![GitHub top language](https://img.shields.io/github/languages/top/postput/api)
![GitHub language count](https://img.shields.io/github/languages/count/postput/api)
![GitHub code size in bytes](https://img.shields.io/github/languages/code-size/postput/api)
![Code Climate maintainability](https://img.shields.io/codeclimate/maintainability-percentage/postput/api)
![Code Climate coverage](https://img.shields.io/codeclimate/coverage/postput/api)
![Code Climate technical debt](https://img.shields.io/codeclimate/tech-debt/postput/api)
![GitHub](https://img.shields.io/github/license/postput/postput)
![Snyk Vulnerabilities for GitHub Repo](https://img.shields.io/snyk/vulnerabilities/github/postput/api)
![GitHub search hit counter](https://img.shields.io/github/search/postput/api/class)
![GitHub issues](https://img.shields.io/github/issues/postput/api)
![GitHub pull requests](https://img.shields.io/github/issues-pr/postput/api)
![GitHub last commit](https://img.shields.io/github/last-commit/postput/api)
<img src="https://cdn-storage.speaky.com/image5/4b34b792-ca51-41d3-8ae9-1c2a21d914a0.png" title="Postput" alt="Postput"> 
    
<!-- [![Postput](https://cdn-storage.speaky.com/image5/4b34b792-ca51-41d3-8ae9-1c2a21d914a0.png)](https://www.postput.com) -->    
    
 # Postput: Cloud native storage operator     
    
> Upload, download and perform operations on the fly on your files   

Postput is a microservice that sits between your file storage and your end-users.     
Its primary function is to [perform various operations on your files](#operations-available).     
It can also be used to simplify the way you download/upload files with [various storage providers](#supported-storage-provider).    
  
   # Table of Contents 
 - [Operations Available](#operations-available) - [Install](#install) - [Run it locally](#run-it-locally) - [How to](#how-to) - [Roadmap](#roadmap)- [Credits](#credits) 
    
 ---    
  
  # Demo
  ![Recordit GIF](https://cdn-storage.speaky.com/image2/5e8e5b0d-e92c-4243-bc4f-1ce62fd1c4d1.gif)    
 
 
 # TL;DR
  
  ### 1. Launch the full stack: 
```shell
wget https://raw.githubusercontent.com/postput/postput/master/docker-compose.yaml -O postput-docker-compose.yaml && \
docker-compose  -f postput-docker-compose.yaml up
```

###  2. Upload any kind of file

```shell
curl -F 'data=@postput-docker-compose.yaml' http://localhost:2000/my_memory_files?name-override=docker-compose.yaml
```

###  2. Download the file you've just uploaded

```shell
curl http://localhost:2000/my_memory_files/docker-compose.yaml
```

### 3. Upload an image by providing its URL

```shell
curl -X POST http://localhost:2000/my_memory_files/\?url=https://i2-prod.mirror.co.uk/incoming/article14334083.ece/ALTERNATES/s810/3_Beautiful-girl-with-a-gentle-smile.jpg\&name-override=my-image.jpg
```

### 4. Resize, blur, rotate, round and optimize this image on the fly. Operations are applied in the order they appear in the request.

```shell
curl http://localhost:2000/my_memory_files/my-image.jpg\?resize=300,300&blur=5&rotate=90&mask=elipse&format=webp
```

### 5. Create [your custom storage](#supported-storage-provider) at: http://localhost:2002

# Operations Available
- [Resize image](#resize)
- [Rotate image](#rotate)
- [Blur image](#blur)
- [Mask image](#mask)
- [Format image](#format)

Operations are applied one after another. Keep in mind that **order may matters** depending on which operations you do!


<table>
    <thead>
        <tr>
            <th>Query parameter</th>
            <th>Description</th>
            <th>Example</th>
        </tr>
    </thead>
        <tfoot>
        <tr>
            <th>Query parameter</th>
            <th>Description</th>
            <th>Example</th>
        </tr>
    </tfoot>
    <tbody>
        <tr>
            <td id="resize">resize</td>
            <td>Resize the image accoring to specified value</td>
            <td>?resize=200 <br/>?resize=10,20</td>
        </tr>
        <tr>
            <td id="rotate">Rotate</td>
            <td>Rotate the image to the degree specified</td>
            <td>?rotate=90</td>
        </tr>
        <tr>
            <td id="blur">blur</td>
            <td> Perform a Gaussian blur on the image</td>
            <td>?blur=5</td>
        </tr>
        <tr>
            <td id="mask">mask</td>
            <td>Apply a mask on the image. Currently, only elipse mask is supported.</td>
            <td>?mask=elipse</td>
        </tr>
        <tr>
            <td id="format">format</td>
            <td>Change the format of the image. supported values: jpeg, jpg, png, gif, webp, tiff, raw.</td>
            <td>?format=webp</td>
        </tr>
    </tbody>
</table>

# Supported Storage Provider    
 ### See [storage reference](https://github.com/postput/api/blob/a56e5a92c2addd7a092c5f2c743ab72ff17697c5/data/storage/custom/custom.json.dist)
 
- [Amazon S3](#amazon-s3) Amazon S3
- [Google cloud storage (GCS)](#google-cloud-storage-gcs)   
- [Spaces (DigitalOcean)](#spaces-digitalocean)
- [Openstack](#openstack) 
- [Azure](#azure)
- [Backblaze](#backblaze)
- [Scaleway](#scaleway)
- [In memory](#in-memory)
- [Filesystem](#filesystem)
- [Webfolder](#webfolder)
- [Proxy](#proxy)
- [All s3 compliant storages](#all-s3-compliant-storages)
          
          
## Amazon (S3)
 
```json
{  
  "custom": {  
  "keyId": "AKXXXXXXXXXXXXXXXXXX",  
  "key": "XCKlXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",  
  "container": "mycontainer",  
  "region": "us-east-1"  
  },  
  "allowUpload": true,  
  "urls": ["http://localhost:2000/"]  
}
```
## Google Cloud Storage (GCS) 

```json
{  
  "custom": {  
  "keyFile": {  
  "type": "service_account",  
  "project_id": "xxxxxxx-xxxx-xxx",  
  "private_key_id": "2bxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",  
  "private_key": "-----BEGIN PRIVATE KEY-----\nXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n-----END PRIVATE KEY-----\n",  
  "client_email": "xxxxx-xx-xxx@xxxx.gserviceaccount.com",  
  "client_id": "xxxxxxxxxxxxxxxxxxxxx",  
  "auth_uri": "https://accounts.google.com/o/oauth2/auth",  
  "token_uri": "https://accounts.google.com/o/oauth2/token",  
  "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",  
  "client_x509_cert_url": "https://www.googleapis.com/robot/v1/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxgserviceaccount.com"  
  },  
  "container": "mycontainer",  
  "projectId": "xxxxxxxx-xxxx-xxxx"  
  },  
  "allowUpload": true,  
  "urls": ["http://localhost:2000/"]  
}
```

## Spaces (DigitalOcean)

```json
{  
  "custom": {  
  "endpoint": "fra1.digitaloceanspaces.com",  
  "bucket": "mybucket",  
  "accessKeyId": "XXXXXXXXXXXXXXXXXXXX",  
  "secretAccessKey": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"  
  },  
  "allowUpload": true,  
  "urls": ["http://localhost:2000/"]  
}
```

## Openstack 

```json
{  
  "custom": {  
  "username": "xxxxxxxxxxxx",  
  "password": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",  
  "tenantId": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",  
  "region": "xxxx",  
  "authUrl": "https://auth.cloud.ovh.net/",  
  "version": "v2.0",  
  "container": "mycontainer"  
  },  
  "allowUpload": true,  
  "urls": ["http://localhost:2000/"]  
}
```

## Azure
```json
{
        "custom": {
          "storageAccount": "my-storage-account",
          "storageAccessKey": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx==",
          "container": "xxxxxxxxx"
        },
        "allowUpload": true,
        "urls": ["http://localhost:2000/", "https://www.my-other-domain.com"]
      }
```
## Backblaze  

```json
 {
        "custom": {
          "applicationKeyId": "qsd5f46qs54fd654q6sdf46q5s",
          "applicationKey": "fq6sd5f65q4sg654sf6g54sfd65g4s6",
          "bucketName": "mybucketname",
          "bucketId": "sqd6f56sqd4f65s4df654sq6fd4"
        },
        "allowUpload": true,
        "urls": ["http://localhost:2000/", "https://www.my-other-domain.com"]
      }
```

## Scaleway  

```json
{  
  "custom": {  
  "endpoint": "s3.fr-par.scw.cloud",  
  "bucket": "mybucket",  
  "accessKeyId": "XXXXXXXXXXXXXXXXXXXXX",  
  "secretAccessKey": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"  
  },  
  "allowUpload": true,  
  "urls": ["http://localhost:2000/"]  
}
```

## In memory  
 Allows you to store your file in memory. All files stores will be lost upon restart. It's mostly usefull for testing and demo purpose  

```json
{  
  "custom": {},  
  "allowUpload": true,  
  "urls": ["http://localhost:2000/"]  
}
```

## Filesystem
 
 No configuration is required. By default, files will be stored on the public folder.
```json
{  
  "custom": {},  
  "allowUpload": true,  
  "urls": ["http://localhost:2000/"]  
}
```

 If your files are located in a specific folder, you can specify its path, be it relative or absolute.

Relative path (the root is the root of the api project)
```json
{  
  "custom": {  
  "path": "public/default"  
  },  
  "allowUpload": true,  
  "urls": ["http://localhost:2000/"]  
}
```

Absolute path
```json
{  
  "custom": {  
  "path": "/tmp"  
  },  
  "allowUpload": true,  
  "urls": ["http://localhost:2000/"]  
}
```



## Webfolder  

```diff
! Upload is not supported for that kind of storage
```

```json
{  
  "custom": {  
  "method": "GET",  
  "uri": "https://www.yoururl.com",  
  "qs": {  
  "my-first-query-string": "myvalue",  
  "my-second-query-string": "my-second-value"  
  }  
 },   
  "urls": ["http://localhost:2000/"]  
}
```

## Proxy 
```diff
! Upload is not supported for that kind of storage
```
It is not a storage provider in itself. A Proxy allows you to connect to any file that is addressable through a publicly-available URL.
```json
{  
  "custom": {  
  "allowedHosts": ["storage.speaky.com"]  
 },  
  "urls": ["http://localhost:2000/"]  
}
``` 
 Webfolder : upload not allowed on webfolder.   Proxy: Will only serve as a transformer    All s3 compliant storages  
  
## All s3 compliant storages
Postput can integrate with every type of s3 storage provider. Wise readers may have noticed that Scaleway and Spaces are actually S3 compliant and have thus the same config pattern. If you want to integrate another kind of s3 complient storage, just adapt the endpoint accordingly.

```json
{  
  "custom": {  
  "endpoint": "my.endpoint.com",  
  "bucket": "mybucket",  
  "accessKeyId": "XXXXXXXXXXXXXXXXXXXXX",  
  "secretAccessKey": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"  
  },  
  "allowUpload": true,  
  "urls": ["http://localhost:2000/"]  
}
```

# Install

Clone this git repo with its dependencies.
```shell
git clone --recursive git@github.com:postput/postput.git
git clone --recursive https://github.com/postput/postput.git
```

Start the whole stack (API + admin backend and frontend) 
```shell
cd postput
docker-compose up
```

# Run it locally

You may want to run each service independently in your computer to be able to see what's happenning under the hood.
Good news: each microservice can be started independently.

### Preriquisites:
- [node.js](#[https://nodejs.org/en/](https://nodejs.org/en/)) > 8
-  Postgresql database up and running. You can spawn one very easily thanks to the docker-compose file provided at the root of this repository.


```shell
docker-compose up postput_db
```

## Launch the API and the admin backend
Once started, you can start the API and the admin-backend. 
You can customize database connexion parameters with environment variables.

### Start the API:

```shell
cd api
npm i
export POSTGRESQL_PORT=5555
export LISTEN_PORT=1999
npm start
```

### Start the admin-backend

```shell
cd admin-backend
npm i
export POSTGRESQL_PORT=5555
export LISTEN_PORT=1998
npm start
```

## Launch the admin frontend

Once the admin backend is started, you can start the admin frontend that allows you to create, read, update and delete storages (CRUD). The frontend is designed to communicate with the backend so you must configure it so that it will hit the admin-backend on the port it is listening : [1998](#start-the-admin-backend) in this case.

admin-frontend
```shell
cd admin-frontend
npm i
ng serve -c debug
```
The debug configuration is a profile that will make the frontend hit the port 1998 so it will just work out of the box.
Unfortunately, angular does not play well with environment variable so if you want/have to modify this port, you'll have to modify the debug profile or create another one. 
The file to modify is located at : [https://raw.githubusercontent.com/postput/admin-frontend/master/src/environments/environment.prod.ts](https://raw.githubusercontent.com/postput/admin-frontend/master/src/environments/environment.debug.ts)


# Configuration

## Api configuration
### Global configuration
#### Environment variable 

<table>
    <thead>
        <tr>
            <th>Environment variable</th>
            <th>Description</th>
            <th>Example</th>
        </tr>
    </thead>
        <tfoot>
        <tr>
            <th>Environment variable</th>
            <th>Description</th>
            <th>Example</th>
        </tr>
    </tfoot>
    <tbody>
        <tr>
            <td>ENV</td>
            <td>set the ENV for your nodejs</td>
            <td>development | production</td>
        </tr>
        <tr>
            <td>LISTEN_PORT</td>
            <td>The port listened</td>
            <td>2000</td>
        </tr>
        <tr>
            <td>URLS</td>
            <td> List of urls that will be appened at the end of each storage configuration.</td>
            <td>["http://localhost:2000/", "https://www.my-other-url.com"]</td>
        </tr>
        <tr>
            <td>ALLOW_UPLOAD</td>
            <td>Specify if upload is allowed to every storages. This config can be overriden by storage specify configuration.</td>
            <td>["http://localhost:2000/", "https://www.my-other-url.com"]</td>
        </tr>
        <tr>
            <td>POSTGRESQL_HOST</td>
            <td>Postgresql host</td>
            <td>localhost</td>
        </tr>
        <tr>
            <td>POSTGRESQL_PORT</td>
            <td>Postgresql port</td>
            <td>5432</td>
        </tr>
        <tr>
            <td>POSTGRESQL_USERNAME</td>
            <td>Postgresql username</td>
            <td>postput</td>
        </tr>
        <tr>
            <td>POSTGRESQL_PASSWORD</td>
            <td>Postgresql password</td>
            <td>postput</td>
        </tr>
        <tr>
            <td>POSTGRESQL_DATABASE</td>
            <td>Postgresql database</td>
            <td>postput</td>
        </tr>
                <tr>
            <td>SEQUELIZE_FORCE_SYNC</td>
            <td> <a href="https://sequelize.readthedocs.io/en/latest/api/sequelize/#sync">Force deletion of table if they exists</a> before creating them. You want to disable that on production.</td>
            <td>true</td>
        </tr>
    </tbody>
</table>


### Storage specific configuration
   
   All parameters listed below are config keys that must be put at the root of your storage configuration.
   See [Storage reference]([https://raw.githubusercontent.com/postput/api/master/storage-reference.json](https://raw.githubusercontent.com/postput/api/master/docker-compose.yaml))
   <table>
    <thead>
        <tr>
            <th>Config key</th>
            <th>Description</th>
            <th>Example</th>
        </tr>
    </thead>
        <tfoot>
        <tr>
            <th>Config key</th>
            <th>Description</th>
            <th>Example</th>
        </tr>
    </tfoot>
    <tbody>
        <tr>
            <td>allowUpload</td>
            <td>Specify if upload is allowed for that storage. It takes precedence over the global configuration</td>
            <td>true</td>
        </tr>
        <tr>
            <td>urls</td>
            <td>Storage specific urls.</td>
            <td>["http://localhost:3000/"]</td>
        </tr>
        <tr>
            <td>custom</td>
            <td>Storage specific config, see <a href="#supported-storage-provider ">Supported Storage Provider </a></td>
            <td>{}</td>
        </tr>
    </tbody>
</table>
   
# How to

## How do I create my custom storage provider?

### With fixtures (Recommended)

Postput API is designed to sync every json file it finds in the [data/storage](https://github.com/postput/api/tree/master/data/storage) directory with the database every time it starts.

This is the preferable method if you plan to use postput on production because you'll have consistent storage info upon restart even if you decide to modify/reset your postgresql instance. 

You'll have to create a json file that follow the [storage reference]([https://github.com/postput/api/blob/master/package.json](https://github.com/postput/api/blob/master/storage-reference.json)) in the [data/storage]([https://github.com/postput/api/tree/master/data/storage](https://github.com/postput/api/tree/master/data/storage)) directory


The method you use to create that file is left to you. 
I recommend to create that file using the [secret/mountpath]([https://kubernetes.io/docs/concepts/configuration/secret/#use-cases](https://kubernetes.io/docs/concepts/configuration/secret/#use-cases)) feature if you use [kubernetes]([https://kubernetes.io/](https://kubernetes.io/)).

If you don't use kubernetes, you can still create a docker image based on the [api image]([https://hub.docker.com/repository/docker/postput/api](https://hub.docker.com/repository/docker/postput/api)) (Image built with [this docker file]([https://github.com/postput/api/blob/master/Dockerfile](https://github.com/postput/api/blob/master/Dockerfile)))


Dockefile

```docker
FROM postput/api
COPY my-storage.json ./data/storage/
```

### With Postgresql
The fastest way to create a storage if you're familiar with SQL.
You can use any database tool that supports Postgresql like [DBeaver]([https://dbeaver.io/](https://dbeaver.io/)) or [navicat]([https://www.navicat.com/](https://www.navicat.com/))
There is only 2 tables : Storage and StorageType. the only table that you want to modify is Storage.

### With the admin dashboard
The easiest way to create your storage.
Simply access the [admin frontend]([https://github.com/postput/admin-frontend](https://github.com/postput/admin-frontend)). If you started the project using the docker-compose of the [TL;DR](#tldr) section, it is located at [localhost:2002](http://localhost:2002)

## How do I deploy it on production?

### On Kubernetes With Helm (Recommended)
A Helm chart will soon be available in the [incubator](https://github.com/helm/charts/tree/master/incubator). Have a look at the [pull request](https://github.com/helm/charts/pull/19158)
Unfortunately, the process to get it integrated may be quite long so, until then, I will integrate charts directly into github repositories under the [`chart`](https://github.com/postput/api/tree/master/chart) directory.

#### Install the API
```shell
git clone --recursive git@github.com:postput/api.git
cd api/chart
helm install . --name=my-release
```
#### [Helm api documentation](https://github.com/postput/api/tree/master/chart)
# FAQ

### My files are already stored on amazon S3. Do I have to migrate them if I want to use postput? 
No, postput is not a storage in itself. The only thing you have to do is to tell postput where is your bucket by [creating an S3 storage provider](#how-do-i-create-my-custom-storage-provider) and [provide the right config for amazon s3](#amazon-s3)


# Roadmap

By end of November 2019:

- Implement Helm chart for Kubernetes cluster: Have a look at [the pull request](https://github.com/helm/charts/pull/19158)


By end of December 2019:
-   Implement Webhooks capabilities
-   Increase test coverage
-   Face detection ( [face-api?]([https://github.com/justadudewhohacks/face-api.js/](https://github.com/justadudewhohacks/face-api.js/))  [opencv?](#[https://github.com/peterbraden/node-opencv](https://github.com/peterbraden/node-opencv)))



# Credits

Most operations ([resize](#resize), [rotate](#rotate), [blur](#blur), [mask](#mask)...) are done by the [sharp library]([https://github.com/lovell/sharp](https://github.com/lovell/sharp)). 
Cloud storage integration is done with the help of [pkgcloud]([https://github.com/pkgcloud/pkgcloud] (https://github.com/pkgcloud/pkgcloud)), [aws-s3]([https://github.com/aws/aws-sdk-js](https://github.com/aws/aws-sdk-js)) and [backblaze-b2](https://www.npmjs.com/package/backblaze-b2).
For the specific memory storage, I use [memfs](#[https://github.com/streamich/memfs](https://github.com/streamich/memfs))
[Sequelize](#[https://github.com/sequelize/sequelize](https://github.com/sequelize/sequelize)) helps me with database communication