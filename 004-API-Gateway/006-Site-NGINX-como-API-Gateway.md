

# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# Git

git status
git add .
git commit -m "Módulo 4 - API Gateway. Aula 06 Para saber mais: API Gateway."
eval $(ssh-agent -s)
ssh-add /home/fernando/.ssh/chave-debian10-github
git push
git status



# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
#  NGINX

https://www.nginx.com/blog/deploying-nginx-plus-as-an-api-gateway-part-1/

<https://www.nginx.com/blog/deploying-nginx-plus-as-an-api-gateway-part-1/>

Resources > Blog > Tech

# Deploying NGINX as an API Gateway, Part 1

Liam Crilly of F5
Sr Director, Product ManagementJanuary 20, 2021

This is the first blog post in our series on deploying NGINX Open Source and NGINX Plus as an API gateway:

    This post provides detailed configuration instructions for several use cases. Originally published in 2018, it has been updated to reflect current best practice for API configuration, using nested location blocks to route requests, instead of rewrite rules.
    Part 2 extends those use cases and looks at a range of safeguards that can be applied to protect and secure backend API services in production.
    Part 3 explains how to deploy NGINX Open Source and NGINX Plus as an API gateway for gRPC services.

Note: Except as noted, all information in this post applies to both NGINX Open Source and NGINX Plus. For ease of reading, the rest of the blog refers simply to “NGINX”.

At the heart of modern application architectures is the HTTP API. HTTP enables applications to be built rapidly and maintained easily. The HTTP API provides a common interface, regardless of the scale of the application, from a single‑purpose microservice to an all‑encompassing monolith. By using HTTP, the advancements in web application delivery that support hyperscale Internet properties can also be used to provide reliable and high‑performance API delivery.

For an excellent introduction to the importance of API gateways for microservices applications, see Building Microservices: Using an API Gateway on our blog.

As the leading high‑performance, lightweight reverse proxy and load balancer, NGINX has the advanced HTTP processing capabilities needed for handling API traffic. This makes NGINX the ideal platform with which to build an API gateway. In this blog post we describe a number of common API gateway use cases and show how to configure NGINX to handle them in a way that is efficient, scalable, and easy to maintain. We describe a complete configuration, which can form the basis of a production deployment.
Introducing the Warehouse API

The primary function of the API gateway is to provide a single, consistent entry point for multiple APIs, regardless of how they are implemented or deployed at the backend. Not all APIs are microservices applications. Our API gateway needs to manage existing APIs, monoliths, and applications undergoing a partial transition to microservices.

In this blog post we refer to a hypothetical API for inventory management, the “Warehouse API”. We use sample configuration code to illustrate different use cases. The Warehouse API is a RESTful API that consumes JSON requests and produces JSON responses. The use of JSON is not, however, a limitation or requirement of NGINX when deployed as an API gateway; NGINX is agnostic to the architectural style and data formats used by the APIs themselves.

The Warehouse API is implemented as a collection of discrete microservices and published as a single API. The inventory and pricing resources are implemented as separate services and deployed to different backends. So the API’s path structure is:

api
└── warehouse
    ├── inventory
    └── pricing

As an example, to query the current warehouse inventory, a client application makes an HTTP GET request to /api/warehouse/inventory.
API gateway architecture for multiple applications
Organizing the NGINX Configuration

One advantage of using NGINX as an API gateway is that it can perform that role while simultaneously acting as a reverse proxy, load balancer, and web server for existing HTTP traffic. If NGINX is already part of your application delivery stack then it is generally unnecessary to deploy a separate API gateway. However, some of the default behavior expected of an API gateway differs from that expected for browser‑based traffic. For that reason we separate the API gateway configuration from any existing (or future) configuration for browser‑based traffic.

To achieve this separation, we create a configuration layout that supports a multi‑purpose NGINX instance, and provides a convenient structure for automating configuration deployment through CI/CD pipelines. The resulting directory structure under /etc/nginx looks like this.

etc/
└── nginx/
    ├── api_conf.d/ ………………………………… Subdirectory for per-API configuration
    │   └── warehouse_api.conf …… Definition and policy of the Warehouse API
    ├── api_backends.conf ………………… The backend services (upstreams)
    ├── api_gateway.conf …………………… Top-level configuration for the API gateway server
    ├── api_json_errors.conf ………… HTTP error responses in JSON format
    ├── conf.d/
    │   ├── ...
    │   └── existing_apps.conf
    └── nginx.conf

The directories and filenames for all API gateway configuration are prefixed with api_. Each of these files and directories enables a different feature or capability of the API gateway as explained in detail below. The warehouse_api.conf file is a generic stand‑in for the configuration files discussed below that define the Warehouse API in different ways.
Defining the Top-Level API Gateway

All NGINX configuration starts with the main configuration file, nginx.conf. To read in the API gateway configuration, we add an include directive in the http block in nginx.conf that references the file containing the gateway configuration, api_gateway.conf (line 28 just below). Note that the default nginx.conf file uses an include directive to pull in browser‑based HTTP configuration from the conf.d subdirectory (line 29). This blog post makes extensive use of the include directive to aid readability and to enable automation of some parts of the configuration.

~~~~BASH
    include /etc/nginx/api_gateway.conf; # All API gateway configuration

    include /etc/nginx/conf.d/*.conf;    # Regular web traffic
~~~~

view raw
nginx.conf hosted with ❤ by GitHub

The api_gateway.conf file defines the virtual server that exposes NGINX as an API gateway to clients. This configuration exposes all of the APIs published by the API gateway at a single entry point, https://api.example.com/ (line 9), protected by TLS as configured on lines 12 through 17. Notice that this configuration is purely HTTPS – there is no plaintext HTTP listener. We expect API clients to know the correct entry point and to make HTTPS connections by default.

This configuration is intended to be static – the details of individual APIs and their backend services are specified in the files referenced by the include directive on line 20. Lines 23 through 26 deal with error handling, and are discussed in Responding to Errors below.

~~~~CONF
include api_backends.conf;
include api_keys.conf;

server {
    access_log /var/log/nginx/api_access.log main; # Each API may also log to a 
                                                   # separate file

    listen 443 ssl;
    server_name api.example.com;

    # TLS config
    ssl_certificate      /etc/ssl/certs/api.example.com.crt;
    ssl_certificate_key  /etc/ssl/private/api.example.com.key;
    ssl_session_cache    shared:SSL:10m;
    ssl_session_timeout  5m;
    ssl_ciphers          HIGH:!aNULL:!MD5;
    ssl_protocols        TLSv1.2 TLSv1.3;

    # API definitions, one per file
    include api_conf.d/*.conf;

    # Error responses
    error_page 404 = @400;         # Treat invalid paths as bad requests
    proxy_intercept_errors on;     # Do not send backend errors to client
    include api_json_errors.conf;  # API client-friendly JSON errors
    default_type application/json; # If no content-type, assume JSON

}
~~~~

view raw
api_gateway.conf hosted with ❤ by GitHub


## Single-Service vs. Microservice API Backends

Some APIs may be implemented at a single backend, although we normally expect there to be more than one, for resilience or load balancing reasons. With microservices APIs, we define individual backends for each service; together they function as the complete API. Here, our Warehouse API is deployed as two separate services, each with multiple backends.

~~~~CONF
upstream warehouse_inventory {
    zone inventory_service 64k;
    server 10.0.0.1:80;
    server 10.0.0.2:80;
    server 10.0.0.3:80;
}

upstream warehouse_pricing {
    zone pricing_service 64k;
    server 10.0.0.7:80;
    server 10.0.0.8:80;
    server 10.0.0.9:80;

}
~~~~

view raw
api_backends.conf hosted with ❤ by GitHub

All of the backend API services, for all of the APIs published by the API gateway, are defined in api_backends.conf. Here we use multiple IP address‑port pairs in each upstream block to indicate where the API code is deployed, but hostnames can also be used. NGINX Plus subscribers can also take advantage of dynamic DNS load balancing to have new backends added to the runtime configuration automatically.
Defining the Warehouse API

The Warehouse API is defined by a number of location blocks in a nested configuration, as illustrated by the following example. The outer location block (/api/warehouse) identifies the base path, under which nested locations specify the valid URIs that get routed to the backend API services. Using an outer block enables us to define common policies that apply to the entire API (in this example, the logging configuration on line 6).

~~~~CONF
# Warehouse API
#
location /api/warehouse/ {
    # Policy configuration here (authentication, rate limiting, logging...)
    #
    access_log /var/log/nginx/warehouse_api.log main;

    # URI routing
    #
    location /api/warehouse/inventory {
        proxy_pass http://warehouse_inventory;
    }

    location /api/warehouse/pricing {
        proxy_pass http://warehouse_pricing;
    }

    return 404; # Catch-all

}
~~~~

view raw
warehouse_api_simple.conf hosted with ❤ by GitHub

NGINX has a highly efficient and flexible system for matching the request URI to a section of the configuration. The order of the location directives is not important – the most specific match is chosen. Here, the nested locations on lines 10 and 14 define two URIs that are more specific than the outer location block; the proxy_pass directive in each nested block routes requests to the appropriate upstream group. Policy configuration is inherited from the outer location unless there is a need to provide a more specific policy for certain URIs.

Any URIs that do not match one of the nested locations are handled by the outer location, which includes a catch‑all directive (line 18) that returns the response 404 (Not Found) for all invalid URIs.
Choosing Broad vs. Precise Definition for APIs

There are two approaches to API definition – broad and precise. The most suitable approach for each API depends on the API’s security requirements and whether it is desirable for the backend services to handle invalid URIs.

In warehouse_api_simple.conf above, we use the broad approach for the Warehouse API, defining URI prefixes on lines 10 and 14 such that a URI that begins with one of the prefixes is proxied to the appropriate backend service. With this broad, prefix‑based location matching, API requests to the following URIs are all valid:

/api/warehouse/inventory
/api/warehouse/inventory/
/api/warehouse/inventory/foo
/api/warehouse/inventoryfoo
/api/warehouse/inventoryfoo/bar/

If the only consideration is proxying each request to the correct backend service, the broad approach provides the fastest processing and most compact configuration. On the other hand, a more precise approach enables the API gateway to understand the API’s full URI space by explicitly defining the URI path for each available API resource. Taking the precise approach, the following configuration for URI routing in the Warehouse API uses a combination of exact matching (=) and regular expressions (~) to define each and every valid URI.

~~~~CONF
    # URI routing
    #
    location = /api/warehouse/inventory { # Complete inventory
        proxy_pass http://warehouse_inventory;
    }

    location ~ ^/api/warehouse/inventory/shelf/[^/]+$ { # Shelf inventory
        proxy_pass http://warehouse_inventory;
    }

    location ~ ^/api/warehouse/inventory/shelf/[^/]+/box/[^/]+$ { # Box on shelf
        proxy_pass http://warehouse_inventory;
    }

    location ~ ^/api/warehouse/pricing/[^/]+$ { # Price for specific item
        proxy_pass http://warehouse_pricing;

    }
~~~~

view raw
warehouse_api_precise.conf hosted with ❤ by GitHub

This configuration is more verbose, but more accurately describes the resources implemented by the backend services. This has the advantage of protecting the backend services from malformed client requests, at the cost of some small additional overhead for regular expression matching. With this configuration in place, NGINX accepts some URIs and rejects others as invalid:
Valid URIs 	  	Invalid URIs
/api/warehouse/inventory 	  	/api/warehouse/inventory/
/api/warehouse/inventory/shelf/foo 	  	/api/warehouse/inventoryfoo
/api/warehouse/inventory/shelf/foo/box/bar 	  	/api/warehouse/inventory/shelf
/api/warehouse/inventory/shelf/-/box/- 	  	/api/warehouse/inventory/shelf/foo/bar
/api/warehouse/pricing/baz 	  	/api/warehouse/pricing
  	  	/api/warehouse/pricing/baz/pub

Using a precise API definition enables existing API documentation formats to drive the configuration of the API gateway. It is possible to automate the NGINX API definitions from the OpenAPI Specification (formerly called Swagger). A sample script for this purpose is provided among the Gists for this blog post.
Rewriting Client Requests to Handle Breaking Changes

As APIs evolve, it’s sometimes necessary to make changes that break strict backward compatibility and require clients to be updated. One such example is when an API resource is renamed or moved. Unlike a web browser, an API gateway cannot send its clients a redirect (code 301 (Moved Permanently)) naming the new location. Fortunately, when it’s impractical to modify API clients, we can rewrite client requests on the fly.

In the following example, we use the same broad approach as in warehouse_api_simple.conf above, but in this case the configuration is replacing a previous version of the Warehouse API where the pricing service was implemented as part of the inventory service. The rewrite directive on line 3 converts requests to the old pricing resource into requests to the new pricing service.

~~~~CONF
# Rewrite rules
#
rewrite ^/api/warehouse/inventory/item/price/(.*)  /api/warehouse/pricing/$1;

# Warehouse API
#
location /api/warehouse/ {
    # Policy configuration here (authentication, rate limiting, logging...)
    #
    access_log /var/log/nginx/warehouse_api.log main;

    # URI routing
    #
    location /api/warehouse/inventory {
        proxy_pass http://warehouse_inventory;
    }

    location /api/warehouse/pricing {
        proxy_pass http://warehouse_pricing;
    }

    return 404; # Catch-all

}
~~~~
view raw
warehouse_api_rewrites.conf hosted with ❤ by GitHub
Responding to Errors

One of the key differences between HTTP APIs and browser‑based traffic is how errors are communicated to the client. When NGINX is deployed as an API gateway, we configure it to return errors in a way that best suits the API clients.

The top‑level API gateway configuration includes a section that defines how to handle error responses.

~~~~CONF
    # Error responses
    error_page 404 = @400;         # Treat invalid paths as bad requests
    proxy_intercept_errors on;     # Do not send backend errors to client
    include api_json_errors.conf;  # API client-friendly JSON errors

    default_type application/json; # If no content-type, assume JSON
~~~~

view raw
api_gateway.conf hosted with ❤ by GitHub

The error_page directive on line 23 specifies that when a request does not match any of the API definitions, NGINX returns the 400 (Bad Request) error instead of the default 404 (Not Found) error. This (optional) behavior requires that API clients make requests only to the valid URIs included in the API documentation, and prevents unauthorized clients from discovering the URI structure of the APIs published through the API gateway.

Line 24 refers to errors generated by the backend services themselves. Unhandled exceptions may contain stack traces or other sensitive data that we don’t want to be sent to the client. This configuration adds a further level of protection by sending a standardized error response to the client.

The complete list of standardized error responses is defined in a separate configuration file referenced by the include directive on line 25, the first few lines of which are shown below. This file can be modified if an error format other than JSON is preferred, with the default_type value on line 26 of api_gateway.conf changed to match. You can also have a separate include directive in each API’s policy section to reference a different file of error responses which override the global responses.

~~~~CONF
error_page 400 = @400;
location @400 { return 400 '{"status":400,"message":"Bad request"}\n'; }

error_page 401 = @401;
location @401 { return 401 '{"status":401,"message":"Unauthorized"}\n'; }

error_page 403 = @403;
location @403 { return 403 '{"status":403,"message":"Forbidden"}\n'; }

error_page 404 = @404;

location @404 { return 404 '{"status":404,"message":"Resource not found"}\n'; }
~~~~

view raw
api_json_errors.conf hosted with ❤ by GitHub

With this configuration in place, a client request for an invalid URI receives the following response.

~~~~bash
$ curl -i https://api.example.com/foo
HTTP/1.1 400 Bad Request
Server: nginx/1.19.5
Content-Type: application/json
Content-Length: 39
Connection: keep-alive

{"status":400,"message":"Bad request"}
~~~~


## Implementing Authentication

It is unusual to publish APIs without some form of authentication to protect them. NGINX offers several approaches for protecting APIs and authenticating API clients. For information about approaches that also apply to regular HTTP requests, see the documentation for IP address‑based access control lists (ACLs), digital certificate authentication, and HTTP Basic authentication. Here, we focus on API‑specific authentication methods.
API Key Authentication

API keys are a shared secret known by the client and the API gateway. An API key is essentially a long and complex password issued to the API client as a long‑term credential. Creating API keys is simple – just encode a random number as in this example.

~~~~bash
$ openssl rand -base64 18
7B5zIqmRGXmrJTFmKa99vcit
~~~~

On line 2 of the top‑level API gateway configuration file, api_gateway.conf, we include a file called api_keys.conf, which contains an API key for each API client, identified by the client’s name or other description. Here are the contents of that file:

~~~~CONF
map $http_apikey $api_client_name {
    default "";

    "7B5zIqmRGXmrJTFmKa99vcit" "client_one";
    "QzVV6y1EmQFbbxOfRCwyJs35" "client_two";
    "mGcjH8Fv6U9y3BVF9H3Ypb9T" "client_six";

}
~~~~

view raw
api_keys.conf hosted with ❤ by GitHub

The API keys are defined within a map block. The map directive takes two parameters. The first defines where to find the API key, in this case in the apikey HTTP header of the client request as captured in the $http_apikey variable. The second parameter creates a new variable ($api_client_name) and sets it to the value of the second parameter on the line where the first parameter matches the key.

For example, when a client presents the API key 7B5zIqmRGXmrJTFmKa99vcit, the $api_client_name variable is set to client_one. This variable can be used to check for authenticated clients and included in log entries for more detailed auditing. The format of the map block is simple and easy to integrate into automation workflows that generate the api_keys.conf file from an existing credential store.

Here we enable API key authentication by amending the “broad” configuration (warehouse_api_simple.conf) to include an auth_request directive in the policy section that delegates the authentication decision to a specified location.

~~~~CONF
# Warehouse API
#
location /api/warehouse/ {
    # Policy configuration here (authentication, rate limiting, logging...)
    #
    access_log /var/log/nginx/warehouse_api.log main;
    auth_request /_validate_apikey;

    # URI routing
    #
    location /api/warehouse/inventory {
        proxy_pass http://warehouse_inventory;
    }

    location /api/warehouse/pricing {
        proxy_pass http://warehouse_pricing;
    }

    return 404; # Catch-all

}
~~~~

view raw
warehouse_api_apikeys.conf hosted with ❤ by GitHub

With the auth_request directive (line 7) we can, for example, have authentication handled by an external authentication server such as OAuth 2.0 token introspection. In this example we instead add the logic for validating API keys to the top‑level API gateway configuration file, in the form of the following location block called /_validate_apikey.

~~~~CONF
    # API key validation
    location = /_validate_apikey {
        internal;

        if ($http_apikey = "") {
            return 401; # Unauthorized
        }
        if ($api_client_name = "") {
            return 403; # Forbidden
        }

        return 204; # OK (no content)

    }
~~~~

view raw
api_gateway_apikey.conf hosted with ❤ by GitHub

The internal directive on line 30 means that this location cannot be accessed directly by external clients (only by auth_request). Clients are expected to present their API key in the apikey HTTP header. If this header is missing or empty (line 32), we send a 401 (Unauthorized) response to tell the client that authentication is required. Line 35 handles the case where the API key does not match any of the keys in the map block – in which case the default parameter on line 2 of api_keys.conf sets $api_client_name to an empty string – and we send a 403 (Forbidden) response to tell the client that authentication failed. If neither of those conditions match, the API key is valid and the location returns a 204 (No Content) response.

With this configuration in place, the Warehouse API now implements API key authentication.

~~~~bash
$ curl https://api.example.com/api/warehouse/pricing/item001
{"status":401,"message":"Unauthorized"}
$ curl -H "apikey: thisIsInvalid" https://api.example.com/api/warehouse/pricing/item001
{"status":403,"message":"Forbidden"}
$ curl -H "apikey: 7B5zIqmRGXmrJTFmKa99vcit" https://api.example.com/api/warehouse/pricing/item001
{"sku":"item001","price":179.99}
~~~~


## JWT Authentication

JSON Web Tokens (JWTs) are increasingly used for API authentication. Native JWT support is exclusive to NGINX Plus, enabling validation of JWTs as described in Authenticating API Clients with JWT and NGINX Plus on our blog. For a sample implementation, see Controlling Access to Specific Methods in Part 2.
Summary

This first blog in a series details a complete solution for deploying NGINX Open Source and NGINX Plus as an API gateway. The complete set of files discussed in this blog can be reviewed and downloaded from our GitHub Gist repo.

Check out the other posts in this series:

    Part 2 explores more advanced use cases for protecting backend services from malicious or badly behaved clients.
    Part 3 explains how to deploy NGINX as an API gateway for gRPC services.

To try NGINX Plus, start your free 30-day trial today or contact us to discuss your use cases.
Tags
API gateway
Hero image
Deploying NGINX as an API Gateway

This free eBook shows you how to deploy NGINX as an API gateway
Download Now


About The Author
Liam Crilly

Sr Director, Product Management

More Blogs By Liam Crilly














# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# Comentários

- Usar um bloco externo nos permite definir políticas comuns que se aplicam a toda a API (neste exemplo, a configuração de log na linha 6).

~~~~CONF
# Warehouse API
#
location /api/warehouse/ {
    # Policy configuration here (authentication, rate limiting, logging...)
    #
    access_log /var/log/nginx/warehouse_api.log main;

    # URI routing
    #
    location /api/warehouse/inventory {
        proxy_pass http://warehouse_inventory;
    }

    location /api/warehouse/pricing {
        proxy_pass http://warehouse_pricing;
    }

    return 404; # Catch-all

}
~~~~







## PENDENTE
- Continua em:
    https://www.nginx.com/blog/deploying-nginx-plus-as-an-api-gateway-part-1/
    "Responding to Errors"