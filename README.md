Persistent Grants in Linux/FreeBSD
==================================

Persistent Grants are a mechanism for amortazing grant copy and grant map/unmap
costs by permanently mapping the grants and using memcpy instead.
This repository contains the patches for Linux netback/netfront 
[[1](http://lists.xenproject.org/archives/html/xen-devel/2015-05/msg01498.html)]
and FreeBSD netback including netmap mode support for the latter. In FreeBSD that is able to send up to 10 Mpps in netmap, and having 2x better performance using the host stack up specifically 1.1 Mpps in RX and 1.7 Mpps in TX.

Linux Results
=============

Packet I/O Tests
----------------

Measured on a Intel Xeon E5-1650 v2, Xen 4.5, no HT. Used pktgen "burst 1"
and "clone_skbs 100000" (to avoid alloc skb overheads) with various pkt
sizes. All tests are DomU <-> Dom0, unless specified otherwise.

![RX](http://cnp.neclab.eu/images/pgntperf/udprx_hostA.png)
![TX](http://cnp.neclab.eu/images/pgntperf/udptx_hostA.png)

Bulk Transfer Tests A
---------------------

Measured on a Intel Xeon E5-1650 v2 @ 3.5 Ghz, Xen 4.5, no HT. Used
iperf (TCP)  with an increased number of flows following similar
methodology explained in [2]. All vif irqs are balanced across cores
in both Dom0 and DomU. Tests are between DomU <-> Dom0, unless
specified otherwise.

![RX](http://cnp.neclab.eu/images/pgntperf/tcprx_hostA.png)
![TX](http://cnp.neclab.eu/images/pgntperf/tcptx_hostA.png)
![VM-to-VM](http://cnp.neclab.eu/images/pgntperf/tcpintra_hostA.png)

Bulk Transfer Tests B
---------------------

Same as before, but measured on a Intel Xeon E5-2697 v2 @ 2.7 Ghz,
to test guests with a higher number of queues (>3).

![RX](http://cnp.neclab.eu/images/pgntperf/tcprx_hostB.png)
![TX](http://cnp.neclab.eu/images/pgntperf/tcptx_hostB.png)
![VM-to-VM](http://cnp.neclab.eu/images/pgntperf/tcpintra_hostB.png)

References
==========

[1] http://lists.xenproject.org/archives/html/xen-devel/2015-05/msg01498.html
