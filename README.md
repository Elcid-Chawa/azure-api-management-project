# azure-api-management-project

# Project: My Weather API

## Project Introduction

## Project Summary

You have been tasked to provide API management to a new weather API your company is developing! The API is fully functional, but you need a way to provide access to the API. Azure API Management has been chosen to provide this access. You have requirements from security that the API must be secured by authentication through subscription keys. Security would also like policies implemented to throttle API requests so malicious actors cannot overwhelm the API. Additionally, security would like certain headers deleted from API responses to mask the service even further. And finally - the API must have the ability to be monitored through metrics, logs and alerts.

## Peliminary Steps

- Open Azure CloudShell
- Select Bash
- Create New for the storage account with name tscottoudacityazure and file share name tscottoudacityfile
- Click Create storage
  ![create storage](./screenshots/create_storege.png)

- Run the following in CloudShell:

```
git clone https://github.com/udacity/azure-api-management
```

- Navigate to the project starter folder cd azure-api-management/starter

- Make the script executable

```
chmod +x setup.sh
```

- Run ./setup.sh (I had to switch from powershell to bash to run this)

```
./setup.sh
```

The script will output a URL of the weather app. This is our backend of our API. Copy this URL. It will look something like this https://WeatherDataAPId72d5e4a32.azurewebsites.net/swagger/v1/swagger.json(opens in a new tab) If you loose your URL you can find it by searching the Azure portal for "weatherdataapi" selecting the web app and copying the URL. Do not forget to append

## Creatig API service

Screenshots and steps

1.  Create an API instance via PowerShell
    ![Create_API_Instance](./screenshots/Create_API_Instance.png)
    ![API_Instance](./screenshots/api_service_online.png)

2.  Import the weather API into API management using the OpenAPI standard

    ![Import_weather_API](./screenshots/Import_weather_API.png)

3.  GET operation on the API ( GetTodaysWeather ). Using 1 and 1 for the latitude and longitude.
    ![test_api](./screenshots/test_api.png)

4.  Applying policies that throttles traffic to the API to 3 calls every 50 seconds AND strips the following header from the API for ALL operations.

    - X-Powered-By
      ![remove_header](./screenshots/remove_header.png) ![remove_header set](./screenshots/remove_header_set.png)
    - Perform a test on any operation and show that the header has been removed from the response
      ![header removed](./screenshots/header_removed.png)
    - Use context.Subscription.Id as the counter key for throttling
      ![set trottle](./screenshots/set_trottle.png)
    - Perform a test to prove the throttling is working by performing 4 test operations quickly on any API endpoint
      ![test trottle](./screenshots/test_trottle.png)

5.  Applying subscription authentication by assigning the API to a product called Udacity
    ![apply athentication](./screenshots/apply_authentication.png)
    ![subscription](./screenshots/subscription.png)

    - Perform the following curl test (change the URL to reflect your API) to prove the subscription authentication is working:

    ```bash
    curl -X GET https://udacity-elcid-beta1.azure-api.net/api/Weather/1/1

    curl -X GET https://udacity-elcid-beta1.azure-api.net/api/Weather/1/1 -H 'Ocp-Apim-Subscription-Key: <subscrition key>'
    ```

    ![test subscription](./screenshots/test_subscription.png)

6.  Create an email alert when there are over 10 4xx alerts within a 1 minute period
    ![notification](./screenshots/notification.png)

        - Test this alert by accessing an invalid path to your API 10 times in a row (generating a 404) and seeing the alert fire

        ![faulty requests](./screenshots/faulty_requests.png)

        - alert fired
            ![alert_fired](./screenshots/alert_fired.png)
            ![alert_fired1](./screenshots/alert_fired1.png)
        - Email notification
        ![alert_fired_email](./screenshots/alert_fired_email.png)
        ![alert_fired_email1](./screenshots/alert_fired_email2.png)

Closing all instaces after completion
