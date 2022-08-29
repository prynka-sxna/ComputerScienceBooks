**Big Data: Principles and best practices of scalable realtime data systems**

**- Nathan Marz**

**Properties of a Big Data System**

Robust, Fault Tolerant, Low Latency Read/Write, Scalability, Generalization, Extensibility, Ad Hoc Query Support, Minimal Maintenance, Debuggability

**Lambda Architecture**

- It implements arbitrary functions on arbitrary dataset to return the result with low latency. 
- It aims to maintain a master dataset (source of truth) and compute views on this to aid query execution. It suggests using a mix of incremental and recomputation algorithms. 

**Master Data**

Properties of Big Data: 
- Raw data is more rich in information compared to normalized data. 
- It should be treated as immutable and perpetually true i.e. each record is true for it's timestamp. 

Modelling Data:
- Data records can be represented as facts | each fact is atomic and timestamped.
- Relationship between facts can be modelled like a graph.

**Master Data Storage**

Distributed file systems allows writes (appends) to be efficient, parallel reads, and tuning processing costs and storage costs.

**Batch Layer**

- This layer is responsible for precomputing batch views (by recomputation from scratch and/or incrementally) | queries can be answered with low latency.
- The key to each batch view is to balance between the size of the precomputed views and the amount of on-the-fly computation required at query time.

**Serving Layer**

- This layer consists of databases that index and serve the results of the batch layer.
- Databases should be batch writable, scalable, support random reads, and be fault tolerant. 
- It need not support random writes.
- Normalized data can be stored in the batch layer and denormalized data in the serving layer.

**Speed Layer**

- Requirements: Support for random reads/writres, scalability, fault tolerance.
- It is responsible for serving recent data views not included in the batch views, and therefore must support random writes.
- Random writes lead to complications associated with disk compaction and concurrent operations.
- It uses incremental computation to keep track of latest data, either synchronously or asynchronously.
- To provide high availability, conflict-free replicated data types and read repair algorithms can be used which support merging of divergent views. These add to the complexity of the speed layer.
- Strategy to expire views must not lead to incoherent data.

Note: Batch layer and Serving layer choose availability over consistency. But Speed layer aims to provide eventual consistency.

**Query Layer**

- It is responsible for making use of batch views and realtime views to answer queries.
