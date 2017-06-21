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
//Build the form we're gonna put into the request 
var tokenEndpoint="https://openid.wundercart.de/connect/token";
var formDict = new Dictionary<string, string>();
formDict.Add("grant_type", "password");
formDict.Add("username", "myUsername");
formDict.Add("password", "myPassword");
formDict.Add("client_id", "myClientID");
formDict.Add("scope", "image_matching");
var formContent = new FormUrlEncodedContent(formDict);
var token = await GetAccessTokenAsync(client, tokenEndpoint, formContent);
//... 
public static async Task<string> GetAccessTokenAsync(HttpClient client, String address, FormUrlEncodedContent form)
{
    var request = new HttpRequestMessage(HttpMethod.Post,address);
    request.Content=form;
    using (var response = await client.SendAsync(request))
    {
        if (!response.IsSuccessStatusCode)
        {
            //Handle error 
        }
        var responseContent = await response.Content.ReadAsStringAsync();
        var tokenResponse = JObject.Parse(responseContent);
        var token = tokenResponse.GetValue("access_token").Value<string>();
        return token;
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
    "access_token": "eyJhbGciOiJSUzI1NiIsImtpZCI6IjMwN0RCNTQ1NUU1NjA4OEMzQTA1OEY4QzEzNkFDOEIyRDk3ODBEQUUiLCJ0eXAiOiJKV1QifQ.eyJqdGkiOiJkMmIwNTY3Yy1iNjRmLTQzNTUtYWMzNy04OThmNjRiZDk3NzQiLCJ1c2FnZSI6ImFjY2Vzc190b2tlbiIsImNmZF9sdmwiOiJwcml2YXRlIiwic2NvcGUiOiJpbWFnZV9tYXRjaGluZyIsInN1YiI6IjgwZTJhNmIzZTY4MWNmYTAzODFlNGRhZSIsImF1ZCI6Imh0dHBzOi8vYXBpLnd1bmRlcmNhcnQuZGUiLCJhenAiOiI4MGUyYTZiM2U2ODFjZmEwMzgxZTRkYWUiLCJuYmYiOjE0OTczNzMxMDcsImV4cCI6MTQ5OTk2NTEwNywiaWF0IjoxNDk3MzczMTA3LCJpc3MiOiJodHRwczovL29wZW5pZC53dW5kZXJjYXJ0LmRlLyJ9.K0VRhlGuzvRKKLTwnEAk0VX0t534D25xy4MBXrB8fNQ5wuUgvbSCjA_webuWQZCpKYw7lojP46lry0JTh7wj0sb6BCj0JOGEMOkjbKZzExsyYbAhOhrC1sXysY1b2dPUkcVeRC13N2W5BXfNiNuG2OXMGt9eiFqIJDnIycCdk1KRYwXXRja-z4cVH8DoQCn8j9-K1jqDEYm82Jvt9U7_96gobYagttYma1s7YV2XQdYgANyPb1VdIxO6d4-Dm8KiL044APsre6GGAO9HGmUOz1Xj52gYAtLJpuWru1lyvoWdN7_wRgEdGxHmAS-85o14eOkoLDAd2Pemi1e1u0Hp1g",
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
public static async Task<JObject> PostImageToMatchingServerAsync(HttpClient client, string address, string token, string imgPath)
{
    using (var stream = File.Open(imgPath, FileMode.Open, FileAccess.Read, FileShare.Read))
    using (var memstream = new MemoryStream())
    {
        //turn the image into a memory stream to add to the request 
        await stream.CopyToAsync(memstream);
        memstream.Seek(0,SeekOrigin.Begin);
        var content = new StreamContent(memstream);
 
        //the actual request 
        var request = new HttpRequestMessage(HttpMethod.Post, address);
        request.Content = content;
        request.Content.Headers.Add("Content-Type", "image/jpeg");
        request.Content.Headers.Add("Content-Length", memstream.Length.ToString());
        request.Headers.Accept.Add(new MediaTypeWithQualityHeaderValue("application/everybag.offers+json", 1)); //this makes the server return a template 
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