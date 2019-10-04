### Load Balancer

- Can be software based or hardware based
- Load balancing algorithms
  - Round Robin - request is distributed sequentially among the servers
  - Weighted Round Robin - same as round robin but some servers get more traffic depending on the weightage defined on them.
  - Least connections - request is sent to the server having least number of connections.
  - Least response time - request is sent to the server with the least response time.
- Hardware Load Balancers are layer 4 and layer 7 OSI model load balancers.
- Weighted load balancers are used when we have resources of different type, that is different memory, compute etc.
