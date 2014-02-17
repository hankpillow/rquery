v0.0.2

##What is rquery?

This is **personal project** that creates a simple api to use [phantomjs](phantomjs.org) and test the loaded pages using "spec files".

##Why?

Sometimes you find yourself trying to figure out which pages you have to test after changing a html component, css, ajax service, etc. This script was designed to help you with it.

###Usage:

- Usage:

```
 $ rquery -s <spec-file> -h htt://github.com
 $ rquery -s <spec-file> -l <my-url-list-file>
```

- Flags:

```
 -u <url> Single url
 -p <prefix> Leading string to be applyed on every url @see -l
 -s <spec> Spec file that will be called to validate every success request.
 -l <list-url> A file with a list of url. One url per line.
 -i <inject script> A hosted javascript to be inject on every request. @default http://ajax.googleapis.com/ajax/libs/jquery/1.8.2/jquery.min.js
```

##Getting started

1 - [install phantomjs](http://phantomjs.org/)

2 - clone this repository

```
git clone https://github.com/hankpillow/rquery.git
```

3 - install dependencies

```
npm install
```

4 - create your spec file:

There is just a single rule for it, your spec file has to export a method called `test`.

The method will be called for every url requested, providing the *document* element then you can run your tests.

If you want to stop the queue - in case of testing a url list file (`rquery -l my-url-list`) - just `return false;` and it's done.

```javascript
exports.test = function () {
	var assert = {
		success : [],
		fail : []
	};
	function report (name, passed) {
		if (passed){
			assert.success.push("\t✓ "+name + " ("+passed+")");
		} else {
			assert.fail.push("\t! "+name);
		}
	}
	report("<h1>",document.getElementsByTagName("h1").length);
	return assert;
};
```

In the above example the page will be created with [jQuery](http://www.jquery.com) loaded in it, so you can take all the advantage of if and run your tests. To load a different script you can use the flag `-i` to inject a different javascript on the page.

5 - run the command

```
$ phantomjs rquery -s ./spec.js -u http://www.github.com
```

terminal output:
```
$ phantomjs rquery -s ./spec.js -u http://github.com
(1/1)	http://github.com	3.978ms
	✓ <h1> (4)
```

To make this work from everywhere (and chop off the leading `phantom`):

1 - change the file persmission: `chmod +x rquery`

2 - `export PATH="path/to/my/clone-rquery:$PATH"`

Now you can just call `$ rquery -s ./spec.js -u http://www.github.com`

###to-do

- keep the loaded javascript locally and avoid loading every request
- test request with redirect
- secure urls
- setting cookies and extra headers
