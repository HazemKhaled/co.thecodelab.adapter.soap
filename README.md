SOAP adapter for Appcelerator Titanium Alloy
==========================
Finally you have full support of SOAP client into Titanium, depend on the power of [SOAP Node.js package](https://www.npmjs.com/package/soap).

This adapter forked from [@Viezel Rest adapter](https://github.com/viezel/napp.alloy.adapter.restapi).

## How To Use

Add the [ti-soap](http://gitt.io/component/soap) commonjs module
```
gittio install soap
```

Download [soap.js](soap.js) to `PROJECT_FOLDER/app/assets/alloy/sync/`

or

use NPM thanks to [appc-npm](https://www.npmjs.com/package/appc-npm):

```
// not working right now
npm install alloy-sync-soap --save
```

Add the following to your model in `PROJECT_FOLDER/app/models/foo.js`.
```javascript
	exports.definition = {	
		config: {
			//"debug": 1, 
			"adapter": {
				"type": "soap",
				"collection_name": "MyCollection",
				"idAttribute": "id"
			},
			"headers": {},
	        	soapMethod: "catalogCategoryLevel",
	        	"parentNode": "news.domestic" //your root node
		},		
		extendModel: function(Model) {		
			_.extend(Model.prototype, {});
			return Model;
		},	
		extendCollection: function(Collection) {		
			_.extend(Collection.prototype, {});
			return Collection;
		}		
	}
```
Create the soap controller in Alloy.js
```javascript
// Set SOAP global objects
Alloy.Globals.soap = {
	client: null,
	session: null,
	create: function( callback ) {

		var SOAP = require( 'soap' );
		SOAP.createClient( "http://example.com/wsdl.xml", function( err, client ) {

			Alloy.Globals.soap.client = client;

			Alloy.Globals.soap.client.login( {
				apiKey: "XXXXXXXX",
				username: "XXXXXXXXXX"
			}, function( err, result ) {
				if( !err ) {
					Alloy.Globals.soap.session = result.loginReturn.$value;
					callback( );
				} else {
					Ti.API.error( "[SOAP API] " + err );
				}
			} );

		} );
	}
};
```

Finally in your index.js, before fetch the first collection you have to create the client
```javascript
Alloy.Globals.soap.create( function( ) {
	// open window or fetch your data
} );
```

# Changelog

**v0.2.2**  
Remove url handling code to fix bug, not needed for SOAP 

**v0.2.1**  
Update appc-npm script (still not working)  
Update depancies and meta in package.js

**v0.2.0** 
init version compitable with ti-soap 0.2.0 build on Node.js SOAP 0.2.0, for from [REST API adapter](https://github.com/viezel/napp.alloy.adapter.restapi)

## Authors

1. [Mads Møller](https://github.com/viezel) - Original adapter that we forked from
2. [Pier Paolo Ramon](https://github.com/yuchi) - Titaniumify the Node.js [SOAP](https://www.npmjs.com/package/soap) package to Titanium module
3. [Hazem Khaled](https://github.com/hazemkhaled) - Code this adapter
4. [Ebrahim 3bmo3ty](https://github.com/e3bmo3ty) - Code this adapter


## License

    The MIT License (MIT)
    
    Copyright (c) 2010-2015

    Permission is hereby granted, free of charge, to any person obtaining a copy
    of this software and associated documentation files (the "Software"), to deal
    in the Software without restriction, including without limitation the rights
    to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
    copies of the Software, and to permit persons to whom the Software is
    furnished to do so, subject to the following conditions:

    The above copyright notice and this permission notice shall be included in
    all copies or substantial portions of the Software.

    THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
    IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
    FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
    AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
    LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
    OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
    THE SOFTWARE.
