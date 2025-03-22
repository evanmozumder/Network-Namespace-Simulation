# Network-Namespace-Simulation

Here's the complete README file code for you to copy and paste directly into your GitHub README.md file:

# Linux Network Namespace Simulation

A bash script for creating and managing multiple network namespaces connected via bridges and a router namespace.

## Overview

This project simulates two separate networks connected through a router using Linux network namespaces and bridges. It creates an isolated networking environment on a single Linux host.

## Network Topology

```mermaid
graph TD
    subgraph "Host System"
        BR0[Bridge br0<br>10.0.1.1/24]
        BR1[Bridge br1<br>10.0.2.1/24]
    end
    
    subgraph "Namespace 1 (ns1)"
        NS1[Network Namespace ns1]
        VETH_NS1[veth-ns1<br>10.0.1.10/24]
    end
    
    subgraph "Namespace 2 (ns2)"
        NS2[Network Namespace ns2]
        VETH_NS2[veth-ns2<br>10.0.2.10/24]
    end
    
    subgraph "Router Namespace (router-ns)"
        ROUTER[Router Namespace]
        VETH_ROUTER_BR0[veth-router-br0<br>10.0.1.254/24]
        VETH_ROUTER_BR1[veth-router-br1<br>10.0.2.254/24]
    end
    
    VETH_NS1 --- VETH_BR0_NS1[veth-br0-ns1]
    VETH_BR0_NS1 --- BR0
    
    VETH_NS2 --- VETH_BR1_NS2[veth-br1-ns2]
    VETH_BR1_NS2 --- BR1
    
    VETH_ROUTER_BR0 --- VETH_BR0_ROUTER[veth-br0-router]
    VETH_BR0_ROUTER --- BR0
    
    VETH_ROUTER_BR1 --- VETH_BR1_ROUTER[veth-br1-router]
    VETH_BR1_ROUTER --- BR1
    
    VETH_ROUTER_BR0 --- ROUTER
    VETH_ROUTER_BR1 --- ROUTER
    
    classDef namespace fill:#f9f,stroke:#333,stroke-width:2px,color:black,font-weight:bold
    classDef bridge fill:#bbf,stroke:#333,stroke-width:2px,color:black,font-weight:bold
    classDef interface fill:#dfd,stroke:#333,stroke-width:1px,color:black,font-weight:bold
    class NS1,NS2,ROUTER namespace
    class BR0,BR1 bridge
    class VETH_NS1,VETH_NS2,VETH_ROUTER_BR0,VETH_ROUTER_BR1,VETH_BR0_NS1,VETH_BR1_NS2,VETH_BR0_ROUTER,VETH_BR1_ROUTER interface

```

## Features

- Creates two isolated network namespaces (ns1, ns2)
- Sets up a router namespace to connect both networks
- Configures bridges (br0, br1) for network connectivity
- Implements proper IP addressing and routing
- Provides commands for testing connectivity
- Includes cleanup functionality

## Requirements

- Linux system with network namespace support
- Root privileges
- Basic bash environment

## IP Addressing

- **Network 1 (10.0.1.0/24)**
  - Bridge (br0): 10.0.1.1/24
  - Namespace 1 (ns1): 10.0.1.10/24
  - Router interface: 10.0.1.254/24

- **Network 2 (10.0.2.0/24)**
  - Bridge (br1): 10.0.2.1/24
  - Namespace 2 (ns2): 10.0.2.10/24
  - Router interface: 10.0.2.254/24

## Technical Details

The script creates:
- 3 network namespaces (ns1, ns2, router-ns)
- 2 network bridges (br0, br1)
- 4 virtual ethernet pairs to connect everything
- Proper routing configuration for cross-network communication
