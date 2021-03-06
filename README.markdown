CasRestClient
===========

A simple HTTP and REST client to interact with services using [CAS](http://www.jasig.org/cas) for authentication. Strongly based on [RestClient](http://github.com/archiloque/rest-client).


### Basic Usage:

The simplest usage is to create an instance of CasRestClient with the credentials for your app and use it every time you need to interact with the cas-protected application.

>require 'rubygems'  
require 'cas_rest_client'    

>client = CasRestClient.new :uri => 'https://some-cas-server.com/tickets', :username => 'user', password => 'pass'  

>client.post 'http://service.using.cas', some_xml, :content_type => :xml  

When the CasRestClient instance is created it'll automatically create a Ticket-Granting Ticket (tgt) and use it during its lifetime. If the credentials provided are not valid, a *RestClient::Request::Unauthorized* exception will be raised.


### Passing custom parameters to CAS
You can pass any custom parameter by adding it to the initialization hash. Doing so, CasRestClient will use these parameters to create the Ticket-Granting Ticket on CAS.

For example, to pass a 'domain' custom parameter just add:
  
>client = CasRestClient.new :uri => 'https://some-cas-server.com/tickets', :username => 'user', password => 'pass', **:domain => "myDomain"**

>client.post 'http://service.using.cas', some_xml, :content_type => :xml 


### Custom parameters for the cas-protected app

Since CasRestClient uses [RestClient](http://github.com/archiloque/rest-client) to make HTTP requests, you can set any parameters that RestClient accepts while interacting with the cas-protected application.

For example, to pass additional HTTP headers:
  
>client = CasRestClient.new :uri => 'https://some-cas-server.com/tickets', :username => 'user', password => 'pass'

>headers = {:content_type => 'text/xml', :user_agent => 'my_app', 'Accept-Language' => 'en_US'}  
>client.post 'http://service.using.cas', some_xml, headers


### Additional options
#### :service => 'some_string' (default to cas-protected app URI)
By default, CasRestClient uses the URI of the cas-protected app as the *service* parameter to create service tickets. To change this just provide the **:service** param: 
  
>client = CasRestClient.new :uri => 'https://some-cas-server.com/tickets', **:service => "http://my_svc.com"**

>client.post 'http://service.using.cas', some_xml, headers

With this example configuration, CasRestClient will always create service tickets for the **http://my_svc.com** service.


#### use_cookie => Boolean (default true)
By default, the first time you make a request to the cas-protected app CasRestClient will save all cookies from the response and give it back in all other requests. This prevents 2 extra requests for every single request: your application won't need to ask CAS for a new service ticket and the cas-protected app won't need to check if the ticket is valid.

If you want to prevent this behaviour, just set **:use_cookies** to false:

>client = CasRestClient.new :uri => 'https://some-cas-server.com/tickets', **:use_cookies => false**

>client.post 'http://service.using.cas', some_xml, headers

### Project info
Written by [Antonio Marques](http://github.com/acmarques) and [Roberto Klein](http://github.com/robertokl). Contributions and feedback are welcomed.