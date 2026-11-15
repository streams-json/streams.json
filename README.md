# streams.json
`streams.json` is a file store in the site root use for stream url for radio, tv, webradio or webtv.

## What is streams.json for?

It allows to list stream url for radio, tv, webradio or webtv. It is a JSON file that contains information about the different streams of the website.

## Why a JSON file?

Because it's something quick and easy to create. It also allows you to have more information about your streams. With our tool you can generate your `streams.json` file easily.

## Where is it located?

**In the site root.** Just next to the robots.txt file or favicon.ico.
Like: ```https://domain.tld/streams.json```

## Is it mandatory?

No, you can put it if you want and if you need it.
In the near futur we would like directories use it to update or save informations automatically.

## ISO types for Languages, Country and Location(s)

Language: [2-letter ISO-639-1 Codes](https://en.wikipedia.org/wiki/ISO_639-1), lower case (`fr`)
Country and Location(s): [ISO 3166-1 Alpha-2 code](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2), upper case (`FR`)

## Format (version 0.5.0)

The format is defined by a [JSON Schema](https://json-schema.org/): [`streams.schema.json`](streams.schema.json). A complete example is available in [`streams.json`](streams.json).

Lists are always arrays, even when they hold a single item, and unknown properties are rejected.

### Root

| Property | Type | Required | Description |
| --- | --- | --- | --- |
| `$schema` | string (URL) | no | URL of the schema, enables validation and autocompletion in editors |
| `version` | string | **yes** | Version of the format, `0.5.0` |
| `generated_at` | string (RFC 3339 date-time) | no | When the file was generated |
| `name` | string | **yes** | Name of the broadcaster or network |
| `description` | string | no | Short description |
| `url` | string (URL) | no | Website |
| `location` | string (country code) | no | Country of the broadcaster |
| `city` | string | no | City of the broadcaster |
| `languages` | array of language codes | no | Languages of the broadcaster |
| `logo` | array of [logos](#logo) | no | Logos of the broadcaster |
| `stations` | array of [stations](#station) | **yes** | At least one station |

### Station

| Property | Type | Required | Description |
| --- | --- | --- | --- |
| `name` | string | **yes** | Name of the station |
| `description` | string | no | Short description |
| `genre` | string | no | Genre, as free text (`Pop, Rock`) |
| `url` | string (URL) | no | Web page of the station |
| `languages` | array of language codes | no | Languages of the station |
| `logo` | array of [logos](#logo) | no | Logos of the station |
| `streams` | array of [streams](#stream) | **yes** | At least one stream, one per format and bitrate |

### Stream

| Property | Type | Required | Description |
| --- | --- | --- | --- |
| `type` | string | **yes** | Format in lower case (`mp3`, `aac`, `ogg`, `hls`…) |
| `bitrate` | integer | no | Bitrate in kbit/s |
| `server` | array of [servers](#server) | **yes** | At least one server delivering this stream |

### Server

| Property | Type | Required | Description |
| --- | --- | --- | --- |
| `url` | string (URL) | **yes** | URL of the stream on this server |
| `priority` | integer | no | Order in which servers should be tried, `1` first |
| `location` | string (country code) | no | Country the server is located in |

### Logo

| Property | Type | Required | Description |
| --- | --- | --- | --- |
| `url` | string (URL) | **yes** | URL of the image |
| `type` | string | no | Shape of the logo (`square`) |
| `size` | integer | no | Width in pixels |

## Validate my streams.json file

With [check-jsonschema](https://github.com/python-jsonschema/check-jsonschema):

```bash
pip install check-jsonschema
check-jsonschema --schemafile https://raw.githubusercontent.com/streams-json/streams.json/main/streams.schema.json streams.json
```

Any other [JSON Schema validator](https://json-schema.org/tools) supporting draft 2020-12 works too.

## Migrating from 0.4.0

- Replace the `_streams.json_*_` headers with `version` (and optionally `$schema` and `generated_at`). `_streams.json_info_` and `_streams.json_url_` are removed.
- `streams` is always an array, even with a single stream.
- `server` is always an array, even with a single server.
- A stream no longer has a `url` property: each URL goes in a `server` entry, with a `priority` if the order matters.

## Create / Update / Check my streams.json file

On [our website](https://streams-json.rocketip.net) you can:
- generate your streams.json file
- update your streams.json file to the last version to be up to date
- check your streams.json file

## Create playlists from my streams.json file

The [playlist generator](https://streams-json.github.io/playlist-php/) turns a streams.json file into `m3u` or `xspf` playlists, one per stream, packed in a zip file. It runs in the browser; a command line version is available in [playlist-php](https://github.com/streams-json/playlist-php).

To let it, and other web tools, read your file from its URL, serve `streams.json` with the `Access-Control-Allow-Origin: *` header.

## Sites using streams.json

- [MOROW](https://www.morow.com)
- and many more…

Are you using streams.json? [Add your website](https://github.com/streams-json/streams.json/edit/main/WEBSITES.md) to the list!
