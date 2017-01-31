# Webhooks

## Webhook-service
Rancher has added a new feature in 1.4 for webhooks. Webhooks are a way of getting real time data of your application, and executing pre-defined actions for the application. All this is done without having to query the application periodically. We have implemented webhooks with our new microservice, called webhook-service. Webhook-service has drivers defined for actions to be taken in case a particular event occurs. I will explain this using our current driver, scaleService. The driver scaleService is for scaling up and down of a service by a given number of containers.

## Creating a webhook
Navigate to *API -> Webhooks* in the UI. This is where all the webhooks you create for the selected environment will be listed
