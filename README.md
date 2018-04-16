# WooCommerce API - Golang Client

A Golang wrapper for the WooCommerce REST API. Easily interact with the WooCommerce REST API using this library.
This is a fork of `mikespook/wc-api-golang`. Main difference: get/post/put/delete accepts context and package uses
`http.DefaultClient` and `http.DefaultTransport`.

This lib is **not** backward compatible with mikespook/wc-api-golang`!

## Installation

```bash
$ go get github.com/darh/wc-api-golang/woocommerce
```

## Getting started

Generate API credentials (Consumer Key & Consumer Secret) following this instructions <http://docs.woocommerce.com/document/woocommerce-rest-api/>
.

Check out the WooCommerce API endpoints and data that can be manipulated in <https://woocommerce.github.io/woocommerce-rest-api-docs/>.

## Setup

Setup for the new WP REST API integration (WooCommerce 2.6 or later):

```golang
import (
  wc "github.com/darh/wc-api-golang/woocommerce"
)

var woocommerce = wc.NewClient(
    "http://example.com", 
    "ck_XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX", 
    "cs_XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
    &wc.Options {
        API: true,
        Version: "wc/v1",
    },
);
```

### Paramaters

|       Option      |   Type   |                Description                 |
| ----------------- | -------- | ------------------------------------------ |
| `store`           | `string` | Your Store URL, example: http://woo.dev/   |
| `ck`              | `string` | Your API consumer key                      |
| `cs`              | `string` | Your API consumer secret                   |
| `options`         | `*wc.Options`  | Extra arguments (see client options table) |

#### Client options

|        Option       |   Type   |                                                      Description                                                       |
|---------------------|----------|------------------------------------------------------------------------------------------------------------------------|
| `API`            | `bool`   | Allow make requests to the new WP REST API integration (WooCommerce 2.6 or later)                                      |
| `APIPrefix`     | `string` | Custom WP REST API URL prefix, used to support custom prefixes created with the `rest_url_prefix` filter               |
| `Version`           | `string` | API version, default is `v3`                                                                                           |
| `Timeout`           | `time.Duration`    | Request timeout, default is `15`                                                                                       |
| `VerifySSL`        | `bool`   | Verify SSL when connect, use this option as `false` when need to test with self-signed certificates, default is `true` |
| `QueryStringAuth` | `bool`   | Force Basic Authentication as query string when `true` and using under HTTPS, default is `false`                       |
| `OauthTimestamp`   | `time.Time` | Custom oAuth timestamp, default is `time.Now()`                                                                            |

## Methods

|    Params    |   Type   |                         Description                          |
| ------------ | -------- | ------------------------------------------------------------ |
| `endpoint`   | `string` | WooCommerce API endpoint, example: `customers` or `order/12` |
| `data`       | `interface{}`  | Only for POST and PUT, data that will be converted to JSON   |
| `parameters` | `url.Values`  | Only for GET and DELETE, request query string                |

### GET

```golang
rc, err := woocommerce.Get(ctx, endpoint, parameters)
```

### POST

```golang
rc, err := woocommerce.Post(ctx, endpoint, data)
```

### PUT

```golang
rc, err := woocommerce.Put(ctx, endpoint, data)
```

### DELETE

```golang
rc, err := woocommerce.Delete(ctx, endpoint, parameters)
```

### OPTIONS

```golang
rc, err := woocommerce.Options(ctx, endpoint)
```

#### Response

All methods will return a `io.ReadCloser` and `nil` on success or an `error` on failure.

## Release History
