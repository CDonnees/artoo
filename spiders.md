---
layout: page
title: artoo.spiders
id: spiders
---

# {{ page.title }}

---

**artoo**'s spiders are a nice and cosy way to trigger a handful of HTTP requests to collect some data.

They work only with ajax for the time being but should be able to crawl though pages of the same domain soon.

---

* [artoo.ajaxSpider](#ajax)

----

<h2 id="ajax">artoo.ajaxSpider</h2>
**artoo**'s ajax spider aims at triggering a series of ajax requests and passing the accumulated data to a final callback.

```js
artoo.ajaxSpider(urlList, [params, callback]);
```

*Arguments*

* **urlList** *array | function* : the list of urls to request through ajax or, alternatively, a function taking as sole argument the index of the iteration and returning either the desired url or `false` to break the spider.
* **params** *?object | ?function* : a object containing optional parameters. If params is a function, it will be considered as the `callback` argument.
  * **callback** *?function* : a function to be triggered each time data is retrieved through ajax. This function takes as arguments the retrieved data, the current index and the current data accumulated. Also, this function should either return the wanted data to accumulate or `false` to break the spider.
  * **throttle** *?integer | ?function*: Number of milliseconds to wait between two ajax requests. You can also pass a function taking the current index and returning the time to wait.
  * **method** *?string* [`'get'`] : the default HTTP or jQuery method.
  * **data** *?object* : the default data object to pass as the requests parameters.
  * **concat** *?boolean* [`false`] : should the data retrieved be concatenated into the accumulator or merely pushed?
  * **done** *?function* : same as `callback` argument.
* **callback** : callback fired when the spider iteration is over and taking as argument the accumulated data.

*Urls List*

The spider can take the following as urls:

* a simple string.
* an object that may contain the following keys:
  * **url** *string* : the url string.
  * **method** *?string* : the http or jQuery method for the request.
  * **data** *?object* : an optional data object to pass as the request parameters.

If you use a function rather than a list to generate your urls, this one is obviously entitled to return the same items as those you would find in the list.

*Examples*

```js
// Basic
artoo.ajaxSpider(['url1', 'url2'], function(data) {
  console.log('Retrieved data:', data);
});

// Complex
artoo.ajaxSpider(
  function(i) {
    return 'http://domain.com/' + i + '.html';
  },
  {
    method: 'getJson',
    callback: function(data, i, acc) {
      return (!data.error) ? data.posts || false;
    },
    throttle: 3000
  },
  function(data) {
    console.log('Retrieved data:', data);
  }
);
```