![Docker Cloud Build Status](https://img.shields.io/docker/cloud/build/postput/api)
![Docker Pulls](https://img.shields.io/docker/pulls/postput/api)
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
![Maintenance](https://img.shields.io/maintenance/yes/2021)

<img src="https://cdn-storage.speaky.com/image5/4b34b792-ca51-41d3-8ae9-1c2a21d914a0.png" title="Postput" alt="Postput"> 
    
<!-- [![Postput](https://cdn-storage.speaky.com/image5/4b34b792-ca51-41d3-8ae9-1c2a21d914a0.png)](https://www.postput.com) -->    
    
 # Postput: An object storage to rule them all!     
    
>Postput is a simple webserver that can serve any kind of files, wherever they are. 
It abstracts the complexity of [storage providers](#supported-storage-provider) to help you upload, download and [operate](#operations-available) on your files.     

# Demo
![Recordit GIF](https://cdn-storage.speaky.com/image2/5e8e5b0d-e92c-4243-bc4f-1ce62fd1c4d1.gif)    
  
# TL;DR
  
### 1. Launch the full stack: 
```shell
wget https://raw.githubusercontent.com/postput/postput/master/docker-compose.yaml -O postput-docker-compose.yaml && \
docker-compose  -f postput-docker-compose.yaml up
```

### 2. Upload any image by providing its URL

```shell
curl -X POST http://localhost:2000/my_memory_files/\?url=https://i2-prod.mirror.co.uk/incoming/article14334083.ece/ALTERNATES/s810/3_Beautiful-girl-with-a-gentle-smile.jpg\&name-override=my-image.jpg
```

### 3. Resize, blur, rotate, round and optimize this image on the fly. Operations are applied in the order they appear in the request.

Copy/paste this URL in your favourite webbrowser:

```shell 
http://localhost:2000/my_memory_files/my-image.jpg?resize=300,300&blur=5&rotate=90&mask=elipse&format=webp 
```

### 4. Create [your custom storage](#supported-storage-provider) at: http://localhost:2002

# Why this project?

This project is the result of some observations:

* It is difficult to choose what storage provider to use for your assets and user-generated files like profile pictures.  
> S3 bucket? GCS? Backblaze? Scaleway? Openstack? Nowadays a lot of solutions exists to host your files. What if you don't want to take your decision now and just want to code what matters to you now?  
  
* It is difficult to switch from one storage provider to another.  
>When dealing with large amount of requests, object storage can rapidly become very costly on some providers.   
Unfortunately, switching from one provider to another includes rewriting some code and effort to match the new API.  
  
* Applying operations on files are too expensive.  
>[Lambda functions](https://aws.amazon.com/fr/blogs/compute/resize-images-on-the-fly-with-amazon-s3-aws-lambda-and-amazon-api-gateway/), [Imagekit](https://imagekit.io), [Imgx](https://www.imgix.com/), [Cloudinary](https://cloudinary.com/)... All these options works well but are too expensive and some of them only support hosting images.   
Even if they use performant GPU technologies and out of the box CDN support, pricing is still hard to swallow when you know that more than 95% of your asset traffic can be cached by a free CDN (like cloudflare).   
You may want to spend your money on a wiser solution instead.  


Postput simplify object storage by providing a unified API tu upload, download and operate functions on files. It is a free product that you can install on your own computer for development and on production later on when you're ready!

 
# Use cases

- You already have an S3 bucket where you store a lot of profile pictures/Avatars. You want to resize and optimize the format of those profile pictures to reduce your bandwith usage and accelerate download speed.
> [Integrate your bucket with postput](doc/s3) and apply the [resize](#resize) and [webp format](#format) filter on the query.

- You know about serverless solutions like Lambda but these solutions turn out to be too costly.
> Postput can integrate with a lot of storage providers and will only cost you the price of the server lease. 

- You start a new project and don't want to waste time building your own HTTP server for storing your profile pictures. You're still not sure if you're going to use [s3](doc/s3), [GCS](doc/gcs), [backblaze](doc/backblaze) or your own [filesystem](doc/filesystem) as a storage provider.

> When still in development, create a [filesystem storage](doc/filesystem) storage. Later on, when you'll know what storage provider to use, integrate it with postput. Your code won't change because all storage providers follow the same API.
> You can even keep those 2 storage providers active at the same time. You can actually create as many storage providers as you like with Postput.

- You're happy with the storage provider that you use. You're not confident about storing your super secret credentials with this (amazing) third party solution but you still want to find a quick solution for resizing and optimizing your files.
> Create a [proxy](doc/proxy) or [webfolder](doc/webfolder) storage with postput. No credentials are needed for those kind of storage but your files need to be publicly available.

# Table of Contents 

- [Operations Available](#operations-available) 
- [Supported Storage Provider](#supported-storage-provider) 
- [Install](#install) 
- [Configuration](#configuration)
- [Roadmap](#roadmap)
- [Credits](#credits)       
 ---    
 
# Operations Available
- [Resize image](#resize)
- [Rotate image](#rotate)
- [Blur image](#blur)
- [Mask image](#mask)
- [Format image](#format)
- [Extract audio/video](#extract)

Operations are applied one after another. Keep in mind that **order may matters** depending on which operations you do!


<table>
    <thead>
        <tr>
            <th>Query parameter</th>
            <th>Description</th>
            <th>Example</th>
        </tr>
    </thead>
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
        <tr>
            <td id="extract">extract</td>
            <td>Extract a portion of an audio/video file format. You'll have to provide a start time and a duration. Both values are expressed in second.</td>
            <td>?extract=5,10 // this will extract a 10 seconds length portion starting at the 5th second.</td>
        </tr>
    </tbody>
</table>

# Supported Storage Provider    
 ### See [storage reference](https://github.com/postput/api/blob/a56e5a92c2addd7a092c5f2c743ab72ff17697c5/data/storage/custom/custom.json.dist)
 
- [Amazon S3](doc/s3)
- [Google cloud storage (GCS)](doc/gcs)   
- [Spaces (DigitalOcean)](doc/spaces)
- [Openstack](/doc/openstack) 
- [Azure](doc/azure)
- [Backblaze](doc/backblaze)
- [Scaleway](doc/scaleway)
- [FTP](doc/ftp)
- [In memory](doc/memory)
- [Filesystem](doc/filesystem)
- [Webfolder](doc/webfolder)
- [Proxy](doc/proxy)
- [All s3 compliant storages](#all-s3-compliant-storages)
  
# Install
Before running and using Postput, you'll have to tell him what kind of storage to use. It is very easy to do so.

Postput is designed to sync every json file it finds in the [data/storage](https://github.com/postput/api/tree/master/data/storage) directory with the database every time it starts.

This is the preferable method if you plan to use postput on production because it ensure a consistent storage info upon restart even if you decide to modify/reset your postgresql instance. 

You'll have to create a json file that looks like [this one](https://github.com/postput/api/blob/master/data/storage/custom/custom.json.dist) in the [data/storage](https://github.com/postput/api/tree/master/data/storage/custom) directory


## Locally

### Clone this git repo with its dependencies.
```shell
git clone --recursive git@github.com:postput/postput.git
git clone --recursive https://github.com/postput/postput.git
```

### Start the whole stack : API + backend + frontend 
```shell
cd postput
docker-compose up
```

By default, a memory storage and a filesystem storage are created for you.

## On production

### With docker
You can create a docker image based on the [api image]([https://hub.docker.com/repository/docker/postput/api](https://hub.docker.com/repository/docker/postput/api)) (Image built with [this docker file]([https://github.com/postput/api/blob/master/Dockerfile](https://github.com/postput/api/blob/master/Dockerfile)))
Don't forget to include your own storage info and to delet those you don't want anymore:
```docker
FROM postput/api
COPY my-storage.json ./data/storage/custom
RUN rm ./data/storage/default.json
```

### With Kubernetes: Helm
A Helm chart will soon be available in the [incubator](https://github.com/helm/charts/tree/master/incubator). Have a look at the [pull request](https://github.com/helm/charts/pull/19158)
Unfortunately, the process to get it integrated may be quite long so, until then, I will integrate charts directly into github repositories under the [`chart`](https://github.com/postput/api/tree/master/chart) directory.
Read the [Chart config](https://github.com/postput/api/tree/master/chart) for more info
```shell
git clone git@github.com:postput/api.git
cd chart
helm install . --name=my-release
```



# Configuration

The API can be easily configured using environment variables and storage specific config.

### Global configuration

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

# Debug

You may want to run each service independently in your computer to be able to see what's happening under the hood.
Good news: each microservice can be started independently.

### Preriquisites:
* [node.js](#[https://nodejs.org/en/](https://nodejs.org/en/)) >= 10
*  Postgresql database up and running. You can spawn one very easily thanks to the docker-compose file provided at the root of this repository.

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


# Roadmap

By end of November 2019:

* Implement Helm chart for Kubernetes cluster: Have a look at [the pull request](https://github.com/helm/charts/pull/19158)


By end of December 2019:
*   Implement Webhooks capabilities
*   Increase test coverage
*   Face detection ( [face-api?]([https://github.com/justadudewhohacks/face-api.js/](https://github.com/justadudewhohacks/face-api.js/))  [opencv?](#[https://github.com/peterbraden/node-opencv](https://github.com/peterbraden/node-opencv)))



# Credits

Most operations ([resize](#resize), [rotate](#rotate), [blur](#blur), [mask](#mask)...) are done by the [sharp library]([https://github.com/lovell/sharp](https://github.com/lovell/sharp)). 

Cloud storage integration is done with the help of [pkgcloud]([https://github.com/pkgcloud/pkgcloud] (https://github.com/pkgcloud/pkgcloud)), [aws-s3]([https://github.com/aws/aws-sdk-js](https://github.com/aws/aws-sdk-js)) and [backblaze-b2](https://www.npmjs.com/package/backblaze-b2).

For the specific memory storage, I use [memfs](#[https://github.com/streamich/memfs](https://github.com/streamich/memfs))

[Sequelize](#[https://github.com/sequelize/sequelize](https://github.com/sequelize/sequelize)) helps me with database communication