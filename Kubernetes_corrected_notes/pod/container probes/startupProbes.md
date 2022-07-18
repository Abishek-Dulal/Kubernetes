# Startup Probes
Startup probes  does a diagnostic check on the container to determining all the containers 
in a pod are ready per container setting. On startup , all  other probes are disabled until the startupProbe succeds. If the startup fails the kubelet will restart the container according to the container restart policy. 