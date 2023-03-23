## defineSubtitlesHandler

This method handles subtitle requests.

### Arguments:

`args` - request object; parameters defined below

### Returns:

A promise resolving to an object containing `{ subtitles: [] }` with an array of [Subtitle Objects](../responses/subtitles.md).

The resolving object can also include the following cache related properties:

- `{ cacheMaxAge: int }` (in seconds) which sets the `Cache-Control` header to `max-age=$cacheMaxAge` and overwrites the global cache time set in `serveHTTP` [options](../../README.md#servehttpaddoninterface-options)

- `{ staleRevalidate: int }` (in seconds) which sets the `Cache-Control` header to `stale-while-revalidate=$staleRevalidate`

- `{ staleError: int }` (in seconds) which sets the `Cache-Control` header to `stale-if-error=$staleError`


## Request Parameters

``type`` - type of the item that we're requesting subtitles for; e.g. `movie`, `series`, `channel`, `tv` (see [Content Types](../responses/content.types.md))

``id`` -  string id of the video that we're requesting subtitles for (videoId); see [Meta Object](../responses/meta.md)

``extra`` - object that holds additional properties; parameters defined below

``config`` - object with user settings, see [Manifest - User Data](../responses/manifest.md#user-data)


## Extra Parameters

``videoHash`` - string [OpenSubtitles file hash](http://trac.opensubtitles.org/projects/opensubtitles/wiki/HashSourceCodes) for the video

``videoSize`` - size of the video file in bytes

``filename`` - filename of the video file

```countryCode`` - set in the `extra` object; an [ISO 3166-1 Alpha 2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) string representing the country Code of the request, retrieved from the CDN if available, currently supported CDNs are: CloudFlare / Bunny CDN / CloudFront.


## Basic Example

```javascript
builder.defineSubtitlesHandler(function(args) {
    if (args.id === 'tt1254207') {
        // serve one subtitle for big buck bunny
        const subtitle = {
            url: 'https://mkvtoolnix.download/samples/vsshort-en.srt',
            lang: 'eng'
        }
        return Promise.resolve({ subtitles: [subtitle] })
    } else {
        // otherwise return no subtitles
        return Promise.resolve({ subtitles: [] })
    }
})
```

[Subtitle Object Definition](../responses/subtitles.md)
