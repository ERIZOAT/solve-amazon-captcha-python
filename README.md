# How to Solve Amazon captcha using Python
Are you tired of being halted by CAPTCHA challenges while scraping data from Amazon? In this guide, we'll walk you through solving Amazon CAPTCHAs effortlessly using Python, with the help of Capsolver.

## Bonus Code
 A bonus code for [Capsolver](https://www.capsolver.com/): **AMN**. After redeeming it, you will get an extra 5% bonus after each recharge, Unlimited
 ![image](https://github.com/ERIZOAT/solve-amazon-captcha-python/assets/157081315/fc2a6424-a77d-48f1-a33d-458d1dfae0d9)


## Why Solve Amazon CAPTCHAs?

Amazon, like many other websites, employs CAPTCHA challenges to prevent automated bots from accessing their data. While CAPTCHAs serve a valuable security purpose, they can be a hindrance when you're trying to scrape product information or other data from the platform.

## Introducing Capsolver

Capsolver offers a powerful solution for automating CAPTCHA solving tasks. With its user-friendly API and extensive documentation, Capsolver makes it easy to integrate CAPTCHA solving capabilities into your Python scripts.

## Using Python to Solve Amazon CAPTCHAs

Below is a simple Python script that demonstrates how to use Capsolver to solve Amazon CAPTCHAs:

### Create Task

Create a task with the [createTask](../api-createtask.md) to create a task.

### Task Object Structure

| Properties               | Type   | Required | Description                                                                                                                                                                                                                                                                                                                |
|--------------------------|--------|----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| type                     | String | Required | `FunCaptchaTaskProxyLess`                                                                                                                                                                                                                                                                                                  |
| websiteURL               | String | Required | Web address of the website using funcaptcha, generally it's fixed value. (Ex: https://google.com)                                                                                                                                                                                                                          |
| websitePublicKey         | String | Required | The domain public key, rarely updated. (Ex: E8A75615-1CBA-5DFF-8031-D16BCF234E10)                                                                                                                                                                                                                                          |
| funcaptchaApiJSSubdomain | String | Optional | A special subdomain of [funcaptcha.com](http://funcaptcha.com/), from which the JS captcha widget should be loaded. Most FunCaptcha installations work from shared domains.                                                                                                                                                |
| data                     | String | Optional | Additional parameter that may be required by FunCaptcha implementation. Use this property to send "blob" value as a stringified array. See example how it may look like. {"\blob\":\"HERE_COMES_THE_blob_VALUE\"}  Learn [how to get FunCaptcha blob data](https://www.capsolver.com/blog/FunCaptcha/funcaptcha-data-blob) |
| proxy                    | String | Optional | Learn [Using proxies](../api-how-to-use-proxy)                                                                                                                                                                                                                                                                             |

### Example Request

``` json
POST https://api.capsolver.com/createTask
Host: api.capsolver.com
Content-Type: application/json

{
    "clientKey": "YOUR_API_KEY_HERE",
    "task": {
        "type":"FunCaptchaTaskProxyLess", //Required
        "websiteURL":"", //Required
        "websitePublicKey":"", //Required
        "data": "{\"blob\": \"flaR60YY3tnRXv6w.l32U2KgdgEUCbyoSPI4jOxU...\"}" // Optional
    }
}
```


After you submit the task to us, you should receive in the response a 'Task id' if it's successfull. Please
read [errorCode: full list of errors](https://captchaai.atlassian.net/wiki/spaces/CAPTCHAAI/pages/394062/FuncaptchaTask+solving+FunCaptcha#)
if you didn't receive the task id.

### Example Response

``` json
{
    "errorId": 0,
    "status": "idle",
    "taskId": "61138bb6-19fb-11ec-a9c8-0242ac110006"
}

```

### **Getting Result**

Use the [getTaskResult](../api-gettaskresult.md) method to get the recognition results

Depending on the system load, you will get the results within the interval of `1s` to `20s`

### Example Request

``` json
POST https://api.capsolver.com/getTaskResult
Host: api.capsolver.com
Content-Type: application/json

{
    "clientKey": "YOUR_API_KEY",
    "taskId": "61138bb6-19fb-11ec-a9c8-0242ac110006"
}
```

### Example Response

``` json
{
    "errorId": 0,
    "solution": {
        "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36",
        "token": "3AHJ_q25SxXT-pmSeBXjzScW-EiocHwwpwqtk1QXlJnGnU......"
    },
    "status": "ready"
}
```


## Solving Amazon Imagetotext



:::

### Create Task

Create the task with the [createTask](../api-createtask.md).

**Task Object Structure**

Note that this type of task returns the task execution result directly after createTask, rather than getting it
asynchronously through getTaskResult.

| Properties | Type    | Required | Description                                                                                            |
|------------|---------|----------|--------------------------------------------------------------------------------------------------------|
| type       | String  | Required | ImageToTextTask                                                                                        |
| websiteURL | String  | Optional | Page source url to improve accuracy                                                                    |
| body       | String  | Required | base64 encoded content of the image (no newlines) (no data:image/****\*****; base64, content           |
| module     | String  | Optional | Specifies the module. Currently, the supported modules are common and queueit                          |
| score      | Float   | Optional | `0.8 ~ 1`, Identify the matching degree. If the recognition rate is not within the range, no deduction |
| case       | Boolean | Optional | Case sensitive or not                                                                                  |

### Example Request

```text
POST https://api.capsolver.com/createTask
Host: api.capsolver.com
Content-Type: application/json
```

```json lines
{
  "clientKey": "YOUR_API_KEY",
  "task": {
    "type": "ImageToTextTask",
    "websiteURL": "https://xxxx.com",
    // You can choose the module you need to use
    // ocr single image model, default common
    "module": "queueit",
    // base64 encoded image
    "body": "/9j/4AAQSkZJRgABA......"
  }
}
```

### Example Response

```json lines
{
  "errorId": 0,
  "errorCode": "",
  "errorDescription": "",
  "status": "ready",
  "solution": {
    "text": "44795sds"
  },
  "taskId": "2376919c-1863-11ec-a012-94e6f7355a0b"
}
```

## Conclusion

With Capsolver and Python, solving Amazon CAPTCHAs becomes a breeze. By integrating Capsolver's API into your Python scripts, you can overcome CAPTCHA challenges and streamline your web scraping workflow. Say goodbye to CAPTCHA frustrations and hello to efficient data scraping on Amazon!

## Documentation 
[Capsolver API documentation](https://docs.capsolver.com/guide/api-server.html)
