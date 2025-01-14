# Debugging

I'm certainly sure that along the way you will find some cache behavior that is not the
expected to the current situation. To help with that, the library has a separate robust
build with support to debug logs enabled.

You can use it by changing the `setupCache` import:

::: code-group

```ts [EcmaScript]
import Axios from 'axios';

import { setupCache } from 'axios-cache-interceptor'; // [!code --]
import { setupCache } from 'axios-cache-interceptor/dev'; // [!code ++]

// same object, but with updated typings.
const axios = setupCache(Axios, {
  debug: console.log // [!code ++]
});
```

```ts [Common JS]
const Axios = require('axios');

const { setupCache } = require('axios-cache-interceptor'); // [!code --]
const { setupCache } = require('axios-cache-interceptor/dev'); // [!code ++]

// same object, but with updated typings.
const axios = setupCache(Axios, {
  debug: console.log // [!code ++]
});
```

```ts{3,4} [Browser]
const Axios = window.axios;

// Choose development bundle. // [!code ++]
const { setupCache } = window.AxiosCacheInterceptor;

// same object, but with updated typings.
const axios = setupCache(Axios, {
  debug: console.log // [!code ++]
});
```

```ts {5,11} [Skypack]
import Axios from 'https://cdn.skypack.dev/axios';

import { setupCache } from 'https://cdn.skypack.dev/axios-cache-interceptor'; // [!code --]
import { setupCache } from 'https://cdn.skypack.dev/axios-cache-interceptor/dev'; // [!code ++]

// same object, but with updated typings.
const axios = setupCache(Axios, {
  debug: console.log // [!code ++]
});
```

:::

::: tip

You only need to import from `/dev` where you import the `setupCache` function.

:::

::: details Sample of logs sent to console.

```json 
[
  {
    "id": "-644704205",
    "msg": "Sending request, waiting …",
    "data": { "overrideCache": false, "state": "empty" }
  },
  {
    "id": "-644704205",
    "msg": "Waiting list had an deferred for this key, waiting for it to finish"
  },
  {
    "id": "-644704205",
    "msg": "Detected concurrent request, waiting for it to finish"
  },
  {
    "id": "-644704205",
    "msg": "Useful response configuration found",
    "data": {
      "cacheConfig": {
        /*...*/
      },
      "cacheResponse": {
        "data": {
          /*...*/
        },
        "status": 200,
        "statusText": "OK",
        "headers": {
          /*...*/
        }
      }
    }
  },
  {
    "id": "-644704205",
    "msg": "Found waiting deferred(s) and resolved them"
  },
  {
    "id": "-644704205",
    "msg": "Returning cached response"
  },

  // First request ended, second call below:
  {
    "id": "-644704205",
    "msg": "Response cached",
    "data": {
      "cache": {
        /*...*/
      },
      "response": {
        /*...*/
      }
    }
  },
  {
    "id": "-644704205",
    "msg": "Returning cached response"
  }
]
```

:::

And much more, depending on your context, situation and configuration. **I'm sure any
misbehavior that you find will have a log to explain it.**
