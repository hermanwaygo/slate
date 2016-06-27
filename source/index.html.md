---
title: Waygo API Reference

language_tabs:
  - shell
  - python

toc_footers:
  - <a href='http://waygoapp.com/api'>Sign Up for a Developer Key</a>

includes:
  - errors

search: true
---

# Introduction

The Waygo API allows you to extract text from images, also known as OCR, in a way that is automatic and scalable. The API currently supports text extraction of Chinese, English, Japanese and Korean. The [detect endpoint](#detect-text) returns the position, color and background color of detected text, while the [translate endpoint](#translate) does the same, with the addition of returning an image with the text replaced with their English translations. If you have any questions about the Waygo API or the documentation, please reach out to [sdk@waygoapp.com](mailto:sdk@waygoapp.com).

# Authentication

> To authorize, use this code:

```python
import waygo

api = waygo.authorize('yourapikey')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: yourapikey"
```

> Make sure to replace `yourapikey` with your API key.

Waygo uses API keys to allow access to the API. You can register a new Waygo API key at our [developer portal](http://waygoapp.com/api).

Waygo expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: yourapikey`

<aside class="notice">
You must replace <code>yourapikey</code> with your personal API key.
</aside>

# Image

## Detect text

```python
import waygo

api = waygo.authorize('yourapikey')
img = open("/path/to/image.jpg", "r")
api.image.detect(image=img, lc_src="zh")
```

```shell
curl \
  --request POST \
  --url http://waygoapp.com/api/v1/image/detect \
  --header 'cache-control: no-cache' \
  --header 'content-type: multipart/form-data' \
  --form 'image=@/path/to/image.jpg' \
  --form lcSrc=zh
```

> The above command returns JSON structured like this:

```json
[
  {
    "value": "漢堡",
    "shape": [
      {"x": 10, "y": 40}, 
      {"x": 100, "y": 40}, 
      {"x": 100, "y": 80},
      {"x": 10, "y": 80}
    ],
    "colors": {
      "fg": [0, 4, 6],
      "bg": [240, 245, 255]
    } 
  },
  {
    "value": "雞香堡",
    "shape": [
      {"x": 10, "y": 140}, 
      {"x": 130, "y": 140}, 
      {"x": 130, "y": 180},
      {"x": 10, "y": 180}
    ],
    "colors": {
      "fg": [0, 4, 6],
      "bg": [240, 245, 255]
    } 
  },
]
```

This endpoint detects the lines of text in an image, and returns the positions, colors and text of the detected text.

### HTTP Request

`POST http://waygoapp.com/api/v1/image/detect`

### POST Form Parameters

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
image     | Yes      | -       | The image to be used for detection, attached as part of a multipart-encoded request.
lcSrc     | Yes      | -       | The ISO language code of the source language, or in other words, the language of the text in the image. <br><br> The accepted language codes are: <ul><li>`en` (English)</li><li>`zh` (Chinese)</li><li>`ja` (Japanese)</li><li>`ko` (Korean)</li></ul><br>If `zh` is specified, the API will automatically handle both simplified and traditional Chinese text.


<aside class="success">
Remember — all requests require a valid API key for authentication.
</aside>

# Text

## Translate

```python
import waygo

api = waygo.authorize('yourapikey')
api.text.translate(text=["漢堡", "雞香堡"], lc_src="zh", lc_tgt="en")
```

```shell
curl "http://waygoapp.com/api/v1/text/translate"
  -H "Authorization: yourapikey"
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

