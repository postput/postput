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
  
### 1. Launch the API: 
```shell
docker run -p 2000:3000 postput/api:latest
```

### 2. Upload any image by providing its URL

```shell
curl -X POST http://localhost:2000/my_memory_files/\?url=https://i2-prod.mirror.co.uk/incoming/article14334083.ece/ALTERNATES/s810/3_Beautiful-girl-with-a-gentle-smile.jpg\&name-override=my-image.jpg
```

### 3. Resize, blur, rotate, round and optimize this image on the fly. Operations are applied in the order they appear in the request.

Copy/paste this URL in your favourite webbrowser:
http://localhost:2000/my_memory_files/my-image.jpg?resize=300,300&blur=5&rotate=90&mask=elipse&format=webp

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

- You have a lot amount a data in a FTP server that you suddenly have to expose via an API.
> Postput can integrate with an [FTP server](doc/ftp) and serve its files smoothly.
>
- You start a new project and don't want to waste time building your own HTTP server for storing your profile pictures. You're still not sure if you're going to use [s3](doc/s3), [GCS](doc/gcs), [backblaze](doc/backblaze) or your own [filesystem](doc/filesystem) as a storage provider.

> When still in development, create a [filesystem storage](doc/filesystem) storage. Later on, when you'll know what storage provider to use, integrate it with postput. Your code won't change because all storage providers follow the same API.
> You can keep those 2 storage providers active at the same time. You can actually create as many storage providers as you like with Postput.

- You're happy with the storage provider that you use. You're not confident about storing your super secret credentials with this (amazing) third party solution but you still want to find a quick solution for resizing and optimizing your files.
> Create a [proxy](doc/proxy) or [webfolder](doc/webfolder) storage with postput. No credentials are needed for those kind of storage but your files need to be publicly available.

# Table of Contents 

- [Operations Available](#operations-available) 
- [Supported Storage Provider](#supported-storage-provider) 
- [Install](#install) 
- [Configuration](#configuration)
- [Add your own provider](#add-your-own-provider)
- [Roadmap](#roadmap)
- [Credits](#credits)       
 ---    
                 
# Operations Available


- [Blur image](#blur)
- [Brighten image](#brightness)
- [Saturate image](#saturation)
- [Hue shift image](#hue)
- [Greyscale](#greyscale)
- [Tint](#tint)
- [ColorSpace](#colorspace)
- [Flip-x image](#flip-x)
- [Flip-y image](#flip-y)
- [Negate image](#negative)
- [Resize image](#resize)
- [Crop image](#crop)
- [Rotate image](#rotate)
- [Mask image](#mask)
- [Format image](#format)
- [Extract audio/video](#extract)
- [Resize video](#resize-video)
- [Crop video](#crop-video)
- [Face detection](#face-detection)

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
            <td id="blur">blur</td>
            <td> Perform a Gaussian blur on the image</td>
            <td>?blur=5</td>
        </tr>
        <tr>
            <td id="brightness">brightness</td>
            <td> Brighten the image by specified value. Must be between 0 and 1.</td>
            <td>?brightness=0.6</td>
        </tr>
        <tr>
            <td id="saturation">saturation</td>
            <td> Apply a saturation on the image. Value must be between 0 and 1.</td>
            <td>?saturation=0.6</td>
        </tr>
        <tr>
            <td id="hue">hue</td>
            <td> Perform a hue shift on the image. Value must be an angle between 0 and 360</td>
            <td>?hue=180</td>
        </tr>
        <tr>
            <td id="greyscale">greyscale</td>
            <td> Transform the image into a white/black</td>
            <td>?greyscale=true</td>
        </tr>
        <tr>
            <td id="tint">tint</td>
            <td> Apply an rgba color mask on top of the image.</td>
            <td>?tint=210,205,15,0.2</td>
        </tr>
        <tr>
            <td id="colorspace">colorspace</td>
            <td> Set the output colourspace. Possible values: srgb, rgb, cmyk, lab, b-w ...</td>
            <td>?colorspace=cmyk</td>
        </tr>
        <tr>
            <td id="flip-x">flip-x</td>
            <td> Flip the image around the X axis.</td>
            <td>?flip-x=true</td>
        </tr>
        <tr>
            <td id="flip-y">flip-y</td>
            <td> Flip the image around the Y axis.</td>
            <td>?flip-y=true</td>
        </tr>
        <tr>
            <td id="negative">negate</td>
            <td> Produce the negative of the image.</td>
            <td>?negate=true</td>
        </tr>
        <tr>
            <td id="resize">resize</td>
            <td>Resize the image accoring to specified value</td>
            <td>?resize=200 <br/>?resize=10,20</td>
        </tr>
        <tr>
            <td id="crop">crop</td>
            <td>Take a portion of an image. Must be used in combination with crop-left(optional), crop-top(optional), crop-width(required) and crop-height(required). Extracted area must strictly be within the original image.</td>
            <td>?crop=true&crop-left=30&crop-top=30&crop-width=200&crop-height=200<br/>?crop=true&crop-width=200&crop-height=200</td>
        </tr>A
        <tr>
            <td id="rotate">Rotate</td>
            <td>Rotate the image to the degree specified</td>
            <td>?rotate=90</td>
        </tr>
        <tr>
            <td id="mask">mask</td>
            <td>Apply a mask on the image. Currently, only `elipse` and `corner` mask are supported.</td>
            <td>?mask=elipse <br />?mask=corner&corner-radius=10</td>
        </tr>
        <tr>
            <td id="format">format</td>
            <td>Change the format of the image. supported values: jpeg, jpg, png, gif, webp, tiff, raw.</td>
            <td>?format=webp</td>
        </tr>
        <tr>
            <td id="extract">Extract audio/video</td>
            <td>Extract a portion of an audio/video file format. You'll have to provide a start time and a duration. Both values are expressed in second.</td>
            <td>?extract=5,10 // this will extract a 10 seconds length portion starting at the 5th second.</td>
        </tr>
        <tr>
            <td id="resize-video">resize-video</td>
            <td>Resize a video to specified width/height.</td>
            <td>?resize-video=900x600</td>
        </tr>
        <tr>
            <td id="crop-video">crop-video</td>
            <td>Crop a video to a certain portion. Format of value follow the FFMPEG specification: width:height:offset-width:offset-height</td>
            <td>?crop-video=300:200:50:30 // Will extract a 300x200 rectangle from the video starting a the 50 width offset and 30 height offset.</td>
        </tr>
        <tr>
            <td id="face-detection">face</td>
            <td>Crop the image to fit the most relevant face. Use it with 'face-pad' to zoom out off the face.</td>
            <td>?face=true&face-pad=1.5</td>
        </tr>
    </tbody>
    
    
</table>

# Supported Storage Provider    
 ### See [provider reference](https://github.com/postput/api/blob/master/data/provider/custom/custom.json.dist)
 
- [Amazon S3 + All compliant storages](doc/s3)
- [Minio](doc/minio)
- [Google cloud storage (GCS)](doc/gcs)   
- [Spaces (DigitalOcean)](doc/spaces)
- [Openstack](/doc/openstack) 
- [Azure](doc/azure)
- [Backblaze](doc/backblaze)
- [Scaleway](doc/scaleway)
- [IBM](doc/ibm)
- [Alibaba](doc/alibaba)
- [FTP](doc/ftp)
- [In memory](doc/memory)
- [Filesystem](doc/filesystem)
- [Webfolder](doc/webfolder)
- [Proxy](doc/proxy)
  
# Install
Before running and using Postput, you'll have to tell it what provider(s) to use. It is very easy to do so.

Postput is designed to sync every json file it finds in the [data/provider/custom](https://github.com/postput/api/blob/master/data/provider/custom) directory with the in-memory sqlite database every time it starts.

You'll have to create a json file that looks like [this one](https://github.com/postput/api/blob/master/data/provider/custom/custom.json.dist) in the [data/provider/custom](https://github.com/postput/api/blob/master/data/provider/custom) directory


## Locally

### Clone this git repo with its dependencies.
```shell
git clone --recursive git@github.com:postput/api.git
git clone --recursive https://github.com/postput/api.git
```

### Start the service
```shell
cd api
docker-compose up
```

By default, a memory provider and a filesystem provider are created for you.

## On production

### With docker
You can create a docker image based on the [api image]([https://hub.docker.com/repository/docker/postput/api](https://hub.docker.com/repository/docker/postput/api)) (Image built with [this docker file]([https://github.com/postput/api/blob/master/Dockerfile](https://github.com/postput/api/blob/master/Dockerfile)))
Don't forget to include your own provider info and to delete those you don't want anymore:
```docker
FROM postput/api
COPY my-provider.json ./data/provider/custom
RUN rm ./data/provider/default.json
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

The API can be easily configured using environment variables and provider specific config.

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
            <td> List of urls that will be appended at the end of each provider configuration.</td>
            <td>["http://localhost:2000/", "https://www.my-other-url.com"]</td>
        </tr>
        <tr>
            <td>ALLOW_UPLOAD</td>
            <td>Specify if upload is allowed to every providers. This config can be overridden by provider specify configuration.</td>
            <td>["http://localhost:2000/", "https://www.my-other-url.com"]</td>
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
            <td>Specify if upload is allowed for that provider. It takes precedence over the global configuration</td>
            <td>true</td>
        </tr>
        <tr>
            <td>urls</td>
            <td>Provider specific urls.</td>
            <td>["http://localhost:3000/"]</td>
        </tr>
        <tr>
            <td>custom</td>
            <td>Provider specific config, see <a href="#supported-storage-provider ">Supported Storage Provider </a></td>
            <td>{}</td>
        </tr>
    </tbody>
</table>

# Add your own provider

You can add your own provider very easily. 
All you have to do is to add your own specific Provider implementation by creating a class [here](https://github.com/postput/api/tree/master/src/provider/implementation). 
The system is designed to load every file in that directory and to consider each of them as Provider implementation.
Your class will have to implement the [Provider interface](https://github.com/postput/api/blob/master/src/provider/interface.ts). If you don't know what to do, just get inspired by other implementations like [this one](https://github.com/postput/api/blob/master/src/provider/implementation/s3.ts), [this one](https://github.com/postput/api/blob/master/src/provider/implementation/filesystem.ts) or [this one](https://github.com/postput/api/blob/master/src/provider/implementation/ftp.ts)


# Debug

You may want to run the API service in your computer to be able to see what's happening under the hood.

### Preriquisites:
* [node.js](#[https://nodejs.org/en/](https://nodejs.org/en/)) >= 10

## Launch the API
Start the API.

### Start the API:

```shell
cd api
npm i
npm start
```

# Roadmap

By end of November 2019:

* Implement Helm chart for Kubernetes cluster: Have a look at [the pull request](https://github.com/helm/charts/pull/19158)


By end of December 2019:
*   Increase test coverage
*   Face detection ( [face-api?]([https://github.com/justadudewhohacks/face-api.js/](https://github.com/justadudewhohacks/face-api.js/))  [opencv?](#[https://github.com/peterbraden/node-opencv](https://github.com/peterbraden/node-opencv)))


# Credits

Most operations ([resize](#resize), [rotate](#rotate), [blur](#blur), [mask](#mask)...) are done by the [sharp library]([https://github.com/lovell/sharp](https://github.com/lovell/sharp)). 

Cloud storage integration is done with the help of [pkgcloud]([https://github.com/pkgcloud/pkgcloud] (https://github.com/pkgcloud/pkgcloud)), [aws-s3]([https://github.com/aws/aws-sdk-js](https://github.com/aws/aws-sdk-js)) and [backblaze-b2](https://www.npmjs.com/package/backblaze-b2).

For the specific memory storage, I use [memfs](#[https://github.com/streamich/memfs](https://github.com/streamich/memfs))

[Sequelize](#[https://github.com/sequelize/sequelize](https://github.com/sequelize/sequelize)) helps me with database communication