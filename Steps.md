# Contributing to webhook-service

I will use one the drivers, `scaleHost`, that we are currently working on, as an example for these steps.

** Package Model **
- The webhook-service repository has a [model](https://github.com/rancher/webhook-service/tree/master/model) package with two files, `driver_model.go` and `framework_model.go`. 
 - In [driver_model.go](https://github.com/rancher/webhook-service/blob/master/model/driver_model.go), add a struct for your new driver, in the same way as `scaleService` has been added. All fields in your custom driver that are necessary for the webhook should be added to this struct. The new struct according to our example should be named something like scaleHost. One more field called `Type` should be added in addition to your fields. You can add `Type` by adding this line to your struct: 
 ```
 Type  string `json:"type,omitempty" mapstructure:"type"`
 ```
 - Now this struct needs to be added to the webhook framework by adding it to the file [framework_model.go](https://github.com/rancher/webhook-service/blob/master/model/framework_model.go) In this file, there's a struct [Webhook](https://github.com/rancher/webhook-service/blob/master/model/framework_model.go#L14). Its last line contains a field for the scaleService type from `driver_model.go`. You need to add a line for the newly added scaleHost type. You can do this by adding the following line to this struct:
 ```
 	ScaleHostConfig    ScaleHost    `json:"scaleHostConfig"`
 ```
- The webhook-service repository has a [drivers](https://github.com/rancher/webhook-service/tree/master/drivers) package, to which new a file should be added for adding a new driver. This package has two files as of now, so to add a new scaleHost driver, create a file `scale_host.go` in this package.
- There is another file, [framework.go](https://github.com/rancher/webhook-service/blob/master/drivers/framework.go) in the drivers package. You will have to register your new driver there in the same way as `scaleService` is registered in the [RegisterDrivers](https://github.com/rancher/webhook-service/blob/master/drivers/framework.go#L22) function. An interface called [WebhookDriver](https://github.com/rancher/webhook-service/blob/master/drivers/framework.go#L13) is defined in this file, and it shows all the functions that a driver should implement.
