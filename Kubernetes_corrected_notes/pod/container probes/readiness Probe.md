# Readiness Probes
Readiness Probes does a  routine diagnostic check on a container . It is per container setting probe meaning all the container can have independent defination for them. When the  readiness check fails kubernetes  removes the pod from load balancing. It is so that to prevent the users from seeing the errors when the application can't temporarily respond to  a request.