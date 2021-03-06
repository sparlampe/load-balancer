
#Step 1 – Generate provider

Execute `sbt provider/run` then execute `curl -X GET localhost:5000/get`. Alternatively 
- build the provider image `sbt provider/docker:publishLocal`
- run a container `docker-compose up -d`
- invoke the provider on method `get` using `curl -X GET localhost:5000/get`
- clean up `docker-compose down`

#Step 2 – Register a list of providers

To demonstrate the functionality
- build the provider image `sbt provider/docker:publishLocal` and the balancer image
`sbt balancer/docker:publishLocal`
- start the cluster with, say, 4 providers by executing `docker-compose up -d --scale provider=4`
- retrieve currently registered providers `curl -X GET localhost:8080/provider`


#Step 3 – Random invocation 
to demonstrate the functionality
- build the provider image `sbt provider/docker:publishLocal` and the balancer image
`sbt balancer/docker:publishLocal`
- start the cluster with, say, 4 providers by executing `docker-compose up -d --scale provider=4`
- execute `curl -X GET localhost:8080/get` several time to see responses from different providers.

#Step 4 – Round Robin invocation
to demonstrate the functionality
- build the provider image `sbt provider/docker:publishLocal` and the balancer image
`sbt balancer/docker:publishLocal`
- start the cluster with, say, 4 providers by executing `docker-compose up -d --scale provider=4`
- execute `curl -X GET localhost:8080/get` several time to see **periodic** responses from different providers.

#Step 5 – Manual node exclusion / inclusion
- start the cluster, retrieve all the providers calling `curl -X GET localhost:8080/provider`
- deactivate one provider by calling `curl -X PATCH localhost:8080/provider/PROVIDER_ID/activate `
- execute `curl -X GET localhost:8080/get` several times to see the excluded provider does not receive request.

#Step 6 – Heart beat checker 
- start the cluster, retrieve all the providers calling `curl -X GET localhost:8080/provider` 
- notice that the all providers are active
- deactivate one provider by stopping its container`
- retrieve all the providers calling `curl -X GET localhost:8080/provider` notice the stopped provider is inactive