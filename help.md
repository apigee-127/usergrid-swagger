# Having Problems with YAML?  Here are some resources to help!

## Wikipedia:


[http://en.wikipedia.org/wiki/YAML](http://en.wikipedia.org/wiki/YAML)

> YAML (/ˈjæməl/, rhymes with camel) is a human-readable data serialization format that takes concepts from programming languages such as C, Perl, and Python, and ideas from XML and the data format of electronic mail (RFC 2822). 


[http://www.yaml.org/start.html](http://www.yaml.org/start.html)

```yaml
# Comments in YAML look like this.
# Make sure you have the swagger: 2 at the top
swagger: 2.0
# The info section has information about your API
info:
# Strings should be "quoted" with double-quotes
  version: "0.0.1"
  title: "Twitter search example"
  description: "Twitter search example"
  termsOfService: "http://apigee.com/about/terms"
# Optional Contact Information:
  contact:
    name: "Apigee 127"
    url: "https://github.com/apigee-127"
  license:
    name: "MIT"
    url: "http://opensource.org/licenses/MIT"
# Optional host, used for hitting the implementation of the API
host: "localhost"
# Optional Basepath of the API
basePath: "/"
# Schemes: http, https, ws, wss
schemes:
  - "http"
  - "https"
# Specify MIME Types for message payload formats - consumes (request) and produces (resposne)
consumes:
  - "application/json"
produces:
  - "application/json"
# x-volos-resources is an Apigee extension that specifies the configuration template for Volos.js services
#   that are referenced by specific resources and operations.
# This section is configuration only.  If the resources are not referenced on any resource/operation 
#   then they will not be used
x-volos-resources:
# The following section configures caching using Volos.js.  
# - 'cache' is the name of the provider that must be used to reference this configuration for a specific resource/operation
# - 'provider' specifies the provider type within Volos.js that will be used.  
#     Options are:
#     - "volos-cache-memory"
#     - "volos-cache-apigee"
#     - "volos-cache-redis"
# - 'options' specifies the options to configure the cache provider.  Options may be specific to the type of provider.
  cache:
    provider: "volos-cache-memory"
    options:
      - "name"
      -
        ttl: 10000
# The following section configures Quota using Volos.js.  
# - 'quota' is the name of the provider that must be used to reference this configuration for a specific resource/operation
# - 'provider' specifies the provider type within Volos.js that will be used.  
#     Options are:
#     - "volos-cache-memory"
#     - "volos-cache-apigee"
#     - "volos-cache-redis"
# - 'options' specifies the options to configure the cache provider.  Options may be specific to the type of provider.
  quota:
    provider: "volos-quota-memory"
    options:
      -
        timeUnit: "minute"
        interval: 1
        allow: 2
  oauth2:
    provider: "volos-oauth-apigee"
    options:
      -
        NOTE: "This section is replaced by app.js."
    validGrantTypes:
      - "client_credentials"
      - "authorization_code"
      - "implicit_grant"
      - "password"
    passwordCheck: "passwordCheck"
    paths:
      note: "these will be placed in the paths section"
      authorize: "/authorize"
      token: "/accessToken"
      invalidate: "/invalidate"
      refresh: "/refresh"
paths:
  /twitter:
    x-swagger-router-controller: "twitter"
    x-volos-authorizations:
      oauth2: []
    x-volos-apply:
      cache: []
      quota: []
    get:
      description: "Returns the results of a search of Twitter"
      summary: "Returns the results of a search of Twitter"
      operationId: "search"
      produces:
        - "application/json"
      parameters:
        -
          name: "search"
          in: "query"
          description: "The Query to pass to Twitter"
          required: true
          type: "string"
      responses:
        200:
          description: "Twitter search response"
          schema:
            $ref: "#/definitions/TwitterSearchResponse"
        default:
          description: "Error payload"
          schema:
            $ref: "#/definitions/ErrorModel"
definitions:
  TwitterSearchResponse:
    required:
      - "name"
    properties:
      name:
        type: "string"
      tag:
        type: "string"
  ErrorModel:
    required:
      - "code"
      - "message"
    properties:
      code:
        type: "integer"
        format: "int32"
      message:
        type: "string"

```
