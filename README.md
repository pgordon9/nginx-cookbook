# Nginx Cookbook
[![License](https://img.shields.io/badge/license-Apache_2-blue.svg)](https://www.apache.org/licenses/LICENSE-2.0)

[Application cookbook][0] which installs and configures the [nginx][1] monitoring daemon. Currently it defaults to ubuntu. 

## Usage
### Supports
- Ubuntu 

Currently this does not support sentinal but it will in due time.

### Dependencies
| Name | Description |
|------|-------------|
| [poise][2] | [Library cookbook][4] built to aide in writing reusable cookbooks. |
| [poise-service][3] | [Library cookbook][4] built to abstract service management. |

### Attributes
All nginx default shipped settings are built directly into the resource and most if not all have default settings attached(same that come packaged with nginx). You can view all default settings/attributes here [Nginx Service][5] [Nginx Site][6]. 

### Resources/Providers

#### nginx_service
This provider will setup install and setup nginx. It will not configure any sites just the service and default nginx.conf.

```ruby
nginx_service "example"
```

If you would like additional settings outside of the basic attributes listed here [Nginx Service][5] you would add them with the same syntax below:

```ruby
nginx_service "www" do
  additional_options do
    option1 'value1'
    option2 'value2'
  end
end
```

You also have the ability your own nginx.conf entirely by specifying the source option:

```ruby
nginx_service "www" do
  source "somefile.erb"
end
```

####nginx_site
This provider will create and enable(symbolic link to sites-enabled) by default for all nginx sites specified in the block. Ideally you will always pass a servername otherwise the default is example.com:
```ruby
nginx_site "wwww" do
  servername "www.company.com"
  notifies :restart, "nginx_service[#{instance}]", :immediately # This will work only when the site instance name and service instance name are alike. 
end
```

If you would like additional settings outside of the basic attributes listed here [Nginx Site][6] you would add them with the same syntax below:
```ruby
nginx_site "www" do
  servername "www.company.com"
  additional_options do
    option1 'value1'
    option2 'value2'
  end
end
```

You can also enable SSL and/or force redirect for anything listening on 80:
```ruby
nginx_site "www" do
  servername "www.company.com"
  enable_ssl true
  force_ssl_redirect true
end
```
 If you would like to add additional SSL options only that won't be apart of the main http service you can use ```additional_ssl_options``` block.

Last but not least you can also bring your own config file and do whatever you like with that.
```ruby
nginx_site "www" do
  source "www.company.com.erb"
end
```

License & Authors
-----------------
- Author:: Anthony Caiafa (<acaiafa1@bloomberg.net>)

```text
Copyright 2015 Bloomberg Finance L.P.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```

[0]: http://blog.vialstudios.com/the-environment-cookbook-pattern#theapplicationcookbook
[1]: http://nginx.org/
[2]: https://github.com/poise/poise
[3]: https://github.com/poise/poise-service
[4]: http://blog.vialstudios.com/the-environment-cookbook-pattern#thelibrarycookbook
[5]: libraries/nginx_service.rb
[6]: libraries/nginx_site.rb
