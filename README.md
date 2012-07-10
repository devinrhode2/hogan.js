## Hogan.js - A mustache compiler. [![Build Status](https://secure.travis-ci.org/twitter/hogan.js.png)](http://travis-ci.org/twitter/hogan.js)

[Hogan.js](http://twitter.github.com/hogan.js/) is a compiler for the
[Mustache](http://mustache.github.com/) templating language. For information
on Mustache, see the [manpage](http://mustache.github.com/mustache.5.html) and
the [spec](https://github.com/mustache/spec).

##About this fork

Throwing [this idea](https://gist.github.com/3072669) out there!! Go read the idea.

## Basics

Hogan compiles templates to HoganTemplate objects, which have a render method.

```js
var data = {
  screenName: "dhg",
};

var template = Hogan.compile("Follow @{{screenName}}.");
var output = template.render(data);

// prints "Follow @dhg."
console.log(output);
```



<style type="text/css">
    p.p3 { font-family: 12.0px Menlo; color: #c70000}
    p.p4 {margin: 0.0px 0.0px 0.0px 0.0px; font: 12.0px Menlo; min-height: 14.0px}
    p.p5 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 18.0px; font: 12.0px Menlo; color: #c70000}
    p.p6 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 18.0px; font: 12.0px Menlo}
    p.p7 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 18.0px; font: 12.0px Helvetica; min-height: 14.0px}
    p.p8 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 18.0px; font: 12.0px Helvetica}
    p.p9 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 18.0px; font: 12.0px Menlo; min-height: 14.0px}
    p.p10 {margin: 0.0px 0.0px 0.0px 0.0px; font: 12.0px Helvetica; color: #bc2e00}
    p.p11 {margin: 0.0px 0.0px 0.0px 0.0px; font: 12.0px Menlo}
    span.s1 {font: 12.0px Menlo}
    span.s2 {font: 12.0px Consolas; color: #c70000}
    span.s3 {font: 12.0px Helvetica; color: #000000}
    span.s4 {color: #000000}
    span.s5 {color: #f90000}
    span.s6 {color: #c70000}
    span.s7 {text-decoration: underline}
    span.s8 {font: 12.0px Menlo; color: #c70000}
    span.s9 {text-decoration: underline ; color: #000000}
    span.s10 {font: 12.0px Menlo; text-decoration: underline}
    span.s11 {font: 12.0px Helvetica}
  </style>
You should have <span class="s1">ruby 1.9.2, rails 3.2.2, and gem</span>
installed and setup.

  

You may need to update rails with: <span class="s2">gem update
rails</span>

  

**clone** down the github repo

**cd** into the project directory

to install various gems with the project, run:

<span class="s3">\$ </span>bundle install

  

-Download and run the Postgres installer from the official site at
[http://www.enterprisedb.com/products-services-training/pgdownload][]<span class="Apple-converted-space"> </span>

During installation, use your standard system
password.<span class="Apple-converted-space"> </span>

I renamed my install directory to /usr/local/pgsql and the data source
to /usr/local/pgsql/data2. These steps probably aren’t necessary.
Otherwise, stick with all the defaults. At the end there may be
something about another install stack thing, skip this.

  

Now we want to become the postgres super user created from the
installer:

  

<span class="s4">\$ </span>sudo su - postgres

<span class="s4">postgres \$ </span>psql template1

<span class="s4">template1=\# </span>CREATE USER okraapp WITH PASSWORD
’<span class="s5">***yourStandardPassword***</span>’;

<span class="s4">template1=\# </span>CREATE DATABASE
okraapp\_development WITH OWNER = postgres;

<span class="s4">template1=\# </span>GRANT ALL PRIVILEGES ON DATABASE
okraapp\_development to okraapp;

<span class="s4">template1=\# </span>CREATE DATABASE okraapp\_test WITH
OWNER = postgres;

<span class="s4">template1=\# </span>GRANT ALL PRIVILEGES ON DATABASE
okraapp\_test to okraapp;

template1=\# <span class="s6">\\q</span>

postgres \$ <span class="s6">exit</span>

  

Edit config/database.yml

You need to open config/database.yml and add in your standard password
for the <span class="s1">password:</span> field under
<span class="s1">development:</span> and <span class="s1">test:</span>

  

<span class="s4">\$ </span>rake db:migrate

  

<span class="s7">if you see a bunch of output like the following you
win!</span>

  

==<span class="Apple-converted-space">  </span>AddAttachments: migrating
=================================================

– add\_column(:notifications, :attachment, :string)

<span class="Apple-converted-space">   </span>-\> 0.0018s

==<span class="Apple-converted-space">  </span>AddAttachments: migrated
(0.0029s) ========================================

  

==<span class="Apple-converted-space">  </span>CreateMessagesTable:
migrating ============================================

  

And finally run <span class="s1">**rails server**</span> and go to
localhost:3000 (or <span class="s8">open http://localhost:3000</span>)
you should see an app running! Congrats!

  

<span class="s9">Problems with </span><span class="s10">rake
db:migrate</span><span class="s7"><span class="Apple-converted-space" truncated! please download pandoc if you want to convert large files.>

  [http://www.enterprisedb.com/products-services-training/pgdownload]: http://www.enterprisedb.com/products-services-training/pgdownload





## Features

Hogan is fast--try it on your workload.

Hogan has separate scanning, parsing and code generation phases. This way it's
possible to add new features without touching the scanner at all, and many
different code generation techniques can be tried without changing the parser.

Hogan exposes scan and parse methods. These can be useful for
pre-processing templates on the server.

```js
var text = "{{^check}}{{#i18n}}No{{/i18n}}{{/check}}";
text +=  "{{#check}}{{#i18n}}Yes{{/i18n}}{{/check}}";
var tree = Hogan.parse(Hogan.scan(text));

// outputs "# check"
console.log(tree[0].tag + " " + tree[0].name);

// outputs "Yes"
console.log(tree[1].nodes[0].nodes[0]);
```

It's also possible to use HoganTemplate objects without the Hogan compiler
present. That means you can pre-compile your templates on the server, and
avoid shipping the compiler. However, the optional lambda features from the
Mustache spec do require the compiler to be present.

## Why Hogan.js?

Why another templating library?

Hogan.js was written to meet three templating library requirements: good
performance, standalone template objects, and a parser API.

## Compilation options

The second argument to Hogan.compile is an options hash.

```js
var text = "my <%example%> template."
Hogan.compile(text, {delimiters: '<% %>'});
```

There are current four valid options.

asString: return the compiled template as a string. This feature is used
by hulk to produce strings containing pre-compiled templates.

sectionTags: allow custom tags that require opening and closing tags, and
treat them as though they were section tags.

```js
var text = "my {{_foo}}example{{/foo}} template."
Hogan.compile(text, { sectionTags: [{o: '_foo', c: 'foo'}]});
```

The value is an array of object with o and c fields that indicate names
for custom section tags. The example above allows parsing of {{_foo}}{{/foo}}.

delimiters: A string that overrides the default delimiters. Example: "<% %>".

disableLambda: disables the higher-order sections / lambda-replace features of Mustache.

## Issues

Have a bug? Please create an issue here on GitHub!

https://github.com/twitter/hogan.js/issues

## Versioning

For transparency and insight into our release cycle, releases will be numbered with the follow format:

`<major>.<minor>.<patch>`

And constructed with the following guidelines:

* Breaking backwards compatibility bumps the major
* New additions without breaking backwards compatibility bumps the minor
* Bug fixes and misc changes bump the patch

For more information on semantic versioning, please visit http://semver.org/.

## Authors

**Robert Sayre**

+ http://github.com/sayrer

**Jacob Thornton**

+ http://github.com/fat

## License

Copyright 2011 Twitter, Inc.

Licensed under the Apache License, Version 2.0: http://www.apache.org/licenses/LICENSE-2.0
