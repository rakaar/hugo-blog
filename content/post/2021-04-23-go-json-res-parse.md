---
date: "2021-04-23T00:00:00Z"
tags:
- code
title: Parsing HTTP response in Golang
---

Recently, while writing some code in Golang, I had to make HTTP requests and use the response. The response obtained is a string and it needs to parsed, which was kind of frustrating process(it is really smooth in other languages like JS and Python). Many sources suggest to define a struct and Parse, which is actually a good way. But I wish to write less for sake of simplicity. So in this post, I attempt to parse the different cases of JSON response with less lines of code, though it is not a recommended way!

## Making HTTP request

Suppose you want to making a GET request with headers, you can do it with the following code 

```go
func MakeRequest(URL string) string{
	client := &http.Client{}
	req, _ := http.NewRequest("GET", URL, nil)
	req.Header.Set("Header_Key", "Header_Value")
	res, err := client.Do(req)
	if err != nil {
		fmt.Println("Err is", err)
	}
	defer res.Body.Close()

	resBody, _ := ioutil.ReadAll(res.Body)
	response := string(resBody)

	return response 
}
```

## 1. Suppose the response is a JSON {}

For example, the response is 

```json
{
"name": "raghav",
"age": 19
}
```

Then to parse it

```go
res := MakeRequest(api_url) // Making the request
resBytes := []byte(res) // Converting the string "res" into byte array
var jsonRes map[string]interface{} // declaring a map for key names as string and values as interface 
_ = json.Unmarshal(resBytes, &jsonRes) // Unmarshalling
// I SKIPPED THE ERROR HANDLING FOR BREVITY, BUT YOU SHOULD NOT
```

You can get the individual values from the map like in below code. Note the typecasting done. For example "raghav"  and 19 are of interface{} type. Mostly, they are useful if they are of string and integer type. Typecasting to string is straight forward as you can see below. But for integers, using the same way doesn't work. Below is the right way to do it. (refer [Mujibur's answer](https://stackoverflow.com/questions/18041334/convert-interface-to-int))

```go
name := jsonRes["name"].(string)
var age int = int(jsonRes["age"].(float64))
```

## 2. Suppose the response is an array []

For example, if the response is

```json
[
 {
   "name": "raghav",
   "age": 19
 },
 {
   "name": "kau",
   "age": 20
 }
]
```

To parse it

```go
res := MakeRequest(API_URL) // Making the Request
resBytes := []byte(res) // Converting the string "res" into byte array 
var resArr []map[string]interface{} // declaring an array of maps with key names as string and values as interface{}
_ = json.Unmarshal(resBytes, &resArr) // Unmarshalling
// I SKIPPED THE ERROR HANDLING FOR BREVITY, BUT YOU SHOULD NOT
```

To access values

```go
for i := range resArr {
    name := resArr[i]["name"].(string)
    var age int = int(resArr[i]["age"].(float64))
}
```

## 3. Suppose the response contains nested JSON

Like below, value of "details" is a JSON again

```go
{
"name": "raghav",
"details": {
     "city": "hyd"
   }
}
```

We just do the type casting once again

```go
res := MakeRequest(api_url) // Making the request
resBytes := []byte(res) // Converting the string "res" into byte array
var jsonRes map[string]interface{} // declaring a map for key names as string and values as interface{}
_ = json.Unmarshal(resBytes, &jsonRes) // Unmarshalling

// Type casting again so that interface{} -> map[string]interface{}
details_map := jsonRes["details"].(map[string]interface{}) // type the interface again to a map with key string type and value as interface
city := details_map["city"].(string)
```

## 4. Suppose the response contains a array as value of some key

Like below, the value of "interests" is an array 

```json
{
   "name": "raghav",
    "interests": ["a", "b", "c", 1]
}
```

We need to typecast the array to access its items

```go
res := MakeRequest(api_url) // Making the request
resBytes := []byte(res) // Converting the string "res" into byte array
var jsonRes map[string]interface{} // declaring a map for key names as string and values as interface 
_ = json.Unmarshal(resBytes, &jsonRes) // Unmarshalling

// interface{} -> []interface{}
arr := jsonRes["interests"].([]interface{}) // type the interface again to a array of interfaces
for i := range arr {
    arr_item := arr[i] // you can do the type casting as per ur requirements here
}
```

