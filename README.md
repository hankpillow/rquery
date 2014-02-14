rquery
======

v0.0.1

#What is rquery?

`rquery` is a **personal project** made with nodejs that gives you the ability to run javascript over the html of remote pages. All this is local, you won't iteract with any remote page!

#Why?

Sometimes you find yourself trying to figure out which pages you have to test after changing a html component, css, ajax service, etc. This script was designed to help you with it

#How?

The `rquery` will just bring the html from an url (or url list) and create a virtual document where you can run your scripts and test/check whatever you want.

#Getting started

1 - [install nodejs](http://nodejs.org/)

2 - clone this repository

```
git clone https://github.com/hankpillow/rquery.git
```

3 - fetch dependencies

```
npm install
```

4 - create your spec file:

There is just a single rule for it, your spec file has to export a method called `test`.

The method will be called for every url requested, providing a window object where you can run your tests.

If you want to stop the queue - in case of testing a url list file (`rquery -h`) - just `return false;` and it's done.

```javascript
exports.test = function (window) {
	function report(name, passed) {
		if (passed){
			console.log("✓ %s",name);
		} else {
			console.log("! %s",name);
		}
	}
	report("<h1>",!!window.jQuery("h1").length);
	return true;
};
```

In the above example the page will be created with [jQuery](http://www.jquery.com) loaded in it, so you can take all the advantage of if and run your tests. You wan't to run your own javascript file use the flag `-s http://my/js/file` or type: `rquery -h`;

5 - run the command

```
$ node rquery ./spec.js -u http://www.github.com
```

terminal output:
```
+ (1/1) http://www.github.com
   ✓ <h1>
- no more url to request.
```

To make this work from everywhere (and chop off the leading `node`):

1 - change the file persmission: `chmod +x rquery`

2 - `export PATH="path/to/my/clone-rquery:$PATH"`

Now you can just call `$ rquery ./spec.js -u http://www.github.com`

#to-do

- test request with redirect
- secure urls
- setting cookies and extra headers
