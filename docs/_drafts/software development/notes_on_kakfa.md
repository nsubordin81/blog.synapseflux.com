# Apache kafka 

quick, what do you know about kafka? I know it is about event driven architectures, I know it has things like partitions, I know that it retains messages for a long time, I know that it is based on something like a pub-sub model which I know is a way to have one entity in a system publish messages and have others subscribe to them. I know that observer pattern and reactive functional programming are based on similar things and all enable different things to be done, and it has to do with making suer that events don't get lost and also that they arrive only once and that the sender and receiver don't  need to konw more about themselves they just both need to participate in the pattern together. I also know that they talk about use cases for kafka for event buses for microservices, data pipelines and the like. 

so from all that, before digging in further, I'd say that kafka is a messaging solution of some kind that is inspired by systems where things happen in real time and don't always succeed, to ensure stability when working with distributed systems and also make it so that components don't have to know details about things about each other. That feels pretty vague, good thing I'm reading this book. 

first section: moving data around between creaating and then analyzing is important. doing it faster is important. 

Pub/Sub - critical pattern element. publishers don't send messages directly to subscribers. they just tag the message and then subscribers know to consume it. 

how the need for kafka tends to evolve: 
- starting with a simple use case where you have created metrics data somewhere and need to push it somewhere else so you have a direct connection. 
- problem encountered, analyzing metrics in the long term. maybe another service to receive, store, analyze. 

where are you? one app creating metrics and then posting them for display, then another app that is getting metrics and analyzing them for long term stuff, but that also receives from the source app that creates them. 

someone adds requirement for active polling of the services taht provide metrics. over time you have a lot of services that rely on the central service the dependency graph between them is a bit complicated. 

pub sub fixes this tangle of dependencies between systems with direct connections but putting a single broker between them that all data producers are sending to and all data consumers are reading from. essentially, feels like what is happening is they are shrinking the network graph edges. that is a great way of visualzing the benefit

# potential blog post. visualize the benefit of adding pub sub in terms of edge removal in tghe dependency graph of systems. 

the book postulates kafka as a way to deal with the fact that pub-sub systems have to be rebuilt for every domain of data transfer you want, offering a generic way to do it that works for all use cases that pub sub applies to. 

'distributed commit log' 'distrubuted streaming platform' 

a commit log is a way to reconstruct your data from commits that are detailed and carefully recorded enough that they can be replayed to get the state of the data back entirely, multiple times. this has to do with what the events are and what order they are stored in. 

kafka is like that but it is distrubuted so it can scale and also provide protections against faults with any one system. 

message is the unit of data for kafka. messages can contain keys which are mostly used to ensure a message is always sent to the same partition using hashes and modulo arithmetic. 

batches, collections of messages going to the same partition and topic. (we haven't defined those terms yet)

latency throughput tradeoff: more messages in a batch means you can send more at once with less overhead, however it means waiting longer on average for any given message to process. 

schema is formatting for the message structure, XML and json are classic options, a new, probably better one is apache avro. Avro provides forward and backward compatibility, strong typing, schemas that can be stored separately from code and don't require regeneration after changing





