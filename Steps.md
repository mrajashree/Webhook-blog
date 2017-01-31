# Contributing to webhook-service

Lets use `addHost` as an example driver for going through the steps on adding custom drivers

**Package Model**
- The webhook-service repository has a [model](https://github.com/rancher/webhook-service/tree/master/model) package with two files, `driver_model.go` and `framework_model.go`. 
 - In [driver_model.go](https://github.com/rancher/webhook-service/blob/master/model/driver_model.go), add a type struct for your new driver, in the same way as `scaleService` has been added. All fields in your custom driver that are necessary for the webhook should be added in your struct. The new struct according to our example should be named something like addHost. One more field called `Type` should be added in addition to your fields. You can add `Type` by adding this line to your struct: 
 ```
 Type  string `json:"type,omitempty" mapstructure:"type"`
 ```
 - Now, the newly added type `addHost` needs to be added to the webhook framework by adding it to the file [framework_model.go](https://github.com/rancher/webhook-service/blob/master/model/framework_model.go) In this file, there's a struct [Webhook](https://github.com/rancher/webhook-service/blob/master/model/framework_model.go#L14). Its last line contains a field for the scaleService type from `driver_model.go`. You need to add the following line to this struct:
 ```
 	AddHostConfig    addHost    `json:"addHostConfig"`
 ```

**Package Driver**
- The webhook-service repository has a [drivers](https://github.com/rancher/webhook-service/tree/master/drivers) package. You need to register the new driver in this package. For that, the file [framework.go](https://github.com/rancher/webhook-service/blob/master/drivers/framework.go) needs to be modified. In this file, add your driver in the [RegisterDrivers](https://github.com/rancher/webhook-service/blob/master/drivers/framework.go#L22) functions, using this example line:
```
Drivers["addHost"] = &AddHostDriver{}
```
- An interface called [WebhookDriver](https://github.com/rancher/webhook-service/blob/master/drivers/framework.go#L13) is defined in this file. It lists all functions to be implemented by every driver. So to add your custom `addHost` driver, create a new file in the drivers package, called `add_host.go`.
- You can use our file [scale_service.go](https://github.com/rancher/webhook-service/blob/master/drivers/scale_service.go) as an example for your new file. 
 - It will have the type `AddHostDriver` just like the `ScaleServiceDriver`. 
 - The `ValidatePayload` function is where you can convert the request data to your defined type for the driver, and confirm if all the fields are of the required type.
 - The `Execute` function is where you will add code to carry out the actual addition of hosts, and you can use the apiClient passed from the framework for making cattle API calls
 - `ConvertToConfigAndSetOnWebhook` is a function required by the framework to get all the driver information. This is what your example function will look like:
 ```
 func (a *AddHostDriver) ConvertToConfigAndSetOnWebhook(conf interface{}, webhook *model.Webhook) error {
	if addHostConfig, ok := conf.(model.AddHost); ok {
		webhook.AddHostConfig = addHostConfig
		webhook.AddHostConfig.Type = webhook.Driver
		return nil
	} else if configMap, ok := conf.(map[string]interface{}); ok {
		config := model.AddHost{}
		err := mapstructure.Decode(configMap, &config)
		if err != nil {
			return err
		}
		webhook.AddHostConfig = config
		webhook.AddHostConfig.Type = webhook.Driver
		return nil
	}
	return fmt.Errorf("Can't convert config %v", conf)
}
 ```
 - `GetDriverConfigResource` just returns the driver type when requested.
 - `CustomizeSchema` is to set default values or change the type of some fields. You can leave it blank for now and just add a line `return schema` to it.
