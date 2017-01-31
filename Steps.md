# Contributing to webhook-service

I will use one the drivers, `scaleHost`, that we are currently working on, as an example for these steps.
- The webhook-service repository has a [drivers](https://github.com/rancher/webhook-service/tree/master/drivers) package, to which new a file should be added for adding a new driver. This package has two files as of now, so to add a new scaleHost driver, create a file `scale_host.go` in this package.
- There is another file, [framework.go](https://github.com/rancher/webhook-service/blob/master/drivers/framework.go) in the drivers package. You will have to register your new driver there in the same way as `scaleService` is registered in the [RegisterDrivers](https://github.com/rancher/webhook-service/blob/master/drivers/framework.go#L22) function
