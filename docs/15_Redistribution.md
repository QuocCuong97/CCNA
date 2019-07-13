# Redistribution
## **1) Redistribute into EIGRP**
```
Router(config) # router eigrp [AS]
```
- **RIP** => **EIGRP**
    ```
    Router(config-router) # redistribute rip metric [bandwidth] [delay] [reliability] [load] [MTU]
    ( Có thể để là 1 1 1 1 1 )
    ```
- **OSPF** => **EIGRP**
    ```
    Router(config-router) # redistribute ospf [process-id] metric [bandwidth] [delay] [reliability] [load] [MTU]
    ```
- **Static Route** & **Default Route** => **EIGRP**
    ```
    Router(config-router) # redistribute static 
    ```
- **Connected Route** => **EIGRP**
    ```
    Router(config-router) # redistribute connected
## **2) Redistribute into OSPF**
```
Router(config) # router ospf [process-id]
```
- **RIP** => **OSPF**
    ```
    Router(config-router) # redistribute rip subnets
    ```
- **EIGRP** => **OSPF**
    ```
    Router(config-router) # redistribute eigrp [AS] subnets
    ```
- **Static Route** => **OSPF**
    ```
    Router(config-router) # redistribute static subnets
    ```
- **Default Route** => **OSPF**
    ```
    Router(config-router) # default-information originate
    ```
- **Connected Route** => **OSPF**
    ```
    Router(config-router) # redistribute connected subnets
    ```
## **3) Redistribute into RIP**
```
Router(config) # router rip
```
- **EIGRP** => **RIP**
    ```
    Router(config-router) # redistribute eigrp [AS] metric 1
    ```
- **OSPF** => **RIP**
    ```
    Router(config-router) # redistribute ospf [process-id] metric 1
    ```
- **Static Route** => **RIP**
    ```
    Router(config-router) # redistribute static metric 1
    ```
- **Default Route** => **RIP**
    ```
    Router(config-router) # default-information originate
    ```
- **Connected Route** => **RIP**
    ```
    Router(config-router) # redistribute connected metric 1
    ```
