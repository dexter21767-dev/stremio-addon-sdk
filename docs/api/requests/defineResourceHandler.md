## defineResourceHandler

This method currently handles addon catalog requests. As opposed to `defineCatalogHandler()` which handles meta catalogs, this method handles catalogs of addon manifests. This means that an addon can be used to just pass a list of other addons that can be installed in Stremio.


### Arguments:

`args` - request object; parameters described below

### Returns:

A promise that resolves to an object containing `{ addons: [] }` with an array of [Catalog Addon Object](../responses/addon_catalog.md)

The resolving object can also include the following cache related properties:

- `{ cacheMaxAge: int }` (in seconds) which sets the `Cache-Control` header to `max-age=$cacheMaxAge` and overwrites the global cache time set in `serveHTTP` [options](../../README.md#servehttpaddoninterface-options)

- `{ staleRevalidate: int }` (in seconds) which sets the `Cache-Control` header to `stale-while-revalidate=$staleRevalidate`

- `{ staleError: int }` (in seconds) which sets the `Cache-Control` header to `stale-if-error=$staleError`


## Request Parameters

``type`` - type of the catalog's content; e.g. `movie`, `series`, `channel`, `tv` (see [Content Types](../responses/content.types.md))

``id`` - string id of the catalog that is requested; these are set in the [Manifest Object](../responses/manifest.md)

``config`` - object with user settings, see [Manifest - User Data](../responses/manifest.md#user-data)


## Extra Parameters

```countryCode`` - set in the `extra` object; an [ISO 3166-1 Alpha 2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) string representing the country Code of the request, retrieved from the CDN if available, currently supported CDNs are: CloudFlare / Bunny CDN / CloudFront.


## Basic Example


```javascript
builder.defineResourceHandler('addon_catalog', function(args) {
    return Promise.resolve({
        addons: [
            {
                transportName: 'http',
                transportUrl: 'https://example.addon.org/manifest.json',
                manifest: {
                    id: 'org.myexampleaddon',
                    version: '1.0.0',
                    name: 'simple example',
                    catalogs: [],
                    resources: ['stream'],
                    types: ['movie'],
                    idPrefixes: ['tt']
                }
            }
        ]
    })
})
```

[Catalog Addon Object Definition](../responses/addon_catalog.md)
