---
layout: post
title: Lapis如何解析JSON 
description: "Lapis如何解析JSON"
modified: 2017-04-06
tags: [REST,JSON]
categories: [Lapis]
---

作者：糖果


app.moon 

```lua
import json_params from require "lapis.application"

class App extends lapis.Application
  @enable "etlua"
  "/onerest": json_params => 
    param_data= {
        s_type: @params.type
    }
    json: {
       success: true
    }
```

app.lua

```lua

    ["/onerest"] = json_params(function(self)
      local param_data = {
        s_type = self.params.type
      }
      return {
        json = {
          success = true
        }
      }
    end)
```


application.moon

```lua
json_params = (fn) ->
  (...) =>
    if content_type = @req.headers["content-type"]
      -- Header often ends with ;UTF-8
      if string.find content_type\lower!, "application/json", nil, true
        ngx.req.read_body!
        local obj
        pcall -> obj, err = json.decode ngx.req.get_body_data!
        @@support.add_params @, obj, "json" if obj

    fn @, ...
```

application.moon

```lua
local json_params
json_params = function(fn)
  return function(self, ...)
    do
      local content_type = self.req.headers["content-type"]
      if content_type then
        if string.find(content_type:lower(), "application/json", nil, true) then
          ngx.req.read_body()
          local obj
          pcall(function()
            local err
            obj, err = json.decode(ngx.req.get_body_data())
          end)
          if obj then
            self.__class.support.add_params(self, obj, "json")
          end
        end
      end
    end
    return fn(self, ...)
  end
end
```





本文请不要用于商业目地，非商业转载请署名原作者与原文链接。
[https://www.moonscript.cn/openresty/resty-http-for-graylog/](https://www.moonscript.cn/openresty/resty-http-for-graylog/)
