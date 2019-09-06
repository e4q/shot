# Shot
Shot is a small HTTP client library for Erlang

## Goals
Shot aims to provide a simple way for REST calls

## Documentation
For examples will be used **shot** and test services for requests:
* [http://ptsv2.com](http://ptsv2.com) - for testing upload files
* [http://httpbin.org](http://httpbin.org) - for testing GET, POST, PUT etc.

### Build & Run
```sh
$ git clone https://github.com/vkatsuba/shot.git
$ cd shot
$ make
```
### Install shot to project: [Rebar3](https://www.rebar3.org/)
* Edit file **rebar.config**:
```
...
{deps, [
  ...
  {shot, {git, "git://github.com/vkatsuba/shot.git", {branch, "master"}}},
  ...
]}.
...
```
* Edit file ***.app.src**:
```
...
  {applications,
   [
    ...,
    shot,
    ...
   ]},
...
```
### PUT
```erlang
shot:put("http://httpbin.org/put").
```
### GET
```erlang
shot:get("http://httpbin.org/get").
```
```erlang
Data = #{
  u => "https://httpbin.org/bearer",                % URL string, eg: "http://test.com"
  h => #{"Authorization" => "Bearer dXNlcjpwYXNz"}  % Headers
}.
shot:get(Data).
```
### POST
```erlang
shot:post("http://httpbin.org/post").
```
```erlang
Data = #{
  u => "https://httpbin.org/anything",                  % URL string, eg: "http://test.com"
  b => "{\"foo\":[\"bing\",2.3,true]}",                 % Body data
  ct => "application/json",                             % Content-Type, eg: "application/json"
  h => #{                                               % Headers
    "Authorization" => "Basic dmthdHN1YmE6JDFxMnczZTQk"
  }
}.
shot:post(Data).
```
### DELETE
```erlang
shot:delete("http://httpbin.org/delete").
```
### multipart/form-data
* Create file file **test.dat** with any data
* Go to the [http://ptsv2.com](http://ptsv2.com)
* Click to the button **New Random Toilet**
* Find on page field **Post URL** and copy **Post URL**
* Prepare and create request:
```erlang
ReqMap = #{
  m => post,                        % Method, can be POST, PUT atom only, eg: post, put
  u => "http://ptsv2.com/ID/post",  % URL string, eg: "http://test.com"
  p => "/path/to/test.dat",         % Full path to file, eg: "/path/to/file.dat"
  o => [],                          % Options, eg: [{ssl, [[{ciphers, [{rsa, aes_128_cbc, sha}]}]]}]
  cd => "test-data",                % Content-Disposition, eg: "dat-model"
  ct => ""                          % Content-Type, eg: "application/json"
}.

shot:multipart(ReqMap).
```
* response
```sh
{ok,{{"HTTP/1.1",200,"OK"},
     [{"date","Tue, 05 Mar 2019 20:38:51 GMT"},
      {"server","Google Frontend"},
      {"content-length","54"},
      {"content-type","text/html; charset=utf-8"},
      {"x-cloud-trace-context",
       "801e26cb1bec64bbf24fd1e9259893e9"}],
     "Thank you for this dump. I hope you have a lovely day!"}}
```
* Back to **Toilet** page, refresh page and see block **Dumps**

### To be continued ...

## Support
v.katsuba.dev@gmail.com
