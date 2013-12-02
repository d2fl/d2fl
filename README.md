# Implemention of D2Fl

<pre>

  _____ ___  ______ _      
 |  __ \__ \|  ____| |     
 | |  | | ) | |__  | |     
 | |  | |/ /|  __| | |     
 | |__| / /_| |    | |____ 
 |_____/____|_|    |______|

 </pre>
## Background

The existence of misbehavior routers has been a major concern in large-scale networks. The presence of such nodes in a network can lead to Byzantine failures including packet corruption, forging or delaying, which in turn can cause major performance issues. Data-plane fault localization (FL) is a hopeful way to solve this problem. However in practice there is no efficient coun- termeasure. In fact, the path-based FL fails to support dynamic routing, and the neighbor-based FL requires a centralized trusted administrative controller (AC), global clock synchronization in each router and storage over- head for caching packets.

Therefore, in this work, we address all these prob- lems by introducing a dynamic distributed and low-cost model, D2FL, using random 2-hop neighborhood au- thentication. D2FL supports volatile path without requir- ing the AC or any global clock synchronization. Besides, D2FL requires only constant tens of KB for caching. That is also independent of the packet transmission rate. This is to be compared with the cache size of DynaFL which consumes several MBs and depend on the packet transmission rate. We evaluate its effect and the results show that D2 F L achieves low false positive and false negative rate with no more than 3% bandwidth overhead. We also propose a dynamic adaptive sampling algorithm model, to further reduce the overhead in authenticating packets.

## Implementation Overview
At the endpoint, we use tap device to catch the packets, add tag and redirect them to bridge.

At the route node, we use click router modular to change the server to the router.
## SetUp

### On the Router

you just need to copy the files in router dir to elements/local and follow the instructions on the CLICK website.

### On the Endpoint, 

- Create a bridge(via /etc/network/interfaces)

- Create two tap devices( tap0 and tap1 ) and add tap1 to bridge

<code>
    tunctl -t tap0
    
    tunctl -t tap1
    
    ifconfig tap0 up
    
    ifconfig tap1 up
    
    brctl addif br0 tap1
</code>

- Change the gateway to set tap0 as the default dev

<code>
    if route del default via $GW dev br0
    
    ip route del $NET dev br0
    
    ip route add $NET dev tap0
    
    ip route add default via $GW dev tp0
</code>

- Compile the tagger and run

## Evaluation

We setup the evironment on 3 Dell T610 Servers: one router and two end nodes. We test the system performance with benchmark like Netperf and real applications like apache web server. The influence of D2FL on the network is less than 10% compared with the baseline(environment without D2FL). You can get the detailed evaluation results in our paper.
  
