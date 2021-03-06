# Unsplash Source JS

A javascript wrapper for the [Unsplash Source API](https://source.unsplash.com/). Get random Unsplash photos by keyword, location, category, user, or ID.

## Usage

Install `unsplash-source-node-js` with:

```bash
npm install unsplash
```

You can require `unsplash-source-node-js` like this:

```javascript
const UnsplashSource = require('unsplash-source-node-js');

// Or you can require the UnsplashPhoto object directly:

const UnsplashPhoto = require('unsplash-source-node-js').UnsplashPhoto;
```

Then create the photo you want using any of the chainable methods below, calling `fetch` as the last step to return the photo's URL.

Method | Arguments | Example | Description
-------|-----------|---------|------------
`find`|`"publicId"`|`photo.find("oMpAz-DN-9I")`|Finds a photo by its ID
`width`|`width`|`photo.width(2000)`|Sets the width in pixels
`height`|`height`|`photo.height(2000)`|Sets the height in pixels
`size`|`width, height`|`photo.size(1080,800)`|Shorthand for setting the width and height in pixels
`randomize`|`null or "daily" or "weekly"`|`photo.randomize("weekly")`|Sets the randomization interval
`fromUser`|`"username"`|`photo.fromUser("erondu")`|Limits to a specific photographer
`fromCategory`|`"category"`|`photo.fromCategory("nature")`|Limits to a specific category
`of`|`[arrayOfKeywords] or "keyword"`|`photo.of("dog")`|Limits to tags or locations matching the keywords
`all`||`photo.all()`|Searches for all photos, instead of just featured photos
`fetch`||`photo.fetch()`|Returns the configured URL.

By default, the photos are [filtered to featured photos](https://unsplash.com/documentation#get-a-random-photo). To remove this filter, call `all`. We recommend using `all` when limiting photos to a specific keyword (using `of`) or limiting photos to specific photographers (using `fromUser`) to maximize the possiblity of a matching photo.

Resize operations (`width`, `height`, `size`) maintain the aspect ratio of the original photo by cropping if necessary.

*Note*: Not all methods are compatible with each other. For example, trying to randomize a specific photo doesn't make sense. The wrapper will ignore incompatible methods and only construct URLs compatible with the Unsplash Source API.

## Examples

Get a random photo (the Unsplash Source API defaults to a width of 1080px):

```js
var photo = new UnsplashSource.UnsplashPhoto();

photo.fetch(); // => "https://source.unsplash.com/random"
```

Get a random featured photo that rotates weekly, cropped to `800px` x `600px`:

```js
var photo = new UnsplashSource.UnsplashPhoto();

photo.randomize("weekly")
     .size(800, 600)
     .fetch(); // => "https://source.unsplash.com/800x600/weekly"
```

Get a random photo from photographer [Jared Erondu](https://unsplash.com/erondu) cropped to `2048px` x `1200px`, that changes once a day:

```js
var photo = new UnsplashSource.UnsplashPhoto();

photo.all()
     .fromUser("erondu")
     .width(2048)
     .height(1200)
     .randomize("daily")
     .fetch(); // => "https://source.unsplash.com/user/erondu/2048x1200/daily"
```

Get a random nature photo of trees and water from the 'all' feed, cropped to `1000px` x `1200px`:

```js
var photo = new UnsplashSource.UnsplashPhoto();

photo.all()
     .fromCategory("nature")
     .of(["trees", "water"])
     .size(1000, 1200)
     .fetch(); // => "https://source.unsplash.com/category/nature/1000x1200?trees,water"
```

Get a specific photo (the photo ID matches the photo ID from unsplash.com):

```js
photo = new UnsplashSource.UnsplashPhoto();

photo.find("oMpAz-DN-9I")
     .fetch(); // => "https://source.unsplash.com/oMpAz-DN-9I"
```
