Munin plugin to monitor requests number, cache statuses, http status codes and average request times of
specified nginx upstreams.

# Configuration parameters:
*env.graphs* - which graphs to produce (optional, list of graphs separated by spaces, default - cache http time request)
*env.log* - log file path (mandatory, ex.: /var/log/nginx/upstream.log)
*env.upstream* - list of upstreams to monitor (mandatory, including port numbers separated by space, ex.: 10.0.0.1:80 10.0.0.2:8080)
*env.statuses* - list of http status codes to monitor (optional, default - all statuses, ex.: 200 403 404 410 500 502)
*env.percentiles* - which percentiles to draw on time graphs (optional, list of percentiles separated by spaces, default - 80)

## Installation
Copy file to directory /usr/share/munin/pligins/ and create symbolic link(s) for each log file you wish to monitor.

Specify log_format at /etc/nginx/conf.d/upstream.conf:
```
log_format upstream "ua=[$upstream_addr] ut=[$upstream_response_time] us=[$upstream_status] cs=[$upstream_cache_status]"
```

Use it in your site configuration (/etc/nginx/sites-enabled/anything.conf):
```
access_log /var/log/nginx/upstream.log upstream;
```

And specify some options in munin-node.conf:
```
[nginx_upstream_multi_upstream]
env.graphs cache http time request
env.log /var/log/nginx/upstream.log
env.upstream 10.0.0.1:80 10.0.0.2:8080 unix:/tmp/upstream3
env.statuses 200 403 404 410 500 502
env.percentiles 50 80
```
