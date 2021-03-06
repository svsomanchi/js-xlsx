# Other JS Engines and Deployments

There are many JS engines and deployments outside of web browsers. NodeJS is the
most popular deployment, but there are many others for special use cases.  Some
optimize for low overhead and others optimize for ease of embedding within other
applications.  Since it was designed for ES3 engines, the library can be used in
those settings!  This demo tries to demonstrate a few alternative deployments.

Some engines provide no default global object.  To create a global reference:

```js
var global = (function(){ return this; }).call(null);
```


## Swift + JavaScriptCore

iOS and OSX ship with the JavaScriptCore framework, enabling easy JS access from
Swift and Objective-C.  Hybrid function invocation is tricky, but explicit data
passing is straightforward.

Binary strings can be passed back and forth using `String.Encoding.ascii`.


## Nashorn

Nashorn ships with Java 8.  It includes a command-line tool `jjs` for running JS
scripts.  It is somewhat limited but does offer access to the full Java runtime.

`jjs` does not provide a CommonJS `require` implementation.  This demo uses a
[`shim`](https://rawgit.com/nodyn/jvm-npm/master/src/main/javascript/jvm-npm.js)
and manually requires the library.

The Java `nio` API provides the `Files.readAllBytes` method to read a file into
a byte array.  To use in `XLSX.read`, the demo copies the bytes into a plain JS
array and calls `XLSX.read` with type `"array"`.


## Rhino

[Rhino](http://www.mozilla.org/rhino) is an ES3+ engine written in Java.  The
`SheetJSRhino` class and `com.sheetjs` package show a complete JAR deployment,
including the full XLSX source.

Due to code generation errors, optimization must be disabled:

```java
Context context = Context.enter();
context.setOptimizationLevel(-1);
```


## duktape and skookum

[Duktape](http://duktape.org/) is an embeddable JS engine written in C.  The
amalgamation makes integration extremely simple!  Duktape understands the source
code and can process binary strings out the box, but does not provide I/O or
other standard library features.

To demonstrate compatibility with duktape, this demo uses the JS runtime from
[Skookum JS](https://github.com/saghul/sjs).  Built upon the duktape engine, it
adds a simple I/O interface to enable reading from files.

