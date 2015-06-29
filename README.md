Juseppe - Jenkins Update Site Embedded for Plugin Publishing
====================

## Getting started

### 1. Checkout & Build 

With maven just run `mvn package`, and you will find jar in `target/juseppe.jar`
 
### 2. Run to serve plugins

To run server with file watching in current directory (not the dir where jar located!)

`java -jar juseppe.jar`

### 3. Configure 

You can define system properties to override default behaviour:

- `update.center.plugins.dir` - where the plugins are. Searches only `*.hpi`. Defaults to *current working dir*
- `update.center.saveto.dir` - where to save generated json file. Defaults to *current working dir*
- `update.center.json.name` - name of generated json file. Defaults to `update-center.json`
- `update.center.private.key` - path of private key (must be used in pair with cert). Defaults to *uc.key*
- `update.center.certificate` - path of certificate (must be used in pair with private key prop). Defaults to *uc.crt* 
- `update.center.baseurl` - url to prepend for plugins download link in json. Defaults to `http://localhost:8080`
- `jetty.port` - port for file server. Defaults to `8080`

Example: 

`java -jar -Dupdate.center.saveto.dir=/tmp/update/ juseppe.jar`

## How to connect Jenkins

Site can be added with help of: 
    
- [UpdateSites Manager plugin](https://wiki.jenkins-ci.org/display/JENKINS/UpdateSites+Manager+plugin)

## How to launch with help of docker

Build image  

`docker build -t juseppe:source .`

Next, run it with mounted plugins folder as volume. Remember to set `JUSEPPE_BASE_URI` env var

`docker run --name juseppe -v /your/plugins/dir/:/juseppe/plugins/ -e JUSEPPE_BASE_URI=http://my.company.com -p 80:8080 juseppe:source`

Then it will be available on `http://dockerhost:80/update-center.json`

Certificate can be copied from json `signature.certificates[0]` with additional 
`-----BEGIN CERTIFICATE-----` and `-----END CERTIFICATE-----` with line breaks and without quotes.
