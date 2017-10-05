---
layout: post
title: "Golang: Returning errors with context"
date: 2017-10-5 11:55:46 +0530
comments: true
categories: Golang Notes Errors
---

Usually in Golang, it is recommended that, while propagating errors outside, just return the error do not log it. [This](https://dave.cheney.net/2015/11/05/lets-talk-about-logging) blog by Dave Cheney talks in length about how shall we handle logging and errors. But while returning/propagating errors, somtimes it becomes necessary to add the context of the error along with the actual error.

**Basic example is:**

Consider your web app is talking to another gRPC server for getting location information through a function called, `GetLocationFor(user)`. Now there could be long enough function call tree that lands us onto this function. So if something goes wrong with gRPC connection, and if we return the error as is, technically we have lost the context.

{% highlight golang %}
func GetLocationFor(u *User) (*Location,error){
  respMsg, err := grpcClient.GetLocationFor(u.name)
  if err != nil{
    // here we directly send the error 
    return nil, err 
  }
  // process the respMsg and move on
}
{% endhighlight %}

So there is one better way to just return the error (without logging it) and keeping the context:

Consider following supporting method for creating error objects

{% highlight golang %}
func New(args ...interface{}) error {
	var err error
	var rawData []interface{}
	for _, arg := range args {
		switch arg.(type) {
		case error:
			err = arg.(error)
			log.Println("error", err)
			continue
		default:
			rawData = append(rawData, arg)
		}
	}
	if err == nil {
		err = errors.New(fmt.Sprintf("%v", rawData))
	}
	return errors.New(fmt.Sprintf("%v [error => %s]", rawData, err.Error()))
}
{% endhighlight %}

And use it as

{% highlight golang %}
import github.com/akshaydeo/errors

func GetLocationFor(u *User) (*Location,error){
  respMsg, err := grpcClient.GetLocationFor(u.name)
  if err != nil{
    // here we directly send the error 
    return nil, errors.New("while getting location from grpc client in GetLocationFor", err)
  }
  // process the respMsg and move on
}
{% endhighlight %}

After this, whenver you log the error it will be something like `while getting location from grpc client in GetLocatioFor [error => <original_error_message_from_grpc_client>]`

This keeps the context of the error very specific and makes it easier to pinpoint the exact issue.

Happy coding \m/
