This Project is a sandbox of setting up envoy proxy with jaeger distributed tracing tool, with setting up some sandbox microservices to implement distributed microservice tracing.
## Quickstart
1. Install Docker to use for bringing up the sandbox services.
2. The services are defined in `docker-compose.yml`. There are four services. The Envoy frontend service is responsible for proxying the requests and make connection between microservices. There are two microservices defined as service 1 and service 2. You can see their definitions in `shared/python/tracing/service.py` and `shared/python/tracing/service2.py` and you can change them to see how they work in order of setting trace headers and functioning as a microservice.
3. Set `.env` file in project root. There are two parameters in the docker compose file to set. First is PORT_PROXY, which is the Envoy frontend proxy port, then is PORT_UI, which is the port of jaeger tracer UI.
4. Bring up services using docker compose: `docker-compose --env-file .env up`

## Envoy configurations
There is a separate envoy configuration file for each service. In each config file, under filters there is a tracing config added to the Envoy configuration:
          
          tracing:
            provider:
              name: envoy.tracers.zipkin
              typed_config:
                "@type": type.googleapis.com/envoy.config.trace.v3.ZipkinConfig
                collector_cluster: jaeger
                collector_endpoint: "/api/v2/spans"
                shared_span_context: false
                collector_endpoint_version: HTTP_JSON

Also a jaeger cluster is added in order to make it available to tracing config for sending traces:

    name: jaeger
    type: STRICT_DNS
    lb_policy: ROUND_ROBIN
    load_assignment:
      cluster_name: jaeger
      endpoints:
      - lb_endpoints:
        - endpoint:
            address:
              socket_address:
                address: jaeger
                port_value: 9411

The other configs are routine routing configurations for Envoy to proxy requests to their designated services.
