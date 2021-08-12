## FloppySend OTP API

Integrating with our OTP API is easy. When you use FloppySend’s REST API to integrate SMS capabilities into your application, you’ll save hundreds of hours compared to building OTP functionality from the ground up.

The One-Time-Password (OTP) Interface is used to perform a mobile authentication or to build the second path in a Two-Factor-Authentication (2FA) environment.

With the OTP Interface the Customer can easily submit one time passwords to their end-users and verify their responses without needing to maintain a databas
  - Request a SMS PIN and send it to the Customer’s end-user
  - Verify the PIN that was entered by the end-user

### FloppySend OTP Request API

Request Types
  - POST METHOD ONLY: You need to append params as x-www-form-urlencoded inside the body of the request

```
https://api.floppy.ai/otp/request
```
```
{
    X-api-key:"Your x-api-key" (string required)
    X-Secret-Hash:"Your x-secret-hash" 
    (string required if added from dashboard)
}
```
You can get your X-Api-Key And X-Secret-Hash, from your [account setting](https://app.floppysend.com) page in Api Keys section 

#### Parameters
  | Parameter | Type | Required | Description |
  | --------- | ---- | -------- | ----------- |
  | To | String | Yes | |
  | From | String | Yes | |
  | Text | String | Yes | |
  | Pin_type | origin is set to numeric | Optional | only one of these three options {numeric, alpha, alphanumeric}) |
  | Pin_length | origin is set to 4 | Optional | only one of these three options {4,5,6} |
  | Max_amount | origin is set to 3 | Optional | only one of these three options {3,4,5} |
  

#### OTP Request Response
```JSON
{
    "Result": "Success",
    "Datainfo": 
    [
        {
            "Status": "Sent",
            "ID": "Response_Id",
            "Error_Code": "0",
            "Error_Desc": "0"
        }
    ]
}
```

#### OTP Request API Code Sample

```PHP
//PHP

<?php

$curl = curl_init();
curl_setopt_array($curl, array(
        CURLOPT_URL => 'https://api.floppy.ai/otp/request',
        CURLOPT_RETURNTRANSFER => true,
        CURLOPT_ENCODING => '',
        CURLOPT_MAXREDIRS => 10,
        CURLOPT_TIMEOUT => 0,
        CURLOPT_FOLLOWLOCATION => true,
        CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
        CURLOPT_CUSTOMREQUEST => 'POST',
        CURLOPT_POSTFIELDS =>
        'from={sender}&to={to_number}&text={message_body}%20%7Bpin%7D&pin_type=alphanumeric&pin_len
        gth=4&max_amount=3',
        CURLOPT_HTTPHEADER => array(
        'x-api-key: "Your x-api-key"',
        'Content-Type: application/x-www-form-urlencoded'
        ),
    ));
$response = curl_exec($curl);
curl_close($curl);
echo $response;
```

```ruby
//Ruby

require "uri"
require "net/http"
url = URI("https://api.floppy.ai/otp/request")
https = Net::HTTP.new(url.host, url.port)
https.use_ssl = true
request = Net::HTTP::Post.new(url)
request["x-api-key"] = "Your x-api-key"
request["Content-Type"] = "application/x-www-form-urlencoded"
request.body =
"from={sender}&to={to_number}&text={message_body}%20%7Bpin%7D&pin_type=alphanumeric&pin_len
gth=4&max_amount=3"
response = https.request(request)
puts response.read_body
```

```Java
//JAVA
//The OkHttpClient is an external package, you need to install it before making the request, 
//And don't forget to add user permission for the internet inside "\MyApplication\app\src\main\AndroidManifest.xml"
//Also, you need to add required dependencies inside "MyApplication\app\build.gradle"

OkHttpClient client = new OkHttpClient().newBuilder()
.build();
MediaType mediaType = MediaType.parse("application/x-www-form-urlencoded");
RequestBody body = RequestBody.create(mediaType,
"from={sender}&to={to_number}&text={message_body}
{pin}&pin_type=alphanumeric&pin_length=4&max_amount=3");
Request request = new Request.Builder()
.url("https://api.floppy.ai/otp/request")
.method("POST", body)
.addHeader("x-api-key", "Your x-api-key")
.addHeader("Content-Type", "application/x-www-form-urlencoded")
.build();
Response response = client.newCall(request).execute();
```

```C#
//C#

var client = new RestClient("https://api.floppy.ai/otp/request");
client.Timeout = -1;
var request = new RestRequest(Method.POST);
request.AddHeader("x-api-key", "Your x-api-key");
request.AddHeader("Content-Type", "application/x-www-form-urlencoded");
request.AddParameter("from", "{sender}");
request.AddParameter("to", "{to_number}");
request.AddParameter("text", "{message_body} {pin}");
request.AddParameter("pin_type", "alphanumeric");
request.AddParameter("pin_length", "4");
request.AddParameter("max_amount", "3");
IRestResponse response = client.Execute(request);
Console.WriteLine(response.Content);
```

```Javascript
//NodeJs
//Make sure to import Axios and install npm packages before doing the request

var axios = require('axios');
var qs = require('qs');
var data = qs.stringify({
    'from': 'SENDER',
    'to': 'NUMBER',
    'text': 'SAMPLE-TEXT{pin}',
    'pin_type': 'alphanumeric',
    'pin_length': '4',
    'max_amount': '4'
});
var config = {
    method: 'post',
    url: 'https://api.floppy.ai/otp/request',
    headers: {
        'x-api-key': 'YOUR-API-KEY',
        'Content-Type': 'application/x-www-form-urlencoded'
    },
    data: data
};

axios(config)
    .then(function (response) {
        console.log(JSON.stringify(response.data));
    })
    .catch(function (error) {
        console.log(error);
    });
```


### FloppySend OTP Response API

Request Types
  - POST METHOD ONLY: You need to append params as x-www-form-urlencoded inside the body of the request

```
https://api.floppy.ai/otp/validate
```

```
{
    X-api-key:"Your x-api-key" (string required)
    X-Secret-Hash:"Your x-secret-hash" 
    (string required if added from dashboard)
}
```

You can get your X-Api-Key And X-Secret-Hash, from your [account setting](https://app.floppysend.com) page in Api Keys section 

#### Parameters
  | Parameter | Type | Required |
  | --------- | ---- | -------- |
  | Pin | String | Yes |
  | Id | String | Yes |
  
  
#### OTP Response Response
```JSON
{
    "Result": "Success",
    "Datainfo": 
    [
        {
            "Status": "Valid",
            "ID": "Response_Id",
            "Error_Code": "0",
            "Error_Desc": "0"
        }
    ]
}
```

#### OTP Response API Code Sample

```PHP
//PHP

<?php

$curl = curl_init();
curl_setopt_array($curl, array(
        CURLOPT_URL => 'https://api.floppy.ai/otp/validate',
        CURLOPT_RETURNTRANSFER => true,
        CURLOPT_ENCODING => '',
        CURLOPT_MAXREDIRS => 10,
        CURLOPT_TIMEOUT => 0,
        CURLOPT_FOLLOWLOCATION => true,
        CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
        CURLOPT_CUSTOMREQUEST => 'POST',
        CURLOPT_POSTFIELDS => 'pin={pin}&id={id}',
        CURLOPT_HTTPHEADER => array(
        'x-api-key: "Your x-api-key"',
        'Content-Type: application/x-www-form-urlencoded'
        ),
    ));
$response = curl_exec($curl);
curl_close($curl);
echo $response;
```

```ruby
//Ruby

require "uri"
require "net/http"
url = URI("https://api.floppy.ai/otp/validate")
https = Net::HTTP.new(url.host, url.port)
https.use_ssl = true
request = Net::HTTP::Post.new(url)
request["x-api-key"] = "Your x-api-key"
request["Content-Type"] = "application/x-www-form-urlencoded"
request.body = "pin={pin}&id={id}"
response = https.request(request)
puts response.read_body
```

```Java
//JAVA
//The OkHttpClient is an external package, you need to install it before making the request, 
//And don't forget to add user permission for the internet inside "\MyApplication\app\src\main\AndroidManifest.xml"
//Also, you need to add required dependencies inside "MyApplication\app\build.gradle"

OkHttpClient client = new OkHttpClient().newBuilder()
.build();
MediaType mediaType = MediaType.parse("application/x-www-form-urlencoded");
RequestBody body = RequestBody.create(mediaType,
"pin={pin}&id={id}");
Request request = new Request.Builder()
.url("https://api.floppy.ai/otp/validate")
.method("POST", body)
.addHeader("x-api-key", "Your x-api-key")
.addHeader("Content-Type", "application/x-www-form-urlencoded")
.build();
Response response = client.newCall(request).execute();
```

```C#
//C#

var client = new RestClient("https://api.floppy.ai/otp/validate");
client.Timeout = -1;
var request = new RestRequest(Method.POST);
request.AddHeader("x-api-key", "Your x-api-key");
request.AddHeader("Content-Type", "application/x-www-form-urlencoded");
request.AddParameter("pin", "{pin}");
request.AddParameter("id", "{id}");
IRestResponse response = client.Execute(request);
Console.WriteLine(response.Content);
```

```Javascript
//NodeJs
//Make sure to import Axios and install npm packages before doing the request

var axios = require('axios');
var qs = require('qs');
var data = qs.stringify({
    'pin': 'PIN-FROM-SMS',
    'id': 'SMS-ID-FROM-OTPREQUEST'
});
var config = {
    method: 'post',
    url: 'https://api.floppy.ai/otp/validate',
    headers: {
        'x-api-key': 'YOUR-API-KEY',
        'Content-Type': 'application/x-www-form-urlencoded'
    },
    data: data
};

axios(config)
    .then(function (response) {
        console.log(JSON.stringify(response.data));
    })
    .catch(function (error) {
        console.log(error);
    });
```
