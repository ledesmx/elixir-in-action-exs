# First steps

Today most applications are driven and supported by a server system that processes
requests, crunches data, and pushes relevant information to many connected clients.
These systems have some nonfunctional requirements in common. The system must be
responsive, regardless of the number of connected clients. The effects of
unexpected errors must be minimal, instead of affecting the entire system. Ideally,
the system should never crash or be takend down, not even during a software
upgrade. It should be always up and running.

## About Erlang

Erlang is a development platform for building scalable and reliable systems that
constantly provide service with little or no downtime.

Despite being originally built for telecom systems, Erlang is a general-purpose
development platform that provides special support for technical, nonfunctional
challegens, such as concurrency, scalability, fault tolerance, distribution, and
high availablility.

### High availability

Erlang was specifically created to support the develoment of highly available
systems. To make systems work 24/7 without any downtime, you must first tackle some
technical challenges:
- **Fault tolerance**--A system must keep working when something unforseen happens.
Whatever happens, you want to localize the effect of an error as much as possible,
recover from the error, and keep the system running and providing service.
- **Scalability**--A system should be able to handle any possible load. You should be
able to respond to a load increase by adding more hardware resources without any
software intervention. Ideally, this should be possible without a system restart.
- **Distribution**--To make a system that neve stops, you need to run it on multiple
machines. This promotes the overall stability: if a machine is taken down, another
one can take over. Furthermore, this gives you the means to scale horizontally.
- **Responsiveness**--A system should always be reasonably fast and responsive. Even
if the load increases or unexpected errors happen. Occasional lenghty tasks shouldn't
block the rest of the system.
- **Live update**--You may want to push a new version of your software without
restarting any servers. You don't want to disconnect esablished connections while you
upgrade the software.

Erlang provides tools to address thes challenges.

### Erlang concurrency

Concurrency is at the heart and soul of Erlang systems. Instead of relying on
heavyweight threads and OS processes, Erlang takes concurrency into its own hands.

The basic concurrency primitive is called an Erlang process, and typical Erlang
systems run thousands, or even millions, of such processes. The Erlang virtual
machine, called Bogdan/Björn’s Erlang Abstract Machine (BEAM), uses its own
schedulers to distribute the execution of processes as much as possible. The way
processes are implemented provides many benefits.

#### Fault tolerance
Erlang processes are completely isolated from each other. They share no memory,
and a crash of one process doesn't cause a crash of other processes. Erlang provides
you with the means to detect a process crash and do something about it.

#### Scalability
Processes communicate via asynchronous messages. There are no complex synchronization
mechanisms, the interaction between concurrent entities is much simpler to develop
and understand.

Typical Erlang systems are divided into a large number of concurrent processe. The
virtual machine can efficiently parallelize the execution of processes as mush as
possible. Because they can take advantage of all available CPU cores, this makes
Erlang systems scalable.

#### Distribution
Communication between processes works the same way regardless of wheter these processes
reside in the same BEAM instance or on two different instances on two separte, remote
computers. This gives you the ablility to run a cluster of machines that share the
total system load.

Aditionally, running on multiple machines makes the system truly resilient.

#### Responsiveness
The runtime is specifically tuned to promote the overall responsiveness of the system

- Erlang takes the execution of multiple processes into its own hands by employing
dedicated schedulers that interchangeably execute many Erlang processes.
*A scheduler is preemptive*--it gives a small execution window to each process and then
pauses it and runs another process. A single long-running process can't block the rest
of the system.

- I/O opreation are internally delegated to separate threads, or a kernel-poll service
of the underlying OS is used. Any process that waits for an I/O operation to finish
won't block the execution of other processes.

- Garbage collection is specifically tuned to promote system responsiveness. As 
processes are completely isolated, this allows per=process garbage collection; instead
of stopping the entire system, each process is individually collected. Such collections
are much quicker and don't block the entire system for long periods of time.

### Server-side systems

Erlang can be used in various applications and systems. Its sweet spot lies in
server-side systems--sytems that run on one or more servers and must serve many clients
simultaneously.

The term *server-side system* indicates that it's more than a simple server that
processes requests. It's an entire system that, in addition to handling requests, must
run various backgrounds jobs and manage some kind of server=wide in-memory state.

This is where Erlang can make your life significantly simpler. Every component can be
implemented as an erlang process, which makes the system scalable, fault-tolerant, and
easy to distribute.

It's important to notice that Erlang tools aren't always full-blown alternatives to
mainstream solutions, such as web server, database servers, and in-memory key-value 
stores. But Erlang gives you options, making it possible to impplement an initial
solution using exclusively Erlang and resorting to alternative technologies when 
Erlang solution isn't sufficient.

Erlang isn't an isolated island. It can run in-process code written in languages as
C, C++ or Rust and can communicate with external components such as message queues,
in-memory key-value stores, and external databases. Therefore, you aren't deprived of 
using existing third-party technologies.

### The development platform

Erlang is a full-blown development platform consisting of four distinct parts: the
language the virtual machine, the framework, and the tools.

Erlang the language is the primary way of writing code that runs in the Erlang virtual
machine.

The source code is compiled into bytecode that's then executed in the BEAM.

The standard part of the release is a framework called Open Telecom Platform (OTP). It's
a general purpose framework that abstracts away many typical Erlang tasks, including the
following:
- Concurrency and distribution patterns
- Error detection and recovery in concurrent systems
- Packaging code into libraries
- Live code updates

The tools are used for several typical tasks, such as compiling Erlang code, starting the
BEAM instance, and so on.

### Relationship to microservices

Due to its concurrency model and how it is used to increase the availability of the
system, Erlang is sometimes compared to microservices. 

Splitting the system into multiple services can improve system fault tolerance and
scalability. At first glance, it seems we can get all the benefits of Erlang by splitting
the system into services, escially if we keep the services smaller in size and scope.

While it is true that there is some overlap berween microservices and Erlang concurrency,
it's worth pointing out that the latter leads to much mor fine-grained concurrency. This
would improve the system responsiveness, provide the potential for vertical scalability,
and increase fault tolerance. You can't really simulate this with microservices alone
because ir would require too many OS processes.

Microservices offer some important benefits that are not easily achieved with BEAM.
The ecosystem, including tools such as Dock and Kubernetes, significantly simplifies the
operational tasks, such as deployment, horizontal scaling, and coarse-grained fault
tolerance. Therefore, BEAM concurrency and microservices complement each other well.

Erlang gives you a lot of flexibility when choosing your architecture, without forcing
you to compromise the availability of the system. You can opt for a plain monolith and
gradually move to the (micro)services architecture as the system grows in size and
complexity.

## About Elixir

Elixir is an alternative language for the Erlang virtual machin that allows you to write
cleaner, more compact code that does a better job fo revealing your intentions.

Elixir targets the Erlang runtime. The result of compiling the Elixir source code is
BEAM-compliant bytecode files that can run in a BEAM instance and can normally cooperate
with pure Erlang code. There's nothing you can do in Erlang that can't be done in Elixir,
and usually, the Elixir code is as performant as its Erlang counterpart.

### Code simplification

One of the most important benefits of Elixir is its ability to radically reduce boilerplate
and eliminate noise from code, which results in simpler code that's easier to write and
mantain.

The following example Erlang code implements a simple server process that adds two numbers.
```erlang
-module(sum_server).
-behaviour(gen_server).

-export([
 start/0, sum/3,
 init/1, handle_call/3, handle_cast/2, handle_info/2, terminate/2,
 code_change/3
]).

start() -> gen_server:start(?MODULE, [], []).
sum(Server, A, B) -> gen_server:call(Server, {sum, A, B}).

init(_) -> {ok, undefined}.
handle_call({sum, A, B}, _From, State) -> {reply, A + B, State}.
handle_cast(_Msg, State) -> {noreply, State}.
handle_info(_Info, State) -> {noreply, State}.
terminate(_Reason, _State) -> ok.
code_change(_OldVsn, State, _Extra) -> {ok, State}.
```

The Elixir version of the same server process.
```elixir
defmodule SumServer do
 use GenServer

 def start do
   GenServer.start(__MODULE__, nil)
 end

 def sum(server, a, b) do
   GenServer.call(server, {:sum, a, b})
 end

 def handle_call({:sum, a, b}, _from, state) do
   {:reply, a + b, state}
 end
end
```

Despite being significantly smaller, the Elixir version still feels somewhat noisy. The
excess noise exists because Elixir retains a 1:1 semantic relation to the underlying
Erlang library.

But Elixir gives you tools to further eliminate whatever you may regard as noise and
duplication. For example, Saša Jurić developed his own Elixir library, called ExActor.
```elixir
defmodule SumServer do
 use ExActor.GenServer

 defstart start

 defcall sum(a, b) do
   reply(a + b)
 end
end
```
The final implementation of the sum server process is powered by Elixir macros facility.
A macro is Elixir code that runs at compile time. Macros take an internal representation
of your source code as input and can create alternative output.

Because macros are written in Elixir, they're flexible and powerful, making it possible
to extend the language and introduce new constructs that look like an integral part of
the language.

Due to its macro supppot and smart compiler architecture, most of Elixir is written in
Elixir. Language constructs like `if` and `unless` are implemented via Elixir macros.

### Composing functions

Both Erlang and Elixir are functional languages. They rely on immutabl data and functions
that transform data. Code is divided into many small, reusable, composable functions.

Unfortunately, the composablility feature works clumsy in Erlang.
```erlang
process_xml(Model, Xml) ->
 Model1 = update(Model, Xml),
 Model2 = process_changes(Model1),
 persist(Model2).
```
```erlang
process_xml(Model, Xml) ->
 persist(
   process_changes(
     update(Model, Xml)
   )
 ).
 ```

Elixir gives you an elegant way to chain multiple function calls together.
```elixir
def process_xml(model, xml) do
 model
 |> update(xml)
 |> process_changes()
 |> persist()
end
```

### The big pixture

- The API for standard libraries is cleaned up and follows some defined conventions.
- Syntatic sugar is introduced that simplifies typical idioms.
- A concise syntac for working with structured data is provided.
- String manipulation is improved.
- *Mix* simplifies common tasks, such as creating applications and libraries, managing
dependencies, and compiling and testing code.
- *Hex* (package manager) makes it simpler to package, distribute, and reuse dependencies.

## Disadvantages

### Speed

Erlang programs are run in the BEAM and, therefore, can't achieve the speed of
machine-compiled languages. But it isn't poor engineering. The goal of the platform isn't
to squeeze out as many requests per second as possible but to keep performance as
predictable and within limits as possible.

Intensive CPU computations aren't as performant as the low level counterparts, so you may
consider implementing such task in some other language and the integrating the
corresponding into your Erlang system. If most of your system's logic is heavil CPU bound,
you should probably consider some other technology.

### Ecosystem

The ecosystem built around Erlang isn't small, but it definitely isn't as large as that
of some other languages.

You should be aware that your choice of libraries won't be as abundant as you may be used
to. Keep in mind all the benefits you get from Erlang. Erlang's significant aid in
solving hard problems makes it a useful tool.

Perhaps you don't expect a high load or a system doesn't need to run constantly and be
extremely fault tolerant. In such cases, you may want to consider some other technology
stack.
