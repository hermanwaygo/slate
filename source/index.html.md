---
title: Waygo API Reference

language_tabs:
  - shell
  - python

toc_footers:
  - <a href='mailto:sdk@waygoapp.com'>Contact us for a Developer Key</a>

includes:
  - errors

search: true
---

# Introduction

The Waygo API allows you to extract text from images, also known as OCR, in a way that is automatic and scalable. The API currently supports text extraction of Chinese, English, Japanese and Korean. The [detect endpoint](#detect-text) returns the position, color and background color of detected text. If you have any questions about the Waygo API or the documentation, please reach out to [sdk@waygoapp.com](mailto:sdk@waygoapp.com).

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

Waygo uses API keys to allow access to the API. You can request a new Waygo API key by [contacting us](mailto:sdk@waygoapp.com).

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
  --header 'Authorization: yourapikey' \
  --form 'image=@/path/to/image.jpg' \
  --form lcSrc=zh
```

> The above command returns JSON structured like this:

```json
[
  {
    "value": "漢堡",
    "translation": "Hamburger",
    "romanization": "hàn bǎo",
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
    "translation": "Chicken Burger",
    "romanization": "jī xiāng bǎo",
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

This endpoint detects the lines of text in an image, and returns the positions, colors and text of the detected text. It will also return an English translation and an English romanization of the text, if the source language is not English. If the source language is English, the translation and romanization fields will currently merely mirror the value field.

### HTTP Request

`POST http://waygoapp.com/api/v1/image/detect`

### POST Form Parameters

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
image     | Yes      | -       | The image to be used for detection, attached as part of a multipart-encoded request.
lcSrc     | Yes      | -       | The ISO language code of the source language, or in other words, the language of the text in the image. <br><br> The accepted language codes are: <ul><li>`en` (English)</li><li>`zh` (Chinese)</li><li>`ja` (Japanese)</li><li>`ko` (Korean)</li></ul><br>If `zh` is specified, the API will automatically handle both simplified and traditional Chinese text.
lcTgt     | No       | en       | The ISO language code of the source language, or in other words, the language to translate to. Currently, the only valid option is `en` (English), and the parameter is not required.


<aside class="success">
Remember — all requests require a valid API key for authentication.
</aside>
