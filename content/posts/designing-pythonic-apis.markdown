---
date: "2016-08-05T00:00:00Z"
title: Designing Pythonic APIs
tags:
- python
- software design
---

*Learning from Kenneth Reitz's [Requests][requests url]*

When writing a package (library), providing it with a good API, is almost as important as its
functionality itself (well, at least if you want some adoption), but what makes a good API? In this post, I'll try to provide some insights on that question by comparing *Requests* and *Urllib* (part of Python's standard library) in a few typical HTTP usage scenarios and see why *Requests* has become the de facto standard among Python users.

\* Throughout our investigation we'll be using **Python 3.5** and **Requests 2.10.0**.

\*\* This blog post is an adaptation of a talk I gave at our local Python meetup - [PywebIL][meetup link] - last week. You can find the slides [here][talk slides].

## *Requests* vs. *Urllib*

### Use case #1: sending a GET request

```python
import urllib.request
urllib.request.urlopen('http://python.org/')
```




    <http.client.HTTPResponse at 0x7fdb08b1bba8>




```python
import requests
requests.get('http://python.org/')
```




    <Response [200]>



#### Explicit (API endpoints) is better than implicit
* Notice how requests is more concise (hence, clear) about what it will do.
* *Urllib* is getting told implicitly to send a GET request since it didn't receive a `data` argument
* *Requests* function name explicitly mark what it will do.

#### Helpful object representation
* *Requests* returns a helpful string with the request status code when examining it (this done by implementing the `__repr__()` method).
* *Urllib* just returns the default (unclear) object representation

#### Code snippet
([requests/api.py](https://github.com/kennethreitz/requests/blob/v2.10.0/requests/api.py)):


```python
def request(method, url, **kwargs):
    with sessions.Session() as session:
        return session.request(method=method, url=url, **kwargs)

def get(url, params=None, **kwargs):
    kwargs.setdefault('allow_redirects', True)
    return request('get', url, params=params, **kwargs)

def post(url, data=None, json=None, **kwargs):
    return request('post', url, data=data, json=json, **kwargs)
```

* All the HTTP verbs follow a similar flow prior to sending, hence there is a the `request()` main flow function.
* Implementing a "helper function" for each verb that calls `request()`, enables the explicitness we are looking for.

### Use case #2: getting a request status code


```python
import urllib.request
r = urllib.request.urlopen('http://python.org/')
r.getcode()
```




    200




```python
import requests
r = requests.get('http://python.org/')
r.status_code
```




    200



#### No need for getters and setters
* Accessing an object property as an actual property (and not a method call) makes the code a little clearer.
* If you come from other OOP language (hmmm... Java), you might be tempted to use getters and setters to allow future changes to the object's properties. No need for that in Python, just use the [`@property`](http://www.programiz.com/python-programming/property) decorator.

#### Code snippet

[http/client.py](https://github.com/python/cpython/blob/3.5/Lib/http/client.py#L737):


```python
class HTTPResponse(io.BufferedIOBase):

    # ...

    def getcode(self):
        return self.status
```

* *Urllib* (or actually *http*) is using a "getter" to return a class property.


### Use case #3: encoding, sending and decoding a POST request


```python
import urllib.parse
import urllib.request
import json

url = 'http://www.httpbin.org/post'
values = {'name' : 'Michael Foord'}

data = urllib.parse.urlencode(values).encode()
response = urllib.request.urlopen(url, data)
body = response.read().decode()
json.loads(body)
```


```python
import requests

url = 'http://www.httpbin.org/post'
data = {'name' : 'Michael Foord'}

response = requests.post(url, data=data)
response.json()
```

#### Easy access to common functionality
* *Requests* provides an out-of-the-box experience for the encoding of the data and loading the JSON response while in *Urllib* you have to implement those parts yourself.
* When designing your API think: how will my package be commonly use? What plugs can I add to make that usage easier?

On the same note, *Requests* also provides an elegant way to send JSON content:


```python
import requests

url = 'http://www.httpbin.org/post'
data = {'name' : 'Michael Foord'}

response = requests.post(url, json=data)
response.json()
```

### Use case #4: sending authenticated request

The following creates persistence credentials for HTTP requests, and sends a request:

```python
import urllib.request

gh_url = 'https://api.github.com/user'

password_mgr = urllib.request.HTTPPasswordMgrWithDefaultRealm()
password_mgr.add_password(None, gh_url, 'user', 'pswd')
handler = urllib.request.HTTPBasicAuthHandler(password_mgr)

opener = urllib.request.build_opener(handler)
opener.open(gh_url)
```

```python
import requests

session = requests.Session()
session.auth = ('user', 'pswd')
session.get('https://api.github.com/user')
```

But what if we just want to do a single HTTP call? Do we need all that code? *Requests* have you covered here:

```python
import requests

requests.get('https://api.github.com/user', auth=('user', 'pswd'))
```

#### Provide possibilities for simple and advanced usage
* *Requests* allow concise usage, when sending a single request, and a more verbose one for multiple requests.
* Don't make the user go through a lengthy process when he needs a simple use case.


#### Prefer Python data types over self-made ones
* *Requests* usage of Python's data structures makes it very easy to use. No need to get to know internal *Requests* packages.

#### Libraries code

[requests/models.py](https://github.com/kennethreitz/requests/blob/v2.10.0/requests/models.py#L488):


```python
def prepare_auth(self, auth, url=''):
    """Prepares the given HTTP auth data."""

    # ...

    if auth:
        if isinstance(auth, tuple) and len(auth) == 2:
            # special-case basic HTTP auth
            auth = HTTPBasicAuth(*auth)
```

* *Requests* internally converts the `(user,pass)` tuple to an authentication class.

### Use case #5: handling errors


```python
from urllib.request import urlopen
response = urlopen('http://www.httpbin.org/geta')
response.getcode()
```


    ---------------------------------------------------------------------------

    HTTPError                                 Traceback (most recent call last)

    <ipython-input-45-5fba039d189a> in <module>()
          1 from urllib.request import urlopen
    ----> 2 response = urlopen('http://www.httpbin.org/geta')
          3 response.getcode()


    /usr/lib/python3.5/urllib/request.py in urlopen(url, data, timeout, cafile, capath, cadefault, context)
        161     else:
        162         opener = _opener
    --> 163     return opener.open(url, data, timeout)
        164
        165 def install_opener(opener):


    /usr/lib/python3.5/urllib/request.py in open(self, fullurl, data, timeout)
        470         for processor in self.process_response.get(protocol, []):
        471             meth = getattr(processor, meth_name)
    --> 472             response = meth(req, response)
        473
        474         return response


    /usr/lib/python3.5/urllib/request.py in http_response(self, request, response)
        580         if not (200 <= code < 300):
        581             response = self.parent.error(
    --> 582                 'http', request, response, code, msg, hdrs)
        583
        584         return response


    /usr/lib/python3.5/urllib/request.py in error(self, proto, *args)
        508         if http_err:
        509             args = (dict, 'default', 'http_error_default') + orig_args
    --> 510             return self._call_chain(*args)
        511
        512 # XXX probably also want an abstract factory that knows when it makes


    /usr/lib/python3.5/urllib/request.py in _call_chain(self, chain, kind, meth_name, *args)
        442         for handler in handlers:
        443             func = getattr(handler, meth_name)
    --> 444             result = func(*args)
        445             if result is not None:
        446                 return result


    /usr/lib/python3.5/urllib/request.py in http_error_default(self, req, fp, code, msg, hdrs)
        588 class HTTPDefaultErrorHandler(BaseHandler):
        589     def http_error_default(self, req, fp, code, msg, hdrs):
    --> 590         raise HTTPError(req.full_url, code, msg, hdrs, fp)
        591
        592 class HTTPRedirectHandler(BaseHandler):


    HTTPError: HTTP Error 404: NOT FOUND



```python
import requests
r = requests.get('http://www.httpbin.org/geta')
r.status_code
```




    404



#### Let the user choose how to handle errors
* Some programmers prefer exceptions, some prefer checks.
* In some situations a check is much more elegant and sometimes it's the other way around.
* It's good to let your users to choose what to use when.
* Defaulting to return codes allow that, while defaulting to `exceptions` do not.

Usage examples:

```python
from urllib.request import urlopen
from urllib.error import URLError, HTTPError
try:
    response = urlopen('http://www.httpbin.org/geta')
except HTTPError as e:
    if e.code == 404:
        print('Page not found')
else:
    print('All good')
```

    Page not found



```python
from requests.exceptions import HTTPError
import requests
r = requests.get('http://www.httpbin.org/posta')
try:
    r.raise_for_status()
except HTTPError as e:
    if e.response.status_code == 404:
        print('Page not found')
```

    Page not found



```python
import requests
r = requests.get('http://www.httpbin.org/geta')
if r.ok:
    print('All good')
elif r.status_code == requests.codes.not_found:
    print('Page not found')
```

    Page not found



That's it for now. I learned quite a bit preparing this talk/post, and I hope you did too reading it. I'll be glad to hear your comments down bellow or on Twitter (@noamelf).

##### Update (August 8th, 2016)

If you end up wondering how come there is such a stark usability difference between Requests and Urllib like many, including myself, do. Nick Coghlan shares his wide perspective on the subject, in a [comment bellow][nicks comment] and a following blog post (with the self-explanatory title): [what problem does it solve?][nicks post].

[meetup link]: http://www.meetup.com/PyWeb-IL/events/232724175/
[talk slides]: /designing-pythonic-apis-talk
[nicks post]: http://www.curiousefficiency.org/posts/2016/08/what-problem-does-it-solve.html
[nicks comment]: http://noamelf.com/2016/08/05/designing-pythonic-apis/#comment-2823855721
[requests url]: http://docs.python-requests.org/en/master/

<blockquote class="reddit-card" data-card-created="1471210275"><a href="https://www.reddit.com/r/Python/comments/4wfdfc/designing_pythonic_apis_learning_from_requests/?ref=share&ref_source=embed">Designing Pythonic APIs - learning from Requests</a> from <a href="http://www.reddit.com/r/Python">Python</a></blockquote>
<script async src="//embed.redditmedia.com/widgets/platform.js"
charset="UTF-8"></script>
