
<a href="https://www.postput.com"><img src="https://cdn-storage.speaky.com/image5/4b34b792-ca51-41d3-8ae9-1c2a21d914a0.png" title="Postput" alt="Postput"></a>  
  
<!-- [![Postput](https://cdn-storage.speaky.com/image5/4b34b792-ca51-41d3-8ae9-1c2a21d914a0.png)](https://www.postput.com) -->  
  
***https://www.postput.com***  
  
# Postput: Cloud native storage operator   
  
  > Upload, download and perform operations on the fly on your files   


  ## About
  
Postput is a simple microservice that sits between your file storage and your end-users.   
Its primary function is to [perform various operations on your files](#demo).   
It can also be used to simplify the way you download/upload files with your different storage providers.  
  
  # Demo
  
![Recordit GIF](https://cdn-storage.speaky.com/image2/5e8e5b0d-e92c-4243-bc4f-1ce62fd1c4d1.gif)  

## Supported storage provider  
  
  - In memory (for testing and develepment only. Fils will be lost after
   server restart)  
   - Filesystem Allows you to store your files in your own hard drive  
   - Amazon S3  https://aws.amazon.com/fr/s3
   - Google cloud storage  (GCS) 
   - Spaces (DigitalOcean) https://www.digitalocean.com/products/spaces/ 
   - Openstack used by OVH object storage  (https://www.ovh.com/fr/public-cloud/object-storage/ ) 
   - Azure https://azure.microsoft.com/fr-fr/services/storage/
   - Scaleway (https://www.scaleway.com/fr/object-storage/)
   - backblaze https://www.backblaze.com/b2/cloud-storage.html
   - Webfolder
   - Proxy
        
     
                 Webfolder : upload
    not allowed on webfolder.   Proxy: Will only serve as a transformer 
    All s3 compliant storages

  
 
  
## Table of Contents (Optional)  
  
> If your `README` has a lot of info, section headers might be nice.  
  
- [Installation](#installation)  
- [Features](#features)  
- [Contributing](#contributing)  
- [Team](#team)  
- [FAQ](#faq)  
- [Support](#support)  
- [License](#license)  
  
  
---  
  

## TLDR;
wget docker-compose
docker-coompose up

curl to post 
+ curl to fetch  
  
## Installation  
  
  ### Preriquisites
  Docker
  
### 1. Clone  

Clone this repo to your local machine
```shell
git clone https://github.com/fvcproductions/SOMEREPO
```
  

 ###  2. Launch stack
 
Postput store information about cloud providers inside Postgresqsl.

Fortunately, it has become pretty easy to launch any kind of stack with docker.
````shell 
docker-compose up 
````  
  
- For all the possible languages that support syntax highlithing on GitHub (which is basically all of them), refer <a href="https://github.com/github/linguist/blob/master/lib/linguist/languages.yml" target="_blank">here</a>.  
  
---  
  
## Usage

### upload file(s)

### download file(s)

### Add a storage provider


## Documentation
  
  see anotated config for all providers

- Going into more detail on code and technologies used  
- I utilized this nifty <a href="https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet" target="_blank">Markdown Cheatsheet</a> for this sample `README`.  
  
---  
  

  
---  
  
  
---  
  
## FAQ  
  
- **How do I do *specifically* so and so?**  
 - No problem! Just do this.  
