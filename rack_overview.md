## Rack 

[Rack](http://rack.github.io/) provides a minimal interface between webservers supporting Ruby and Ruby frameworks. All Ruby web frameworks are built on top of Rack. 

RubyOnRails has the concept of Middleware which is just a small Rack app that processes a HTTP request/response.


## Create the simplest Rack app possible.  
Will show the current time.

This lambda that will return an array with three entries, the entries will be:  
1. A HTTP Status Code, 200 is the code for OK.  
2. A Ruby hash that contains the HTTP Response Header fields.  
3. Any Object that responds to the 'each' method.  __This will be the body of the page you will see in the browser__
4. It will run the WEBrick server on port 1234 listening for Requests. 

##### Create a Rack app. 

```touch rack/simplest.rb```  

Add the below code:  

```
require 'rubygems'
require 'rack'
                                                           
app =  lambda{ |env|  [200, {'Content-Type' => 'text/plain'}, ["The time is #{Time.now}"] ] }
                                             
Rack::Handler::WEBrick.run app, Port: 1234
```

##### Run the Rack app.
 
```
ruby rack/simplest.rb
```

##### Send a HTTP Request with curl.  
```
curl -v http://localhost:1234
```

##### Send a HTTP Request with Chrome.  

http://localhost:1234

Open the Chrome Inspector and go to the Network tab. Refresh and look at the HTTP Request/Response.



## Show the HTTP Request Headers

```
touch rack/show_http_headers.rb
```

Add this code. 

```
require 'rubygems'
require 'rack'

	# Return the contents of the env passed to this rack app.                                          
app = lambda do |env|
  # HTTP Request Header                                                                            
  response = []
  response << "Method: #{env['REQUEST_METHOD']}"
  response << "Request URI: #{env['REQUEST_URI']}"
  response << "Query String: #{env['QUERY_STRING']}"
  response << "Request Path: #{env['REQUEST_PATH']}"
  response << "Accept: #{env['HTTP_ACCEPT']}"
  response << "Accept Language: #{env['HTTP_ACCEPT_LANGUAGE']}"
  response << "User Agent: #{env['HTTP_USER_AGENT']}"
  response << "URL Scheme: #{env['rack.url_scheme']}\n"

  [200, { }, [response.join("\n")] ]
end

Rack::Handler::WEBrick.run app, Port: 1234

```

```
curl -i http://localhost:1234/this/is/my/path?name=jack&age=33
```

In Chrome, Safari, Firefox, etc.  
http://localhost:1234/this%20is%20it/but/not%20the%20/end?name=jack&age=33)

### Lab
Write a simple Rack app that will fill in the blanks in the below text from URL params. The filled in text will be returned by the HTTP Response. The words in __bold__ are going to be replaced by URL params.

"__Who__ arrested __name__ for __crime__ earlier this week, and according to a report by the __source__, the incident stemmed from a dispute over a __object__."

Example:  
"__Police__ arrested __a Main Street resident__ for __illegal possession of a handgun and ammunition__ earlier this week, and according to a report __in the Lowell Sun__, the incident stemmed from a dispute over a __frozen cheese pizza__."

http://localhost:1234/?who=The Police&name=a Main Street resident&crime=possesion of a handgun&source=in the Lowell Sun&object=frozen cheess pizza


##### Question what are the %20 in the response?### Demo

### Lab 
Write a Rack app, like the above, but also create a method named 'get'. 

This method named 'get' will take two arguments. The first argument will be the resource path and the second argument will be a string to send back in the body of the HTTP Response.

For example:

```get '/stooges/curly_howard.html', "<html>...<body><h3>Curly Howard</h3></body></html>"```

```get '/stooges/show?location="Boston"&date="1-10-1944"', "<html>...<body><h3>Place: Boston</h3><h5>Date: 1-10-1944</body></html>"```

### Lab
Change the get method to have a second argument of the form class#method.


```get '/stooges/3', 'stooge#show' ```

This will:
1. Create a stooge object, an instance of a Stooge class. 
2. Call the show method on this object passing the 3, let call 3 the index, from the path as an argument.
3. Use this index, integer 3, to lookup a stooge in an Array of Stooges!

                                                                      

