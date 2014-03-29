#JSON Transients

This plugin wraps the [request](https://github.com/mikeal/request) module with a timeout cache layer. Inspired by the WordPress Transients API. This NodeJS flavor is a bit simpler and only deals with JSON. It stores the data in an object `var transients = {};`. It's just 2 methods right now, a setter and and a getter.

The idea here, is that slow changing data only needs checked so often. For example, if you are doing a server side call to your twitter feed and passing that JSON to a template, you probably only need to get that data every 5 min, and not every time someone visits the page. This module allows you to specifiy that behavior by returning data from memory until the cache expires, then automatically updating the cache only if it's expired.

### setTransient

Creates a transient object.

	// name - string
	// url - string 
	// cacheFor - int (seconds)
	setTransient( name, url, cacheFor );
	
Returns false if the JSON request fails.


#### Example
	
	var transients = require('transients');
	transients.setTransient( 'ipinfo', 'http://ipinfo.io/json', 3600 );

Sets a transient named 'ipinfo' with data from 'http://ipinfo.io/json' new calls to http://ipinfo.io/json are only made if the last request for data is more than 3600 seconds old.


### getTransient

Retrieves a transient name, makes a new request if the old on has expired, returns cached/fresh Object.

	// name - string 
	// callback (optional)
	getTransient( name, callback( data ) )

Returns the transients data if no callback is provided. 

Returns false if the transient was never set.


#### Example

	var transients = require('transients');
	var ipinfo = transients.getTransient('ipinfo');
	// -- or --
	transients.getTransient( 'ipinfo', function( data ) {
		console.log( data );
	});
