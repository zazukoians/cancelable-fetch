# cancelable-fetch

Makes a fetch cancelable.

## Usage

cancelable-fetch wraps an existing fetch and adds a `cancel` method.
Only the headers will be fetched if the response body of fetch supports `cancel`.
The request can't be canceled before, because the fetch interface doesn't provide a method for it.
In any case, if `cancel` was called, an user cancelation error will be thrown.

The Promise standard doesn't support `cancel`.
Therefore always use the return value of cancelable-fetch, because `cancel` will not be forwarded in the Promise chain!

### Example

In this example `doRequest` takes care that only the latest request will respond.

    var cancelableFetch = require('cancelable-fetch')

    var request = null

    function doRequest () {
      // check if there is a running request
      if (request) {
        // cancel that request
        request.cancel()
      }

      // wrap the new request with cancelableFetch
      request = cancelableFetch(fetch(url)).then(function (res) {
        // remove running request
        request = null

        // forward response
        return res
      })

      return request
    }
