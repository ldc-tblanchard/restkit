Build resource object
=====================

Building a resource object is easy using :class:`restkit.resource.Resource` class. You just need too inherit this object and add your methods. `Couchdbkit <http://www.couchdbkit.org>`_ is using restkit to access to `CouchDB <http://couchdb.apache.org>`_. A resource object is an Python object associated to an URI. You can use `get`, `post`, `put`, `delete` or  `head` method just like you do a request.

Create a simple Twitter Search resource
+++++++++++++++++++++++++++++++++++++++

We use `simplejson <http://code.google.com/p/simplejson/>`_ to handle deserialisation of data.

Here is the snippet::

  from restkit import Resource

  try:
      import simplejson as json
  except ImportError:
      import json # py2.6 only

  class TwitterSearch(Resource):

      def __init__(self, **kwargs):
          search_url = "http://search.twitter.com"
          super(TwitterSearch, self).__init__(search_url, follow_redirect=True,
                                          max_follow_redirect=10, **kwargs)

      def search(self, query):
          return self.get('search.json', q=query)

      def request(self, *args, **kwargs):
          resp = super(TwitterSearch, self).request(*args, **kwargs)
          return json.loads(resp.body_string())

  if __name__ == "__main__":
      s = TwitterSearch()
      print s.search("gunicorn")

Restkit.Client options for Restkit.Resource initialization
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Here is the list of options with default:

* ``follow_redirect=False``, 
* ``force_follow_redirect=False``, 
* ``max_follow_redirect=5``, 
* ``filters=None``, 
* ``decompress=True``, 
* ``max_status_line_garbage=None``, 
* ``max_header_count=0``, 
* ``pool=None``, 
* ``response_class=None``, 
* ``timeout=None``, 
* ``use_proxy=False``, 
* ``max_tries=3``, 
* ``wait_tries=0.3``, 
* ``pool_size=10``, 
* ``backend='thread'``, 
* ``**ssl_args``
 
Source/latest: https://benoitc.github.io/restkit/api/restkit.client.Client-class.html
