# Riot.js and Mocha: Minimalistic testing in the browser

In the last tutorial (https://github.com/vitogit/tdd-mocha-chai-riot) I explained how to test Riot.js tags using Karma, phantomJs and Mocha, that´s works very well to execute the tests from the command line. But there is a more minimalistic approach that doesn't need Karma or phantomJs, in case you want to execute the tests in the browser.  I will use the same test cases from the previous tutorial.  


The mocha before hook looked like this:
```
  before(function() {
    var html = document.createElement('hello')
    document.body.appendChild(html)
    tag = riot.mount('hello')[0]
  });

```

We are going to just change two things. 
If we execute this it will not work because the tag will be undefined, we need to use the riot.compile method:

```
  before(function() { 
    riot.compile(function() {
      var html = document.createElement('hello') 
      document.body.appendChild(html)        
      tag = riot.mount('hello')[0]
    })    
  });
```

Now we got the tag properly loaded, but still the tests will fail because they run before our tag finish loading. 
So to fix this we need to use the `done` method from Mocha, to wait until the tag is loaded:

```
  before(function(done) { 
    riot.compile(function() {
      var html = document.createElement('hello') 
      document.body.appendChild(html)        
      tag = riot.mount('hello')[0]
      done()
    })    
  });
```

And that´s it, now our tests will run just fine. 
Online version:  http://plnkr.co/edit/BFOijJ?p=preview


Clone, install and run
```
npm install
npm start 
```

Then go to localhost:4000 and you should see the tests pass.
