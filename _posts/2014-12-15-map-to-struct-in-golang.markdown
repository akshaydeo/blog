---
title: map to struct in Golang
date: 2014-12-15 17:26:15 Z
tags:
- golang
- json
- json parsing
- json to struct
layout: post
comments: true
image: https://blog.golang.org/go-brand/Go-BB_cover.jpg
---

Recently I was working with a piece of code where I had to convert a JSON into a Golang struct. I faced hell lot of issues, and waster a bit of time in achieving that, so thought of documenting it.

## Scenario

I have a struct called _User_, which has fields. There were no issues with primary data types, but when it came to time.Time, it started fucking me.

{% highlight go %}
Id int `json:"user_id"`
AuthKey string `json:"-" sql:"not null;unique"`

Name string `json:"user_name" sql:"not null;unique"`
EmailAddress string `json:"email_address" sql:"not null;unique"`
Password string `json:"-" sql:"not null"`

AccountId int `json:"account_id" sql:"not null;unique"`

CreatedAt time.Time `json:"created_at"`
UpdatedAt time.Time `json:"updated_at"`
DeletedAt time.Time `json:"deleted_at"`
{% endhighlight %}

A bit of googling landed me onto [http://github.com/ottemo/mapstructure](http://github.com/ottemo/mapstructure). This had everything I wanted. _(Prefer [https://github.com/mitchellh/mapstructure](https://github.com/mitchellh/mapstructure) repo over as ottemo/mapstructure has an AdvancedDecodeHook that helps a bit in this situation)_

## Solution

Suppose that _body_ has the entire JSON. It has some fields along with _User_ struct with _user_ as key.

- First we will convert the JSON into a map[string]interface{}

```go
  var data map[string]interface{}
  err = json.Unmarshal(body, &data)
  if err != nil {
    return nil, err
  }
```

- Okay so data contains our JSON, now as we have a custom fields _time.Time_ we will have to write our own decoder, which means create on AdvancedDecodeHook func and pass it to the DecoderConfig

```go
func myDecoder(val *reflect.Value, data interface{}) (interface{}, error) {
  if val.Type().String() == "time.Time" {
    value, err := time.Parse(time.RFC3339Nano, data.(string))
    val.Set(reflect.ValueOf(value))
    return nil, err
  }
  return data, nil
}
```

So lets understand the structure of this hook -
You get two parameters:

- val => indicates type of the data
- data => value of that data

This function returns an interface and an error. Now this _AdvancedDecodeHook_ works in a way that if you return nil in place of interface, it the decoder assumes that our custom decoder has parsed the value for the given data, and it leaves it in that way. In case of error the entire parsing fails. So you have to decide on whether to throw the error or set a default value when parsing fails.

- And now just write a function to return the decoder with our custom configuration

```go
func getDecoder(result interface{}) (*mapstructure.Decoder, error) {
  return mapstructure.NewDecoder(&mapstructure.DecoderConfig{
  AdvancedDecodeHook: myDecoder,
  TagName: "json",
  Result: result,
  WeaklyTypedInput: false})
}
```

The parameter that we pass in is the result type we expect. This has to be the pointer to the struct.

- And finally call it from your routine

```go
decoder, err := getDecoder(&user)
if err != nil {
  return nil, err
}
err = decoder.Decode(data["user"])
if err != nil {
  return nil, err
}
```

- And you are done.

## Final code

So the final code looks like this

```go

func myDecoder(val *reflect.Value, data interface{}) (interface{}, error) {
  if val.Type().String() == "time.Time" {
    value, err := time.Parse(time.RFC3339Nano, data.(string))
    val.Set(reflect.ValueOf(value))
    return nil, err
  }
  return data, nil
}

func getDecoder(result interface{}) (*mapstructure.Decoder, error) {
  return mapstructure.NewDecoder(&mapstructure.DecoderConfig{
  AdvancedDecodeHook: myDecoder,
  TagName: "json",
  Result: result,
  WeaklyTypedInput: false})
}

func GetUserFromJSON(jsonUser string) (*User,error){
  var data map[string]interface{}
  err = json.Unmarshal(jsonUser, &data)
  if err != nil {
    return nil, err
  }
  decoder, err := getDecoder(&user)
    if err != nil {
    return nil, err
  }
  err = decoder.Decode(data["user"])
  if err != nil {
    return nil, err
  }
  return &user,nil
}
```

Thank you all, keep coding :)
