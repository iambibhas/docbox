## Get an Access Token

Once you have the Client ID and Client Secret, you have to generate an access token to use our API end points. Generating a token requires a POST request to the OPEN ID Server with an URL encoded Form.

Property | Description
---|---
`grant_type` | password or client_credentials
`username` | required if grant_type is password
`password` | required if grant_type is password
`client_id` | required
`client_secret` | required
`scope` | image_matching


```endpoint
POST https://openid.wundercart.de/connect/token
```

#### Example request

```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "https://openid.wundercart.de/connect/token",
  "method": "POST",
  "headers": {
    "content-type": "application/x-www-form-urlencoded",
    "cache-control": "no-cache"
  },
  "data": {
    "grant_type": "client_credentials",
    "client_id": "your_client_id",
    "client_secret": "your_client_secret",
    "scope": "image_matching"
  }
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

```php
<?php

$curl = curl_init();

curl_setopt_array($curl, array(
  CURLOPT_URL => "https://openid.wundercart.de/connect/token",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => "",
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 30,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => "POST",
  CURLOPT_POSTFIELDS => "grant_type=client_credentials&client_id=your_client_id&client_secret=your_client_secret&scope=image_matching",
  CURLOPT_HTTPHEADER => array(
    "cache-control: no-cache",
    "content-type: application/x-www-form-urlencoded"
  ),
));

$response = curl_exec($curl);
$err = curl_error($curl);

curl_close($curl);

if ($err) {
  echo "cURL Error #:" . $err;
} else {
  echo $response;
}
```

```csp
using System.IO.Compression;

#pragma warning disable 414, 3021

namespace MyApplication
{
    [Obsolete("...")]
    class Program : IInterface
    {
        public static List<int> JustDoIt(int count)
        {
            Console.WriteLine($"Hello {Name}!");
            return new List<int>(new int[] { 1, 2, 3 })
        }
    }
}
```

```python
import http.client

conn = http.client.HTTPSConnection("openid.wundercart.de")

payload = "grant_type=client_credentials&client_id=your_client_id&client_secret=your_client_secret&scope=image_matching"

headers = {
    'content-type': "application/x-www-form-urlencoded",
    'cache-control': "no-cache"
    }

conn.request("POST", "/connect/token", payload, headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
OkHttpClient client = new OkHttpClient();

MediaType mediaType = MediaType.parse("application/x-www-form-urlencoded");
RequestBody body = RequestBody.create(mediaType, "grant_type=client_credentials&client_id=your_client_id&client_secret=your_client_secret&scope=image_matching");
Request request = new Request.Builder()
  .url("https://openid.wundercart.de/connect/token")
  .post(body)
  .addHeader("content-type", "application/x-www-form-urlencoded")
  .addHeader("cache-control", "no-cache")
  .build();

Response response = client.newCall(request).execute();
```

#### Example response

```json
{
    "token_type": "Bearer",
    "access_token": "your-unique-access-token",
    "expires_in": 2592000
}
```


## Match an Image

Once you have acquired the access token, it is time now to send an image to the server. The server will process the image and return a JSON object with default values or with the values defined in your template.

```endpoint
POST https://imagematching.wundercart.de/api/find
```

**Headers**

Key | Value
---|---
`Authorization` | Bearer + 'access_token'
`Content-Type` | image/jpeg
`Content-Length` | the size of image in bytes

**Body**

The Body of the request should contain the image file's actual bytes sent in binary.

**Note**

*  The General Image Format should be in JPG
*  The longer side should be 512px. Bigger images will be accepted as well.
*  The image size should not exceed 300kb.

#### Example Request


```python
import http.client

conn = http.client.HTTPSConnection("imagematching.wundercart.de")

headers = {
    'authorization': "Bearer access-token",
    'content-type': "image/jpeg",
    'content-length': "image-filesize-in-bytes",
    'cache-control': "no-cache"
    }

conn.request("POST", "/api/find", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "https://imagematching.wundercart.de/api/find",
  "method": "POST",
  "headers": {
    "authorization": "Bearer access-token",
    "content-type": "image/jpeg",
    "content-length": "image-file-size-in-bytes",
    "cache-control": "no-cache"
  }
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

```php
<?php
$data = file_get_contents("image.jpg");

$curl = curl_init();

curl_setopt_array($curl, array(
  CURLOPT_URL => "https://imagematching.wundercart.de/api/find",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_BINARYTRANSFER => 1,
  CURLOPT_PUT => 1,
  CURLOPT_INFILE => $data,
  CURLOPT_INFILESIZE => filesize("filename"),
  CURLOPT_ENCODING => "",
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 30,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => "POST",
  CURLOPT_HTTPHEADER => array(
    "authorization: Bearer access-token",
    "cache-control: no-cache",
    "content-length: image-file-size-in-bytes",
    "content-type: image/jpeg"
  ),
));

$response = curl_exec($curl);
$err = curl_error($curl);

curl_close($curl);

if ($err) {
  echo "cURL Error #:" . $err;
} else {
  echo $response;
}
```

```java
OkHttpClient client = new OkHttpClient();

Request request = new Request.Builder()
  .url("https://imagematching.wundercart.de/api/find")
  .post(null)
  .addHeader("authorization", "Bearer access-token")
  .addHeader("content-type", "image/jpeg")
  .addHeader("content-length", "image-file-size-in-bytes")
  .addHeader("cache-control", "no-cache")
  .build();

Response response = client.newCall(request).execute();
```

```csp
var imageMatchingUri="https://imagematching.wundercart.de/api/find";
//...
var token = await GetAccessTokenAsync(client, tokenEndpoint, formContent); // remember to get the token first
//...
var offer = await PostImageToMatchingServerAsync(client, imageMatchingUri, token, imgPath);
//...
public static async Task<JObject> PostImageToMatchingServerAsync(HttpClient client, string address, string token, FileInfo imgPath)
{
    using (var stream = imgPath.Open(FileMode.Open, FileAccess.Read, FileShare.Read))
    using (var content = new StreamContent(stream))
    {
        //the actual request
        var request = new HttpRequestMessage(HttpMethod.Post, address);
        request.Content = content;
        request.Content.Headers.Add("Content-Type", "image/jpeg");
        request.Content.Headers.Add("Content-Length", stream.Length.ToString());
        request.Headers.Accept.Add(new MediaTypeWithQualityHeaderValue("application/everybag.offers+json", 1));
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", token);

        using (var response = await client.SendAsync(request).ConfigureAwait(false))
        {
            if(response.IsSuccessStatusCode)
            {
                var responseContent = await response.Content.ReadAsStringAsync();
                var offerResponse = JObject.Parse(responseContent);
                return offerResponse;
            }
            else
            {
                //handle error
            }
        }
    }
}
```


#### Example response

```json
{
  "results": [
    {
      "merchant_id": "...",
      "merchant_name": "...",
      "id": "...",
      "sku": "..."
    },
    {
        ...
    }
  ]
}
```
