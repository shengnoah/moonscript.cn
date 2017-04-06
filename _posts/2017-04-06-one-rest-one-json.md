---
layout: post
title: Lapis如何解析JSON 
description: "Lapis如何解析JSON"
modified: 2017-04-06
tags: [REST,JSON]
categories: [Lapis]
---

作者：糖果


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



本文请不要用于商业目地，非商业转载请署名原作者与原文链接。
[https://www.moonscript.cn/openresty/resty-http-for-graylog/](https://www.moonscript.cn/openresty/resty-http-for-graylog/)
