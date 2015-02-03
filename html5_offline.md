# Create Offline Web Applications with AngularJS

With the appearance and support of newer HTML5 features it is possible to build Web-Applications which work completely offline.

In this blog post we describe a simple Web-Application which can store PDF-Files for offline access. A working version of the sample application is available online:

[http://pdfoffline.iterativ.ch](http://pdfoffline.iterativ.ch)

This sample application has the following features:

* It can locally store PDF-files to make them available offline with the help of the localForage library together with AngularJS.
* It works completely offline. You can access the site without internet connection with the help of Application Cache.
* The build process for all needed files including the AppCache manifest file is done automatically with Grunt.
* It shows the usage of the createObjectURL-API needed to create links out of locally stored files.

You can download and inspect the source on GitHub [https://github.com/iterativ/angularjs-pdf-offline-demo](https://github.com/iterativ/angularjs-pdf-offline-demo). The GitHub README-file contains also a short description on how to start the sample application on your machine.

In the following paragraphs we describe some of the parts of the sample application.


## Store files locally with localForage

Our sample app uses the excellent [angular-localForage](https://github.com/ocombe/angular-localForage) library, which abstracts away the differences between different HTML5 storage APIs like [IndexedDB](http://caniuse.com/#feat=indexeddb) and [localStorage](http://caniuse.com/#feat=namevalue-storage) and provides an AngularJS service and directive for the Mozilla [localForage](https://github.com/mozilla/localForage) library.

angular-localForage provides a `$localForage`-service which you can use in your AngularJS code like other AngularJS-services.

The methods of `$localForage` that we use are `setItem(key, value)` and `getItem(key, value)`. All methods are asynchronous and return a promise. So the usage of these methods is similar to a `$http.get` call.


## PDF-files and Blobs

A [Blob](https://developer.mozilla.org/en/docs/Web/API/Blob) is a JavaScript object which represents a piece of binary data. The content of a PDF-file is binary data. So to work with PDF-files directly in JavaScript we have to use Blobs.

It is very easy to get the a Blob from a PDF-file, you simply have to make an AJAX-request to the file. In AngularJS we can specify the `responseType` for a [`$http.get()`](https://docs.angularjs.org/api/ng/service/$http) call. The possible values for the `responseType` are listed here on [MDN](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest#responseType). For our purposes we need the responseType `"blob"`.

By choosing the responseType `"blob"` for the call we directly get a PDF-Blob back from the $http-call. We can store this Blob value without modification into $localForage. The key is the PDF-filename and the value is the PDF-Blob.

    var pdfFileUrl = <url of pdf file>;
    var filename = <pdf file name used as key>;
    
    $http.get(pdfFileUrl, {responseType:'blob'}).then(function (response) {
        // response.data contains the pdfBlob
        $localForage.setItem(filename, response.data);
    });


## Object URLs

The normal behavior when you press a link with a PDF-file is, that the PDF file opens in the browser. A user also expects that he can right click on a link and download the linked file.

To make this behavior also work with a locally stored PDF-Blob we need the ability to create some kind of URL out of our Blob.

HTML5 supports exactly this use case. With the help of the method [`URL.createObjectURL(pdfBlob)`](https://developer.mozilla.org/en-US/docs/Web/API/URL.createObjectURL) we can create a URL out of a PDF Blob. The name of this URL gets created automatically and has the form `blob:<some-uuid>`

We can use the URL created from the `createObjectURL` to set the href-attribute in a link.

In the controller:

    $scope.localURL = URL.createObjectURL(pdfBlob);
    
In the view:

    <a ng-href="{{localURL}}">filename</a>
    
One thing to notice is, that you have to explicitly [enable the blob URL protocol](http://stackoverflow.com/questions/15606751/angular-changes-urls-to-unsafe-in-extension-page) in AngularJS. This can be done with this line in the AngularJS config function:

    $compileProvider.aHrefSanitizationWhitelist(/^\s*(https?|ftp|mailto|chrome-extension|blob):/);
    
One problem with the Object-URL is that it does not have a very nice name. Its something like `blob:http%3A//pdfoffline.iterativ.ch/d735e842-4ea4-4815-930d-b1415434a5c2`. One way to overcome this problem is by using the [download-attribute of the `<a>` element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a):

	<a download="filename.pdf" ng-href="{{localURL}}">filename</a>

In the presence of the download attribute you can right click on the link and it will propose the text of the attribute as filename. But the file won't open as PDF anymore, it will always show the download-file dialog. In our sample app we don't use the download-attribute.


## Application Cache

It is not enough to just store files locally to make a real offline Web-Application. The user of your app still needs to be able to access your Web-App when she is offline. [Application Cache](http://caniuse.com/#feat=offline-apps) provides the mechanism for a Web-Application to be accessible even when offline.

To use the Application Cache (AppCache) feature you have to build and serve a so called manifest file. The manifest file needs to be added to the manifest attribute of the `html`-tag in the HTML-page.
Here is a snippet from the index.html file with the included manifest file:

	<html class="no-js" manifest="/manifest.appcache">
	...
	</html>

The manifest file is a simple text file which your server needs to send with the MIME-type `text/cache-manifest`. The manifest file we use in our sample application is accessible here [http://pdfoffline.iterativ.ch/manifest.appcache](http://pdfoffline.iterativ.ch/manifest.appcache). It has the following content:

    CACHE MANIFEST
    # This manifest was generated by grunt-manifest HTML5 Cache Manifest Generator
    # Time: Fri Jun 20 2014 10:41:05 GMT+0200 (CEST)

    CACHE:
    scripts/main.4748b530.js
    scripts/vendor.e1482bbb.js
    styles/vendor.102f46e2.css
    styles/main.4e08dd69.css
    fonts/fontawesome-webfont.eot?v=4.1.0
    fonts/fontawesome-webfont.eot?#iefix&v=4.1.0
    fonts/fontawesome-webfont.woff?v=4.1.0
    fonts/fontawesome-webfont.ttf?v=4.1.0
    fonts/fontawesome-webfont.svg?v=4.1.0#fontawesomeregular
    views/about.html
    views/main.html
    fonts/FontAwesome.otf
    fonts/fontawesome-webfont.eot
    fonts/fontawesome-webfont.svg
    fonts/fontawesome-webfont.ttf
    fonts/fontawesome-webfont.woff
    fonts/glyphicons-halflings-regular.eot
    fonts/glyphicons-halflings-regular.svg
    fonts/glyphicons-halflings-regular.ttf
    fonts/glyphicons-halflings-regular.woff

    NETWORK:
    *
    http://*
    https://*
    /listpdf

    SETTINGS:
    prefer-online
    	
The first time the browser accesses a site with a AppCache manifest file it will download and cache all the files which are listed in the `CACHE`-section.
In the `CACHE`-section every single file which Application Cache should provide needs to be listed.

It is possible to write the manifest.appcache file from hand, but it gets tedious very fast. In our sample app we use the [grunt-manifest](https://github.com/gunta/grunt-manifest) Grunt plugin to generate a manifest file. 

We also use the grunt-usemin plugin to create revisioned file names of our assets. In the Gruntfile we use configure the usemin step to replace the non-revisioned file names to revisioned file names with the help of some RegExps:

    usemin: {
        html: ['<%= yeoman.dist %>/{,*/}*.html'],
        css: ['<%= yeoman.dist %>/styles/{,*/}*.css'],
        manifest: ['<%= yeoman.dist %>/manifest.appcache'],
        options: {
            assetsDirs: ['<%= yeoman.dist %>', '<%= yeoman.dist %>/images'],
            patterns: {
                manifest: [
                    [/(scripts\/vendor\.js)/, 'Replacing reference to vendor.js'],
                    [/(scripts\/main\.js)/, 'Replacing reference to main.js'],
                    [/(styles\/vendor\.css)/, 'Replacing reference to vendor.css'],
                    [/(styles\/main\.css)/, 'Replacing reference to main.css']
                ]
            }
        }
    },

        
This build steps adds the revisions to file names like `main.js` to `main.4748b530.js`. In the usemin-configuration we had to add a distinct pattern for every file we wanted to be replaced.
        
    
AppCache can be a pain to work with and there are many small details which have to be correct in for it in order to work. 

One particular problem we had, was that the AJAX-request to access the `listpdf`-URL often hit the cache, although it was explicitly set in the `NETWORK:`-section of the manifest file. A solution which solved the problem was adding the `*` and the `http://*` line to the manifest file. This solution was presented in a [Stackoverflow answer](http://stackoverflow.com/questions/9740854/how-do-i-allow-json-requests-when-using-html5s-appcache-feature/11102856#11102856).

The best resource I found to work with AppCache was [AppCache Facts](http://appcachefacts.info/). This site also contains pointers to other good articles which discuss AppCache.

To test the version of the sample app with the AppCache support, you have to start the 'dist'-version. You can run this version with:

	> grunt serve:dist

You can then access the site and download a PDF. When you later stop the grunt-server the site is still accessible.


## Tooling and Debugging

The AppCache can be very hard to work with. It is especially hard to get rid of the locally cached files of AppCache.

There is an Application Cache section in the Chrome Developer Tools under the tab 
"Resources". But as far as I have seen, there is no way to remove the cached files from the Developer Tools.

In the chrome browser you can use the internal URL [chrome://appcache-internals/](chrome://appcache-internals/) to see a list of all web sites which use a manifest file. From this site it is possible to remove the cached files for single web sites. 

TODO add screenshot

In other browsers it is not possible to delete the Application Cache files for single web apps. See also the description on MDN: [Storage location and clearing the offline cache](https://developer.mozilla.org/en-US/docs/Web/HTML/Using_the_application_cache#Storage_location_and_clearing_the_offline_cache)

In the Chrome Developer Tools you can inspect the content of the IndexedDB store.

TODO add screenshot