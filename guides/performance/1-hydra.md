# ORY Hydra Performance Benchmarks

In this document you will find benchmark results for different endpoints of ORY Hydra. All benchmarks are executed
using [rakyll/hey](https://github.com/rakyll/hey). Please note that these benchmarks run against the in-memory storage
adapter of ORY Hydra. These benchmarks represent what performance you would get with a zero-overhead database implementation.

We do not include benchmarks against databases (e.g. MySQL or PostgreSQL) as the performance greatly differs between
deployments (e.g. request latency, database configuration) and tweaking individual things may greatly improve performance.
We believe, for that reason, that benchmark results for these database adapters are difficult to generalize and potentially
deceiving. They are thus not included.

This file is updated on every push to master. It thus represents the benchmark data for the latest version.

All benchmarks run 10.000 requests in total, with 100 concurrent requests. All benchmarks run on Circle-CI with a
["2 CPU cores and 4GB RAM"](https://support.circleci.com/hc/en-us/articles/360000489307-Why-do-my-tests-take-longer-to-run-on-CircleCI-than-locally-)
configuration.

## BCrypt

ORY Hydra uses BCrypt to obfuscate secrets of OAuth 2.0 Clients. When using flows such as the OAuth 2.0 Client Credentials
Grant, ORY Hydra validates the client credentials using BCrypt which causes (by design) CPU load. CPU load and performance
depend on the BCrypt cost which can be set using the environment variable `BCRYPT_COST`. For these benchmarks,
we have set `BCRYPT_COST=8`.

## OAuth 2.0

This section contains various benchmarks against OAuth 2.0 endpoints

### Token Introspection

```

Summary:
  Total:	0.4373 secs
  Slowest:	0.0252 secs
  Fastest:	0.0001 secs
  Average:	0.0041 secs
  Requests/sec:	22867.1305
  
  Total data:	1550000 bytes
  Size/request:	155 bytes

Response time histogram:
  0.000 [1]	|
  0.003 [4124]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.005 [1854]	|■■■■■■■■■■■■■■■■■■
  0.008 [2193]	|■■■■■■■■■■■■■■■■■■■■■
  0.010 [1323]	|■■■■■■■■■■■■■
  0.013 [333]	|■■■
  0.015 [89]	|■
  0.018 [30]	|
  0.020 [22]	|
  0.023 [23]	|
  0.025 [8]	|


Latency distribution:
  10% in 0.0001 secs
  25% in 0.0002 secs
  50% in 0.0039 secs
  75% in 0.0070 secs
  90% in 0.0088 secs
  95% in 0.0101 secs
  99% in 0.0144 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0000 secs, 0.0001 secs, 0.0252 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0069 secs
  req write:	0.0000 secs, 0.0000 secs, 0.0062 secs
  resp wait:	0.0039 secs, 0.0001 secs, 0.0193 secs
  resp read:	0.0001 secs, 0.0000 secs, 0.0071 secs

Status code distribution:
  [200]	10000 responses



```

### Client Credentials Grant

This endpoint uses [BCrypt](#bcrypt).

```

Summary:
  Total:	18.5502 secs
  Slowest:	0.7051 secs
  Fastest:	0.0165 secs
  Average:	0.1777 secs
  Requests/sec:	539.0781
  
  Total data:	1570000 bytes
  Size/request:	157 bytes

Response time histogram:
  0.017 [1]	|
  0.085 [1457]	|■■■■■■■■■■■■■■■■■■
  0.154 [2993]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.223 [3288]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.292 [792]	|■■■■■■■■■■
  0.361 [752]	|■■■■■■■■■
  0.430 [523]	|■■■■■■
  0.499 [101]	|■
  0.567 [40]	|
  0.636 [46]	|■
  0.705 [7]	|


Latency distribution:
  10% in 0.0784 secs
  25% in 0.1006 secs
  50% in 0.1808 secs
  75% in 0.2150 secs
  90% in 0.3079 secs
  95% in 0.3866 secs
  99% in 0.4979 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0001 secs, 0.0165 secs, 0.7051 secs
  DNS-lookup:	0.0001 secs, 0.0000 secs, 0.0137 secs
  req write:	0.0002 secs, 0.0000 secs, 0.1643 secs
  resp wait:	0.1769 secs, 0.0165 secs, 0.7051 secs
  resp read:	0.0003 secs, 0.0000 secs, 0.0844 secs

Status code distribution:
  [200]	10000 responses



```

## OAuth 2.0 Client Management

### Creating OAuth 2.0 Clients

This endpoint uses [BCrypt](#bcrypt) and generates IDs and secrets by reading from  which negatively impacts
performance. Performance will be better if IDs and secrets are set in the request as opposed to generated by ORY Hydra.

```
This test is currently disabled due to issues with /dev/urandom being inaccessible in the CI.
```

### Listing OAuth 2.0 Clients

```

Summary:
  Total:	0.3351 secs
  Slowest:	0.0297 secs
  Fastest:	0.0001 secs
  Average:	0.0031 secs
  Requests/sec:	29845.1691
  
  Total data:	4210000 bytes
  Size/request:	421 bytes

Response time histogram:
  0.000 [1]	|
  0.003 [5923]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.006 [1949]	|■■■■■■■■■■■■■
  0.009 [1227]	|■■■■■■■■
  0.012 [593]	|■■■■
  0.015 [186]	|■
  0.018 [78]	|■
  0.021 [21]	|
  0.024 [13]	|
  0.027 [5]	|
  0.030 [4]	|


Latency distribution:
  10% in 0.0001 secs
  25% in 0.0002 secs
  50% in 0.0011 secs
  75% in 0.0053 secs
  90% in 0.0087 secs
  95% in 0.0105 secs
  99% in 0.0154 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0000 secs, 0.0001 secs, 0.0297 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0055 secs
  req write:	0.0000 secs, 0.0000 secs, 0.0094 secs
  resp wait:	0.0029 secs, 0.0000 secs, 0.0225 secs
  resp read:	0.0001 secs, 0.0000 secs, 0.0085 secs

Status code distribution:
  [200]	10000 responses



```

### Fetching a specific OAuth 2.0 Client

```

Summary:
  Total:	0.3203 secs
  Slowest:	0.0252 secs
  Fastest:	0.0001 secs
  Average:	0.0030 secs
  Requests/sec:	31216.3565
  
  Total data:	4190000 bytes
  Size/request:	419 bytes

Response time histogram:
  0.000 [1]	|
  0.003 [5917]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.005 [1675]	|■■■■■■■■■■■
  0.008 [1109]	|■■■■■■■
  0.010 [724]	|■■■■■
  0.013 [368]	|■■
  0.015 [145]	|■
  0.018 [36]	|
  0.020 [14]	|
  0.023 [8]	|
  0.025 [3]	|


Latency distribution:
  10% in 0.0001 secs
  25% in 0.0001 secs
  50% in 0.0008 secs
  75% in 0.0049 secs
  90% in 0.0085 secs
  95% in 0.0104 secs
  99% in 0.0143 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0000 secs, 0.0001 secs, 0.0252 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0070 secs
  req write:	0.0000 secs, 0.0000 secs, 0.0058 secs
  resp wait:	0.0028 secs, 0.0000 secs, 0.0252 secs
  resp read:	0.0001 secs, 0.0000 secs, 0.0047 secs

Status code distribution:
  [200]	10000 responses



```
