# api documentation for  [bcrypt-nodejs (v0.0.3)](https://github.com/shaneGirish/bcryptJS#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-bcrypt-nodejs.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-bcrypt-nodejs) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-bcrypt-nodejs.svg)](https://travis-ci.org/npmdoc/node-npmdoc-bcrypt-nodejs)
#### A native JS bcrypt library for NodeJS.

[![NPM](https://nodei.co/npm/bcrypt-nodejs.png?downloads=true)](https://www.npmjs.com/package/bcrypt-nodejs)

[![apidoc](https://npmdoc.github.io/node-npmdoc-bcrypt-nodejs/build/screenCapture.buildNpmdoc.browser.%252Fhome%252Ftravis%252Fbuild%252Fnpmdoc%252Fnode-npmdoc-bcrypt-nodejs%252Ftmp%252Fbuild%252Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-bcrypt-nodejs/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-bcrypt-nodejs/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-bcrypt-nodejs/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Shane Girish",
        "email": "shaneGirish@gmail.com"
    },
    "bugs": {
        "url": "https://github.com/shaneGirish/bcrypt-nodejs/issues"
    },
    "contributors": [
        {
            "name": "Alex Murray",
            "url": "https://github.com/alexmurray"
        },
        {
            "name": "Nicolas Pelletier",
            "url": "https://github.com/NicolasPelletier"
        },
        {
            "name": "Josh Rogers",
            "url": "https://github.com/geekymole"
        }
    ],
    "dependencies": {},
    "description": "A native JS bcrypt library for NodeJS.",
    "devDependencies": {},
    "directories": {},
    "dist": {
        "shasum": "c60917f26dc235661566c681061c303c2b28842b",
        "tarball": "https://registry.npmjs.org/bcrypt-nodejs/-/bcrypt-nodejs-0.0.3.tgz"
    },
    "homepage": "https://github.com/shaneGirish/bcryptJS#readme",
    "keywords": [
        "bcrypt",
        "javascript",
        "js",
        "hash",
        "password",
        "auth",
        "authentication",
        "encryption",
        "crypt",
        "crypto"
    ],
    "main": "./bCrypt",
    "maintainers": [
        {
            "name": "shanegirish",
            "email": "shanegirish@gmail.com"
        }
    ],
    "name": "bcrypt-nodejs",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git+https://github.com/shaneGirish/bcryptJS.git"
    },
    "version": "0.0.3"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module bcrypt-nodejs](#apidoc.module.bcrypt-nodejs)
1.  [function <span class="apidocSignatureSpan">bcrypt-nodejs.</span>compare (data, encrypted, callback)](#apidoc.element.bcrypt-nodejs.compare)
1.  [function <span class="apidocSignatureSpan">bcrypt-nodejs.</span>compareSync (data, encrypted)](#apidoc.element.bcrypt-nodejs.compareSync)
1.  [function <span class="apidocSignatureSpan">bcrypt-nodejs.</span>genSalt (rounds, callback)](#apidoc.element.bcrypt-nodejs.genSalt)
1.  [function <span class="apidocSignatureSpan">bcrypt-nodejs.</span>genSaltSync (rounds)](#apidoc.element.bcrypt-nodejs.genSaltSync)
1.  [function <span class="apidocSignatureSpan">bcrypt-nodejs.</span>getRounds (encrypted)](#apidoc.element.bcrypt-nodejs.getRounds)
1.  [function <span class="apidocSignatureSpan">bcrypt-nodejs.</span>hash (data, salt, progress, callback)](#apidoc.element.bcrypt-nodejs.hash)
1.  [function <span class="apidocSignatureSpan">bcrypt-nodejs.</span>hashSync (data, salt, progress)](#apidoc.element.bcrypt-nodejs.hashSync)



# <a name="apidoc.module.bcrypt-nodejs"></a>[module bcrypt-nodejs](#apidoc.module.bcrypt-nodejs)

#### <a name="apidoc.element.bcrypt-nodejs.compare"></a>[function <span class="apidocSignatureSpan">bcrypt-nodejs.</span>compare (data, encrypted, callback)](#apidoc.element.bcrypt-nodejs.compare)
- description and source-code
```javascript
function compare(data, encrypted, callback) {
	/*
		data - [REQUIRED] - data to compare.
		encrypted - [REQUIRED] - data to be compared to.
		callback - [REQUIRED] - a callback to be fired once the data has been compared. uses eio making it asynchronous.
			error - First parameter to the callback detailing any errors.
			same - Second parameter to the callback providing whether the data and encrypted forms match [true | false].
	*/
	if(!callback) {
		throw "No callback function was given."
	}
	process.nextTick(function() {
		var result = null;
		var error = null;
		try {
			result = compareSync(data, encrypted)
		} catch(err) {
			error = err;
		}
		callback(error, result);
	});
}
```
- example usage
```shell
...
Asynchronous
'''
bcrypt.hash("bacon", null, null, function(err, hash) {
	// Store hash in your password DB.
});

// Load hash from your password DB.
bcrypt.compare("bacon", hash, function(err, res) {
    // res == true
});
bcrypt.compare("veggies", hash, function(err, res) {
    // res = false
});
'''
...
```

#### <a name="apidoc.element.bcrypt-nodejs.compareSync"></a>[function <span class="apidocSignatureSpan">bcrypt-nodejs.</span>compareSync (data, encrypted)](#apidoc.element.bcrypt-nodejs.compareSync)
- description and source-code
```javascript
function compareSync(data, encrypted) {
	/*
		data - [REQUIRED] - data to compare.
		encrypted - [REQUIRED] - data to be compared to.
	*/

	if(typeof data != "string" ||  typeof encrypted != "string") {
		throw "Incorrect arguments";
	}

	var encrypted_length = encrypted.length;

	if(encrypted_length != 60) {
		throw "Not a valid BCrypt hash.";
	}

	var same = true;
	var hash_data = hashSync(data, encrypted.substr(0, encrypted_length-31));
	var hash_data_length = hash_data.length;

	same = hash_data_length == encrypted_length;

	var max_length = (hash_data_length < encrypted_length) ? hash_data_length : encrypted_length;

	// to prevent timing attacks, should check entire string
	// don't exit after found to be false
	for (var i = 0; i < max_length; ++i) {
		if (hash_data_length >= i && encrypted_length >= i && hash_data[i] != encrypted[i]) {
			same = false;
		}
	}

	return same;
}
```
- example usage
```shell
...

Basic usage:
-----------
Synchronous
'''
var hash = bcrypt.hashSync("bacon");

bcrypt.compareSync("bacon", hash); // true
bcrypt.compareSync("veggies", hash); // false
'''

Asynchronous
'''
bcrypt.hash("bacon", null, null, function(err, hash) {
	// Store hash in your password DB.
...
```

#### <a name="apidoc.element.bcrypt-nodejs.genSalt"></a>[function <span class="apidocSignatureSpan">bcrypt-nodejs.</span>genSalt (rounds, callback)](#apidoc.element.bcrypt-nodejs.genSalt)
- description and source-code
```javascript
function genSalt(rounds, callback) {
	/*
		rounds - [OPTIONAL] - the number of rounds to process the data for. (default - 10)
		seed_length - [OPTIONAL] - RAND_bytes wants a length. to make that a bit flexible, you can specify a seed_length. (default - 20
)
		callback - [REQUIRED] - a callback to be fired once the salt has been generated. uses eio making it asynchronous.
			error - First parameter to the callback detailing any errors.
			salt - Second parameter to the callback providing the generated salt.
	*/
	if(!callback) {
		throw "No callback function was given."
	}
	process.nextTick(function() {
		var result = null;
		var error = null;
		try {
			result = genSaltSync(rounds)
		} catch(err) {
			error = err;
		}
		callback(error, result);
	});
}
```
- example usage
```shell
...
var bCrypt = require("./bCrypt");

var compares = 0;
var salts = [];
var hashes = [];

console.log("\n\n Salts \n");
bCrypt.genSalt(8, saltCallback);
bCrypt.genSalt(10, saltCallback);

function saltCallback(error, result) {
	if(!error) {
		console.log(result);
	} else {
		console.log(error);
...
```

#### <a name="apidoc.element.bcrypt-nodejs.genSaltSync"></a>[function <span class="apidocSignatureSpan">bcrypt-nodejs.</span>genSaltSync (rounds)](#apidoc.element.bcrypt-nodejs.genSaltSync)
- description and source-code
```javascript
function genSaltSync(rounds) {
	/*
		rounds - [OPTIONAL] - the number of rounds to process the data for. (default - 10)
		seed_length - [OPTIONAL] - RAND_bytes wants a length. to make that a bit flexible, you can specify a seed_length. (default - 20
)
	*/
	if(!rounds) {
		rounds = GENSALT_DEFAULT_LOG2_ROUNDS;
	}
	return gensalt(rounds);
}
```
- example usage
```shell
...


/*jslint node: true, indent: 4, stupid: true */
var bCrypt = require("./bCrypt");

console.log("\n\n Salts \n");

var salt1 = bCrypt.genSaltSync(8);
console.log(salt1);

var salt2 = bCrypt.genSaltSync(10);
console.log(salt2);


console.log("\n\n Hashes \n");
...
```

#### <a name="apidoc.element.bcrypt-nodejs.getRounds"></a>[function <span class="apidocSignatureSpan">bcrypt-nodejs.</span>getRounds (encrypted)](#apidoc.element.bcrypt-nodejs.getRounds)
- description and source-code
```javascript
function getRounds(encrypted) {
	//encrypted - [REQUIRED] - hash from which the number of rounds used should be extracted.
	if(typeof encrypted != "string") {
		throw "Incorrect arguments";
	}
	return Number(encrypted.split("$")[2]);
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.bcrypt-nodejs.hash"></a>[function <span class="apidocSignatureSpan">bcrypt-nodejs.</span>hash (data, salt, progress, callback)](#apidoc.element.bcrypt-nodejs.hash)
- description and source-code
```javascript
function hash(data, salt, progress, callback) {
	/*
		data - [REQUIRED] - the data to be encrypted.
		salt - [REQUIRED] - the salt to be used to hash the password. if specified as a number then a salt will be generated and used (
see examples).
		progress - a callback to be called during the hash calculation to signify progress
		callback - [REQUIRED] - a callback to be fired once the data has been encrypted. uses eio making it asynchronous.
			error - First parameter to the callback detailing any errors.
			encrypted - Second parameter to the callback providing the encrypted form.
	*/
	if(!callback) {
		throw "No callback function was given."
	}
	process.nextTick(function() {
		var result = null;
		var error = null;
		try {
			result = hashSync(data, salt, progress)
		} catch(err) {
			error = err;
		}
		callback(error, result);
	});
}
```
- example usage
```shell
...

bcrypt.compareSync("bacon", hash); // true
bcrypt.compareSync("veggies", hash); // false
'''

Asynchronous
'''
bcrypt.hash("bacon", null, null, function(err, hash) {
	// Store hash in your password DB.
});

// Load hash from your password DB.
bcrypt.compare("bacon", hash, function(err, res) {
    // res == true
});
...
```

#### <a name="apidoc.element.bcrypt-nodejs.hashSync"></a>[function <span class="apidocSignatureSpan">bcrypt-nodejs.</span>hashSync (data, salt, progress)](#apidoc.element.bcrypt-nodejs.hashSync)
- description and source-code
```javascript
function hashSync(data, salt, progress) {
	/*
		data - [REQUIRED] - the data to be encrypted.
		salt - [REQUIRED] - the salt to be used in encryption.
	*/
	if(!salt) {
		salt = genSaltSync();
	}
	return hashpw(data, salt, progress);
}
```
- example usage
```shell
...

This code is based on [javascript-bcrypt] and uses [crypto] (http://nodejs.org/api/crypto.html) to create random byte arrays.

Basic usage:
-----------
Synchronous
'''
var hash = bcrypt.hashSync("bacon");

bcrypt.compareSync("bacon", hash); // true
bcrypt.compareSync("veggies", hash); // false
'''

Asynchronous
'''
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
