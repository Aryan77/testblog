
---
layout: default
---

You have a Node.js application that works perfectly well locally, but want to ensure that it will be able to handle the throughput required by your enormous user-base in production. An obvious way of going about this would be to use a more powerful server to host your application in. This is vertical scaling; it has the advantage of you not needing to change the application code at all - simply running it on a more powerful server that can handle a larger throughput is enough. This strategy has the potential to become very expensive very quickly, however. A much more scalable solution is horizontal scaling, which we will explore in this article. 

Horizontal scaling essentially involves duplicating your application instance such that each instance can handle a part of the incoming connections. A simple example of this would be spawning a process (which runs your application instance) for each core in your machine. This means that these processes run on the same port, and a load balancer is used to distribute the incoming connections so each of the application instances in the different processes can handle a part of the load - alleviating the total load falling on a single running instance of the application. 

As for how this is achievable in Node.js, you can implement it using the `cluster` module, which is a [standard Node.js library](https://nodejs.org/api/cluster.html). One instance of your process is responsible for spawning a worker process for each available core - these worker processes will then run an instance of the application. Incoming connections are distributed using a simple round-robin strategy, and since all the workers expose the service on the same port, the client making use of this service sees no difference whatsoever in the way it interacts with the service. Since this is a standard Node.js library, it is fairly limited (but very extensible) in its implementation. I would personally not recommend using `cluster` on its own (I have personally never had to use it for any of my scaling needs, though I would be very interested if any of you have had to use it over more advanced libraries) - there is a much better solution that handles managing the code within the main and worker processes automatically - and that is using the process manager [PM2](https://pm2.keymetrics.io). This library abstracts away the manual management of main and worker threads - you simply write your code as if it were a regular Node.js application running on a single process - and the PM2 daemon will spawn the main process, the different worker processes and handle the load balancing for you. It also provides a simple API to allow changes to the number of processes. I especially like how PM2 will automatically restart a crashing process, and can be very easily configured to perform reloads such that at least one process is always available at any given moment. 

While PM2 will make full use of a given computer's resources, it is still entirely possible that the throughput required for your application far exceeds the amount that a single computer can handle. In this case, you would have multiple deployed servers. The main concept is still the same - we just make use of all the new cores we just obtained with these new machines to run new instances of our application - so within each of the servers we still use PM2 to make full use of that specific server's cores. But now, we need an additional load balancer that will distribute the load among the servers themselves. If anyone wants to manually set this up on a self-hosted system, I would direct you to [NGINX](https://www.nginx.com). (Disclaimer: I have personally never self-hosted an application if it was at a scale that it required load-balancing, so I am not an expert on this specific tool, but it is an extremely popular and well-devised tool, and a cursory overview of it suggests it is a very powerful tool for this task). I would personally recommend any large-scale system like this be hosted on a cloud platform like AWS or GCP - to make full use of their managed load balancer - AWS has ELB for example that is very easy to set up with the rest of your code infrastructure - (assuming the different servers in this case are EC2 instances running your application).

Finally, let's discuss what architectural changes you would need to make in your application to make it work as intended with these scaling strategies. It is, obviously, important to make sure that there is a storage service that your application uses that is entirely separate from the application code itself - since your processes are running on different servers, saving any external data to the filesystem will only make it available on the server whose filesystem the data was stored in. 

Another decoupling that I would suggest falls in line with general good practices in developing any microservice-based architecture - and that involves scaling the application and the database separately. I have, once before, attempted to run both the application code and the database that the application was using on the same machine, and scale accordingly. This was a huge waste of resources as the database throughput required was much less than the application throughput - so I was creating many unnecessary duplicates of the database when I should have scaled them independently so a few database instances could have served the multitude of application instances. 