
## Setting up a Reverse Proxy

### Problem Statement

Setup a Reverse Proxy on port 8080 that interfaces two different remote APIs.

- http://localhost:8080/emojis -> https://api.github.com/emojis
- http://localhost:8080/transit -> https://transit.land/api/v1

### Solution
 1. Enable mod_ssl load module.
```
LoadModule  ssl_module modules/mod_ssl.so
  ```
 2. Add SSLProxyEngine configuration
```
SSLProxyEngine On
  ```

 3. Add reverse proxy & reverse proxy pass configurations.
```
ProxyPass /emojis https://api.github.com/emojis
ProxyPassReverse /emojis https://api.github.com/emojis

ProxyPass /transit https://transit.land/api/v1
ProxyPassReverse /transit https://transit.land/api/v1
  ```

### Set ups instructions
Follow these instructions to setup a basic Apache Web Server instance running locally & test reverse proxy. You will need Docker Compose to get the base version up and running.

1. Get your development environment set up. Open up a terminal, `cd` into the Directory
containing the `docker-compose.yml` file and type:
  ```
  $ docker-compose build --no-cache web && docker-compose up web
  ```
2. Verify it works correctly by running below command.
  ```
$ curl -v http://localhost:8080
  ```
3. Replace the existing httpd.conf file with one available at below location:
```
https://github.com/dj005/reverse-proxy/blob/master/httpd.conf
```
4. Restart docker instance.

5. Run below commands to validate.
  - `curl -v http://localhost:8080/emojis` which will return exactly the same response as https://api.github.com/emojis
  - `curl -v http://localhost:8080/transit` which will return exactly the same response as https://transit.land/api/v1
  - `curl -v http://localhost:8080/transit/changesets` which will return exactly the same response as https://transit.land/api/v1/changesets
