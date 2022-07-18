# What is a Namespace
Namespace are  an ability within kubernetes to subdivide the cluste and its resource to create a virtual cluster.

# Why is Namespace used
1) to isolate the kubernetes resources  from one another and to organise them.
2) to have prod environment / dev environment etc.
3) to limit resources such as ram /storage etc.
4) to have a security boundary with role based access control

## A resource can only be created in one namespace.

# What are the things that are namespaced
Services , pods and all the other resources are namespaced

# What are the things that are not namespaced
PersistantVolumes , Nodes and all the physical things are not namespaced.

# What are the namespaces available by default
The namespaces that are available on creation are:-
- default
- kube-system
- kube-public

we can also create **User defined** namespace.

