*This is not an official Google product.*

Load Balancing Kubernetes Services with NGINX+
========================================

This app demonstrates how to make use of [NGINX+](https://www.nginx.com/products/) Layer 7 load balancing with [Kubernetes](http://kubernetes.io/) services.

## The app

### Kubernetes Services

The app consists of four Kubernetes services written in Ruby, Python, Node, and Go. The services run in Docker containers, and each one does string manipulation based on a request to an endpoint with a `str` parameter. They each have a replication controller (`rc.yaml`), which configures the number of pods running in the container.

### NGINX+ Load Balancer

The load balancer routes all requests to our backend services through one external IP, and is configured in `nginx.conf`. There is an upstream server for each of the four services. In the first `server` block, requests to the four endpoints are proxied to the correct upstream via `proxy_pass`. The second `server` block configures a status page listening on port 8080 to make use of NGINX+ live monitoring.

### Load Testing with Seige

The app uses [Siege](https://www.joedog.org/siege-home/) to load test the `nginxplus` service. The code for this can be found in the `load-generator` directory.

## Deploying

1. Create a project in the [Google Cloud Developer console](https://console.cloud.google.com).

2. Install [Docker](https://www.docker.com/), then create a Docker instance and host it on Google Compute Engine in your Cloud project.

3. Register for NGINX+ and copy your certificate and license key into the `nginx-repo.crt` and `nginx-repo.key` files.

4. Deploy the four Kubernetes services by running `make deploy` inside each services directory: `arrayify`, `reverse`, `to_lower`, and `to_upper`. To verify that 3 replicas are running for each service, run `kubectl get pods`.

5. Deploy the `nginxplus` service by running `make deploy` from the `nginx` directory. Then run `kubectl get svc` to get the external IP address for your `nginxplus` service.

6. When you navigate to this IP in the browser, you should see the "Nginx is running!" page. Next, verify that each service is running correctly: `YOUR-IP/reverse/?str=teststring`.

7. Try out NGINX+ live monitoring by visiting the status page: `YOUR-IP:8080/status.html`. When you make a request to one of the services, you should see the requests and connections count update in realtime on the status page.

8. Load test your service by running the Seige load generator.

