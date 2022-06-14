# My Resume

## Introduction

My name is JinHua Luo (罗锦华).

I am a senior C/C++/Python/Golang/Lua/SQL/Bash programmer and system architect, with 16 years development experience.

I am good at Linux, TCP/IP, Nginx, OpenResty, PostgreSQL.

## Location

Guangzhou, China

## Email

home_king@163.com

luajit.io@gmail.com

## Education

* bachelor, South China Agricultural University

## Career

* 2018 - 2019, Ericsson
* 2017 - 2018, Kugou Music
* 2008 - 2016, Elephant Talk
* 2005 - 2008, Guangdong Linux Center

## Projects

* [pgcat - Enhanced postgresql logical replication](#pgcat)
* [Cassandra Encryption query handler](#cassandra-encryption-query-handler)
* [Openresty worker-thread api](#openresty-worker-thread-api)
* [Mysql Proxy](#mysql-proxy)
* [luajit.io](#luajitio)
* [Image Server Cluster Refactoring](#image-server-cluster-refactoring)
* [Diameter Route Agent](#diameter-route-agent)
* [GSM SMSC](#gsm-smsc)
* [CDMA SSP](#cdma-ssp)
* [boost-reflection](#boost-reflection)
* [Diameter stack](#diameter-stack)
* [SIGTRAN stack](#sigtran-stack)
* [High performance CORBA implementation](#high-performance-corba-implementation)
* [Server Bootstrap CD](#server-bootstrap-cd)
* [Others](#others)

### pgcat

*2019 open-source project*

https://github.com/kingluo/pgcat

Enhanced postgresql logical replication.

Similiar to BDR, but it does not depends on specific postgresql version and non-invasive.

The built-in logicial replication has below shortages:

* only support base table as replication target
* do not filter any origin, which will cause bi-directional dead loop
* could not do table name mapping
* no conflict resolution

pgcat makes below enhancements:

* supports any table type as replication target
e.g. view, fdw, partitioned table, citus distributed table
* only replicates local changes
so that you could make bi-directional replication,
e.g. replicates data between two datacenter
* table name mapping
* optional lww (last-writer-win) conflict resolution
* save replication progress in table, so that it would be logged
when subscriber failovers, it would retain the progress. In contrast,
the built-in logical replication of pg saves the progress in non-logged file.

### Cassandra Encryption query handler

*2018 Ecrission*

*written in Java*

Implements encryption via Cassandra Query Handler.

* encrypt specific fields of tables transparently from client
* enable encrypt variant implmentations, e.g. aes, fuzz
* only the authorized users could access the encrypted fields
* support alias and composite types
* enable configuration updates on runtime

### Openresty worker-thread API

*2017 open-source project*

This API is useful when you need to execute the below types of tasks:

* CPU bound task, e.g. do md5 calculation
* File I/O task
* Call `os.execute()` or blocking C API via `ffi`
* Call external Lua library not based on cosocket or nginx

https://github.com/openresty/lua-nginx-module#ngxrun_worker_thread

### Mysql Proxy

*2017-2018 KuGou Music*

*written in golang*

* Auto/Manual failover (supports master-master)
* Write at master, Read at slaves in load-balance manner
* Sharding (supports perpared statement)
* Data re-balancing among Nodes
* Configuration and Metadata stored on zookeeper, could be changed on runtime

### luajit.io

*2015 open-source project*

Pure lua io framework, which re-implements the functionalities and performance of nginx and ngx_lua.

Why reinvent the wheel? Well, the nginx and ngx_lua is renowned at effciency and extensible, but they are written in C language, so you need to be as smart as the authors to contribute codes. What if the core is written in pure lua language, but without any effciency tradeoff? Then not only the web apps are extensible, but also the server core is extensible at ease by any levels of developers!

The Luajit is a perfect JIT engine to improve lua performance, so with dedicated and luajit-oriented design, the luajit.io would reassemble the advantages of nginx and ngx_lua, but provides extra benefit: simple and extensible at the core.

Just to emphasize that luajit.io already implements most common functionalities of http1.1, e.g. gzip, ssl, if_not_modified. Besides http, you could use it as general tcp server, just like what ngx_stream module does.

luajit.io simulates nginx architecture, including master-workers model, ip/port/domain matching, location matching (besides the nginx location directive semantics, you could define function to do arbitrary matching), response filters chaining, signal controlling and configuration file syntax.

And, the API is compatible with ngx_lua, including exec/redirect flow control, shared memory dictionary, dfa-pattern socket read, enhanced coroutine api, etc, so that luajit.io could reuse almost all lua-resty-* libraries directly (with some trivial naming changes).

See https://github.com/kingluo/luajit.io for detail.

### Image Server Cluster Refactoring

*2015 UCWeb*

The image server is used to convert image formats, which is CPU-bound app. The time cost per request ranges from 5ms to 5s, randomly.

In the first release, the front load balancer dispatch http requests from app servers (clients) to the backend image servers. The Apache TrafficServer runs at the backend server: the master thread dispatches http request to the thread pool in round-robin way. The number of the thread pool is the number of the CPU cores of the machine. The flaw of this design is that the requests are not dispatched among the CPU cores of machines evenly, then the average latency is high and unstable.

Think that at given moment, some threads get many request pending in the thread specific queue, while the other threads are idle but those pending requests could not be migrated to them.

How to improve it? Well, each thread is only avaliable only when they finished the previous request, it should announce its avaliabltiy to clients. So I use one redis server, create a queue on it, and apply the producer-consumer model to dispatch the requests. When the thread is avaliable, it push itself (ip/port info) to the queue. Each client would pop the queue to determine the handling thread. The pop operation is blocking for the redis queue, and the overhead is small. The http protocol between the client and the server is replaced by simple and clear protocol.

After refactoring, the overall performance of the cluster improves a lot!

### Diameter Route Agent

*2016 Elephanttalk*

Diameter Route Agent is like a full functional IP router, but it's diameter message dedicated router. The highlight of this project is you could use lua script to extend the route logic:

* host validation logic
* message routing logic, e.g. round-robin, weighted load-balance
* message mangling just like what iptables does

### GSM SMSC

*2014-2015 Elephanttalk*

The SMSC is a message router. The message originator or terminator could be mobile phone or sme (short mesasge entity) via smpp or http. The most common usage is phone-to-phone message. And the sme provides vendor specific service, For example, you could send message from you phone to a short number to query infomation of your bank account. Then the bank could in turn send back the result message to the phone.

The SMSC sits between the GSM network and the Internet. For the GSM, it communcates with various network entities, e.g. MSC, HLR to complete the MO and MT operations. Similarly, each service party (sme) connects to the SMSC via smpp or http protocol.

The PostgreSQL is used to store persistent data, e.g. sms and processing records, And it also do the message delivery scheduling, handling retry rules, routing rules and barring rules. The database would pg_notify the protocol adapter to do the delivery task. Table partitioning is used to archive history messages in month. The logic is written in pl/pgsql.

### CDMA SSP

*2014-2015 Elephanttalk*

CDMA SSP is service switching point, which connects to the SCP to apply pre-paid accounting, call forwarding, play announcement, Three-Way Calling, and other customized services upon the voice process.

The lightspot of this project is that I embed the luajit upon the low-level protocol stack, so that we could implement the business logics in pure lua. The system is then extensible at ease for any usecase.

I implement the WIN protocol (CDMA intelligent network) in the SIGTRAN stack, with API exported via CORBA. We have ton of test cases, while C lanuage is not so productive. The lua is good embedding language, simple but powerful. And the luajit's ffi could access the idl generated structures without conversion. Then I decide to wrap the whole ssp core with lua API.

Just like ngx_lua, the CORBA C API (generated from the idl) are asynchronous, but the lua API is coroutine based, synchronous and nonblocking. The lua API is high-level, which may involves multiple low-level CORBA operations invocations, for example, opening dialog, sending operatioin, handleing operation callback, closing dialog. The core handles all low-level stuff, e.g. the dialog management, the state machine, and the memory management.

### boost-reflection

*2012-2013 open-source project*

As known, C++ lacks of reflection, which is important when you're buidling a large-scale framework, like Java Spring.

I bring Java reflection, Java annotations, and Java proxy object into C++ land, in non-intrusive way.

It supports all modern compilers, no generator or additional tools needed.

See the github page for source codes:

https://github.com/kingluo/boost-reflection

### Diameter stack

*2012-2013 Elephanttalk*

Diameter is an important AAA (Authenticate, Authorize, Accounting) protocol in telecom.

Implemented protocols layers:

* SCTP (kernel based)
* diameter base
* dcca

### SIGTRAN stack

*2008-2013 Elephanttalk*

SS7 over IP network.

It's the base of telecom networking, just like the role of TCP/IP stack in Internet.

Implemented protocols layers:

* SCTP (kernel based)
* M3UA
* SCCP
* TCAP
* MAP / CAP / WIN

### High performance CORBA implementation

*2008-2013 Elephanttalk*

CORBA is a well-known RPC standard, but it has below disadvantages:

* heavy weighted, e.g. object proxy, message marshaling
* complex async call extension
* transport (GIOP) is too strict and low effiencicy

This project is to design and develop a RPC framework based on simplified and optimized CORBA version, used by components from our product lines.

It consists of:

* idl compiler (compile idl into C stubs and skeleton files), written in perl and tcl
* configuration compiler (tcl)
* api library
* management tools
* launcher
* tracker daemon (process tracker, site clustering)

Features:

* supporting Linux and Windows
* application in shared object library, loaded by the launcher
* configuration via tcl (the schema is defined in idl, and you could retrieve them in C types at runtime)
* object failover and load-balance
* fully asynchronous call just like nodejs, as well as tranditional synchronous call
* make full use of shared memory and unix domain socket
* address resolution via in-shared-memory distributed db
* no message marshaling and demarshaling
* lz4 compression for large message
* support CORBA_Any type in idl (think about what golang interface could do)
* failure aware and callback
* managed memory access and recycle for call arguments

### Server Bootstrap CD

*2006 GuangDong Linux Center*

The Bootstrap CD is a special compact disc to guide the Administrators to install OS onto the bare server machine at ease. It boots the bare machine into a special in-memory gentoo linux, which provides installation wizard which guides the user to configure the target OS (Redhat, Suse, Windows) installation, then it installs the OS automatically, and finally after reboot, the user get a full system with OS installed and integrated drivers for the particular machines.

The system is based on a dedicated MVC framework. The backend is written in Bash scripts following the configuration-driven model of 'portage' (the flexible package management system of Gentoo Linux distribution), while the front-end is written in Java (tomcat). The backend and the front-end are communicated through an .ini file, which is composed of fields used to synchronize the function logic of each other.

It makes use of many techniques, e.g. initrd, squashfs, unionfs, sysfs (devices scanning), OS installer hijacking (for Linux, it injects the drivers and post-installation scripts into the target initrd and installer, e.g. redhat Anaconda; for Windows, it constructs a special freedos image to launch auto-installtion).

### Others

*2005-2008 GuangDong Linux Center*

#### lvs

I submit a transparent proxy patch to the project author.

http://archive.linuxvirtualserver.org/html/lvs-users/2006-11/msg00261.html

http://thread.gmane.org/gmane.linux.network/50686

http://thread.gmane.org/gmane.linux.network/51121

#### drbd

I submit some bugfix patches.

http://lists.linbit.com/pipermail/drbd-cvs/2006-February/000993.html

#### u-boot

I implement the shell history extension and new hardware ports.

http://u-boot.10912.n7.nabble.com/My-3-patches-td2038.html

#### ibox livecd

I create my own linux distribution based on Gentoo at school:

http://distrowatch.com/table.php?distribution=ibox
