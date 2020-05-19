
# Multi-Level DHT Design and Evaluation

This is a local copy of a document from [Protocol Labs](https://protocol.ai/).

## Background

Routing in content-oriented or content-centric networks means that any content published in this network needs to have a routable address. While traditionally the number of routable addresses (i.e., IP addresses) needed was bounded by the number of physical machines/end-nodes, switching to addressable content means that the number of routable addresses expands to match the number of unique content items (i.e. any file) that is published on the network. Content-addressable networks face the challenge of routing scalability, as the amount of addressable elements in the network rises by several orders of magnitude compared to the host-addressable Internet of today.

In the case of IPFS and libp2p, content routing is done primarily by means of a Distributed Hash Table (DHT). Although DHTs are known to scale in the number of nodes, the decentralised and totally unmanaged structure of the IPFS and libp2p systems presents challenges when it comes to dialability of nodes and look-up latency in the underlying network.

## Problem Statement

The solutions proposed for this RFP will be addressing our published open problem on [Routing at Scale](./Routing-at-Scale.md).

**Scope of this RFP**

In this RFP, we are looking for approaches that target Distributed Hash Table-based architectures and propose enhanced designs that use multiple DHT layers or dimensions. Each layer of the DHT is either topologically embedded into the underlying network topology (e.g., through geolocation or hop count), is based on a specific sharding strategy, or is based on some specific topic to exploit the power of social interactions. Topological embedding is important here in order to take advantage of locality of interest and reduced number of network hops to resolve content. Approaches should be resilient to high churn, provide low look-up time, and scale to tens of millions of users.

## Objectives & Key Results

* Objective 1: Design of multi-level DHT
    1. Review related literature, create a short survey and identify solutions from which it is worth borrowing concepts.
    2. Design overall system architecture adapted for the case of libp2p (and IPFS as a use case.
* Objective 2: Performance and scalability evaluation
    1. Demonstrate the performance of your design through formal methods, calculations (e.g. Python Notebook, ObservableHQ, etc.), or a simulation environment (e.g. PeerSim, ns-3) with regard to the following metrics:
        1. Look-up time (from when a node first seeks for a piece of content to when it finds a provider).
        2. Publication propagation time (time needed for published content to be discoverable by other nodes).
        3. Publication propagation overhead (signalling traffic generated when publishing a new content item).
        4. Load balancing (load asymmetry between DHT nodes at different layers).
        5. Performance improvement for non-uniform content popularity (i.e., for realistic content distributions where the exponent of the zipf distribution is in the area of 0.7-1.5).
    2. Compare with alternatives and state of the art.
    3. The performance evaluation should take into account main network stats:
        1. Significant network size variation, with weekly cycles between 150K and 250K nodes.
        2. Large fraction (75%+) of nodes that undiable due to NAT/Firewalls.
        3. A 100GB file transforms into a graph of roughly 1 million blocks when added to IPFS. For random-access to be possible, that means that roughly 2 million provides (DHT puts) need to happen every time interval (default: 24 hours).
>Darren:  I fell that this should be a major area of focus. If random access of large files can be made efficient then won't that port to many files?  

* Objective 3: Validation in a testbed environment
    1. Develop a prototype Go implementation that integrates into the go-libp2p Implementation.
