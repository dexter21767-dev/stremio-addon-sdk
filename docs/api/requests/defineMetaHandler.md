## defineMetaHandler

This method handles metadata requests. (title, releaseInfo, poster, background, etc.)

### Arguments:

`args` - request object; parameters described below

### Returns:

A promise resolving to an object containing `{ meta: {} }` with a [Meta Object](../responses/meta.md)

The resolving object can also include the following cache related properties:

- `{ cacheMaxAge: int }` (in seconds) which sets the `Cache-Control` header to `max-age=$cacheMaxAge` and overwrites the global cache time set in `serveHTTP` [options](../../README.md#servehttpaddoninterface-options)

- `{ staleRevalidate: int }` (in seconds) which sets the `Cache-Control` header to `stale-while-revalidate=$staleRevalidate`

- `{ staleError: int }` (in seconds) which sets the `Cache-Control` header to `stale-if-error=$staleError`


## Request Parameters

``type`` - type of the item; e.g. `movie`, `series`, `channel`, `tv` (see [Content Types](../responses/content.types.md))

``id`` - string id of the meta item that is requested; these are set in the [Meta Preview Object](../responses/meta.md#meta-preview-object)

``config`` - object with user settings, see [Manifest - User Data](../responses/manifest.md#user-data)


## Extra Parameters

```countryCode`` - set in the `extra` object; an [ISO 3166-1 Alpha 2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) string representing the country Code of the request, retrieved from the CDN if available, currently supported CDNs are: CloudFlare / Bunny CDN / CloudFront.


## Basic Example

```javascript
builder.defineMetaHandler(function(args) {
    if (args.type === 'movie' && args.id === 'tt1254207') {
        // serve metadata for Big Buck Bunny
        const metaObj = {
            id: 'tt1254207',
            name: 'Big Buck Bunny',
            releaseInfo: '2008',
            poster: 'https://image.tmdb.org/t/p/w600_and_h900_bestv2/uVEFQvFMMsg4e6yb03xOfVsDz4o.jpg',
            posterShape: 'poster',
            type: 'movie'
        }
        return Promise.resolve({ meta: metaObj })
    } else {
        // otherwise return no meta
        return Promise.resolve({ meta: {} })
    }
})
```

[Meta Object Definition](../responses/meta.md)
