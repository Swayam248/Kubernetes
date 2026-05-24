# **Core Concepts**

**Endpoints:**
-------------------------------------

List of IP addresses and ports that a service forwards traffic to. 

----------------------------------
**Finalizer:**
------------------------------

Ensures that specific cleanup steps are completed before an object is deleted. 

----------------------------------

**Admission Controller:**
------------------------------------

Intercepts requests to the K8S API for validation and mutation. 

------------------------------------

**StatefulSet:**
----------------------------------------

Manages stateful applications, keeping track of each pod's identity. 

-----------------------------------

**PodDisruption Budget:**
-----------------------------------

Limits the number of pods that can be unavailable during maintenance. 

----------------------------------------

**Priority Class:**
-------------------------------

Specifies the priority of pods to influence their scheduling. 

------------------------------------------

**Container:**
----------------------------------------------

Smallest unit of sw packaging that runs inside pod.

------------------------------------------

**Init Container:**
--------------------------------------------

Special containers that run before the main containers in the pod start.

---------------------------------------

**Sidecar Containers:**
----------------------------------------

Helper Containers that run alongside the main container in a pod. 

-----------------------------------------

**Liveness Probe:**
-----------------------------------------------

Checks if a container is still running and should be restarted or not. 

----------------------------------------------------

**Readiness Probe:**
--------------------------------------------

Checks if a container is ready to start and accept traffic. 

------------------------------------------------

**Self Healing:**
------------------------------------------------------------------------

Automatically replaces and reschedules failed containers. 

--------------------------------------------------

**Pod Preset:**
------------------------------------------------------------------------

Injects Certain information like secrets or volume mounts, into pods at creation. 

-------------------------------------------------------------------------

**NetworkPolicy:**
------------------------------------------------------------------------

Controls the traffic between pods.

------------------------------------------------------------------------

**Core DNS:**
--------------------------------------------------------------

A DNS server for the cluster, providing name resolution for services. 

------------------------------------------------------------------

**Headless Service**
------------------------------------------------------

A service without a cluster IP, used to directly access pods. 

------------------------------------------------------------------------
