# Implemention of D2Fl

<pre>

  _____ ___  ______ _      
 |  __ \__ \|  ____| |     
 | |  | | ) | |__  | |     
 | |  | |/ /|  __| | |     
 | |__| / /_| |    | |____ 
 |_____/____|_|    |______|

 </pre>
                           
## Overview
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

  
