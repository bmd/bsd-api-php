Blue State Digital PHP API Client
=================================

This library provides an interface to the BSD Tools.

Example usage:

```
use Blue\Tools\Api\Client;

$client = new Client('user_id', 'secret', 'https://baseurl.com');

/** @var ResponseInterface $response */
$response = $client->get('api/list_things', ['param' => 'value']);
```

The BSD Tools API will sometimes return a deferred result, which means that the results of the call are not immediately available. The above call, however, will poll for updates to this API call, and will block until the result has been resolved. The manner in which the client polls for updates can be configured as follows:

```
// Set the client to wait 30 seconds between updates
$client->setDeferredResultInterval(30);

// Set the client to give up after 10 updates (a RuntimeException will be thrown)
$client->setDeferredResultMaxAttempts(10);
```

If you want to prevent the library from blocking until a deferred result is resolved, then you can use the following configuration option. In this case, the Guzzle response object will be returned even if the response is a 202 deferral and you will have to handle that deferral yourself (i.e. making subsequent calls to the BSD API to confirm that the results have been processed successfully).
```
$client->processDeferredResults(false);
```

Installation
------------

Update `composer.json`:
```
"require": {
    "bluestatedigital/tools-api-client": "~2.0"
}
```

Handling Responses
------------------

By default the API client uses Guzzle's RequestException for any responses above HTTP 300. To use your own handler you may either create your own Guzzle error plugin or emitter (see Guzzle 5 documentation), or disable Guzzle's handler entirely by using the following code:
```
// Prevent Exceptions on non-actionable HTTP response (400 and 500 range)
$client->setRequestOption('exceptions', false);
```