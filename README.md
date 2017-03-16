# Network-overhead-in-microservices-architecture
This work aim to give you all the information about the latency in a microservice architecture. You can use this information to configure your own Circuit breaker such as "Hystrix" https://github.com/Netflix/Hystrix/wiki.

===================

# Orizzontal architecture 
The first scenario involves a simple architecture, composed by well-isolated micro-services which have fully satisfied the independence principle. This kind of scenario is called “horizontal (or mono-level) architecture”. Let’s imagine a simple e-commerce with the possibility of visualizing the items, of checking an order shipping status and of setting the default details for the user, so that it is possible to memorize one’s favourite shipping addresses or methods of payment.
The three micro-services do not have the necessity of communicating within themselves but still the inquiry of one of them involves the one itself, the edge-service and, in case, the service-registry.

![orizzontal-architecture](https://github.com/Marcuccio/Network-overhead-in-microservices-architecture/blob/master/img/orizzontal-architecture.jpg)

## Analyses ##
With respect to a classical monolithic architecture in which the network time would be reduced to the only communication between the client and the server, this architecture, even though really simple, adds network over-head, caused by the work done by the edge-service, and eventually by the service-registry. However the network over-head is really low and the answer delay perceived by the user is practically null. The picture below we can compare the network times that are the result of the above descripted architecture with those of a monolithic architecture. The monolithic architecture’s minimum latency, in the considered example, is around 24 milliseconds, in the case of a micro-serviced architecture this rise up till 49 milliseconds, as the result of the edge-service intervention that doubles the necessary connections.   A particular attention needs to be paid to the maximum time. Whilst in a monolithic application this remains really similar to the minimal one, in a micro-service application the maximum time corresponds to the serviceregistry intervention; This intervention introduces a new connection, trebling it with respect to a monolithic architecture. 
The average time is really near to the minimum time. The service-registry intervention is needed only if the registered instances in the edge-service is expired and we are the first ones to want to be served. The probability of having to wait a call to the service-registry is therefore distributed in a temporal fan “eureka-free” of about 30 seconds and between all the other clients that could update the cache for us.
 
![orizzontal-architecture_plot](https://github.com/Marcuccio/Network-overhead-in-microservices-architecture/blob/master/img/plotbot_architectures.JPG)

# Vertical architecture #
The second scenario represents larger systems. Often “bigger dimensions” is synonym  of “complexity” and – consequently- of tighter links between the services. It may happen that two services -even if they are sharing common workflows- because of their strongly heterogeneous context, remain separated micro-services. Looking back at the e-commerce example of the previous paragraph, we can assume of implementing an item purchasing phase. Once one has click on “confirm payment” and before getting the confirmation of succeeding in it, probably our micro-service architecture had inquiry a service in charge of checking the actual presence of that item in storage; a micro service in charge of take note of  the orders on behalf of a seller (who has to be notified so that he can prepare the shipping), a micro-service that of course provides for the payment validation and probably at the top of this flow of calls we have inquired a micro-service put in place to authenticate the user in order to be able to record the interests and the buying.   The above mentioned actions all belong to different contexts but their tight link is inevitable and we have to be sure that every action has been executed before assure the user of the request taking over.   In similar architectures the network over-head grows up exponentially with respect to the monolithic architectures.

![vertical-architecture](https://github.com/Marcuccio/Network-overhead-in-microservices-architecture/blob/master/img/vertical-architecture.jpg)

## Analyses ##
As we have said is in this architecture that the network overhead become substantial. The presence of synchronous  calls within the several micro-services, whose  variable multitude has to be borne in mind already in design phase. Notwithstanding in the current literature there are not exhaustive information regarding that subject.  In below the pictures on the “x” axe are represented chains of calls that involve several micro-services; Respectively from two to seven microservices . For each situation are described the minimum times, the average ones and the network over-head limits. As in the horizontal architecture previous case, the average times are near to the minimum ones. Also in this case the motivation is in the probability to have to inquiry in a cascade, about network topology, unaware micro-services . Probability which decreases proportionally with the increase of their number. The times, above described, demonstrate  how in a micro-services architecture it is easy to reach network times that are greater than 500% with respect to monolithic architectures. This percentage can increase till reaching 1000% in extreme cases. In addition to it such long network period imply not insignificant charges to the developer. One of the most common errors in the web environment is :“408 Request Timeout”.  In a call succession, as in the above described scenario, to have information about the average answering time means to be able to handle the errors  accurately.

![vertical-architecture_plot_2-3](https://github.com/Marcuccio/Network-overhead-in-microservices-architecture/blob/master/img/plotbox_2-3.JPG)

![vertical-architecture_plot_4-5](https://github.com/Marcuccio/Network-overhead-in-microservices-architecture/blob/master/img/plotbox_4-5.JPG)

![vertical-architecture_plot_6-7](https://github.com/Marcuccio/Network-overhead-in-microservices-architecture/blob/master/img/plotbox_6-7.JPG)
