This Project is a sandbox of setting up envoy proxy with jaeger distributed tracing tool, with setting up some sandbox microservices to implement distributed microservice tracing.
## Quickstart
1. Install Docker to use for bringing up the sandbox services.
2. The services are defined in `docker-compose.yml`. There are four services. The Envoy frontend service is responsible for proxying the requests and make connection between microservices. There are two microservices defined as service 1 and service 2. You can see their definitions in `shared/python/tracing/service.py` and `shared/python/tracing/service2.py` and you can change them to see how they work in order of setting trace headers and functioning as a microservice.
3. Set `.env` file in project root. There are two parameters in the docker compose file to set. First is PORT_PROXY, which is the Envoy frontend proxy port, then is PORT_UI, which is the port of jaeger tracer UI.
4. Bring up services using docker compose: `docker-compose --env-file .env up`
