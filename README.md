# My Resume

## Introduction

My name is JinHua Luo (罗锦华).

I'm a software engineer with 18 years of development experience. I love designing and coding. My main tech stack is PostgreSQL, nginx, and TCP/IP programming. My favorite programming languages are C, C++, Golang, and Python, as well as SQL. I have worked in various companies and on many server backend projects such as CORBA implementation, PostgreSQL middleware, high-performance HTTP proxy, etc. I have two popular open-source projects: one is pgcat, the enhanced logic replication for PostgreSQL, and the other is lua-resty-ffi, which enables hybrid programming in nginx and envoy. I am also proficient in using GDB, eBPF, and Systemtap to analyze program problems.

## Location

Foshan, China

## Email

luajit.io@gmail.com

## Links

http://luajit.io

https://www.linkedin.com/in/jinhua-luo-b52b6bbb/

https://github.com/kingluo

https://www.zhihu.com/people/jinhua-luo-94

## Education

* Bachelor, Electronic Information Engineering, South China Agricultural University

## Career

* 2022 - 2023 **API7 https://api7.ai/**

  Develop and maintain Apache project APISIX (open-source API gateway):

  https://github.com/apache/apisix/

  * Refactor the core code and write new plug-ins
  * Customer SLA support (such as Honor, Geely, vivo)
  * Write technical articles on various topics
  * on call, handle issues in Slack and GitHub

* 2019 - 2022 **_PostgreSQL consultant (Self-employment)_**
  * Postgresql cluster, cross-DC replication, pg_raft
  * Transparent, horizontal scaling shards

* 2018 - 2019 **Ericsson**
  * Cassandra development

* 2017 - 2018 **Kugou Music**
  * Mysql Proxy

* 2008 - 2016 **Elephant Talk**
  * SS7 protocol stack
  * CORBA platform development
  * Telecom function node development, e.g. SMSC, SSP

* 2005 - 2008 **Guangdong Linux Center**
  * Embedded Linux
  * Linux HA, LVS, DRBD

## Projects

### Technical articles

*2022.10 - 2023.10 API7*

* What Is OAuth?

https://www.apiseven.com/blog/what-is-oauth

https://api7.ai/blog/apisix-supports-oauth-and-oidc

* Apache APISIX Lua inspect plugin

https://www.apiseven.com/blog/apisix-inspect-plugin

https://api7.ai/blog/apisix-lua-dynamic-debugging-plugin

* etcd vs PostgreSQL

https://www.apiseven.com/blog/etcd-vs-postgresql

https://api7.ai/blog/etcd-vs-postgresql

* SOAP-to-REST (zero code)

https://www.apiseven.com/blog/api-7-enterprise-soap

https://api7.ai/blog/apisix-soap-to-rest-plugin

* APISIX: Be FIPS 140-2 Compliant

https://www.apiseven.com/blog/api7-enterprise-fips

https://api7.ai/blog/apisix-fips-140-2

### Merge QUIC patches

*2023.08 - 2023.09 API7*

Ported nginx-1.25 to APISIX to support downstream QUIC (HTTP/3).

Furthermore, based on the latest version of curl, I designed and implemented a simple but flexible testing framework:

https://github.com/kingluo/burl

### Optimize APISIX etcd watch

*2023.05 - 2023.05 API7*

apisix uses HTTP to connect etcd to obtain configuration updates, and apisix has more than ten kinds of resources. Each resource has a TCP connection, and the TCP connection of each worker process is independent, so there are many TCP connections between apisix and etcd, which affects Scalability.

This project aims to reconstruct the logic of incremental update configuration of etcd watch, merge multiple HTTP connections into one connection, and improve the real-time performance of watch so that similar watch effects can be achieved without introducing GRPC to connect etcd.

https://github.com/apache/apisix/pull/9456

https://github.com/kingluo/etcd-benchmark

### SOAP-to-REST plugin

*2023.04 - 2023.04 API7*

Implement the SOAP proxy function, convert the downstream RESTful request into a SOAP XML request based on the WSDL file, send it to the upstream Web Service, and then convert the SOAP response into a RESTful response and return it to the downstream.

The biggest advantage of this project is that it can automatically download and analyze WSDL files, and automatically create proxy objects for each operation in it. The object method parameters are all Python objects and can be converted to and from JSON format. Compared with traditional products, there is no need to manually write conversion templates or codes for each WSDL file, which greatly improves development efficiency and reduces error rates.

https://github.com/kingluo/lua-resty-ffi-soap

### MTLS whitelist

*2023.04 - 2023.04 API7*

According to the SSL standard, in the case of MTLS (mutual TLS, two-way certificate), during the SSL handshake, if the client certificate is invalid, an alert will be returned to the client immediately and the creation of the SSL session will be interrupted. However, in reality, there is a practical need to have a whitelist of URLs (which can also be headers). If the URL visited by the client is in the whitelist, the validity of the client certificate will not be checked, and other URL requests will return HTTP 400 error codes.

This project implements the above whitelist logic by setting the SSL handshake callback function through OPENSSL's SSL_CTX_set_cert_cb:

https://www.openssl.org/docs/man3.1/man3/SSL_CTX_set_cert_cb.html

Product documentation:

https://apisix.apache.org/docs/apisix/tutorials/client-to-apisix-mtls/#mtls-bypass-based-on-regular-expression-matching-against-uri

Source code link:

https://github.com/apache/apisix/pull/9322

### Enable FIPS

*2023.03 - 2023.03 API7*

Integrate openssl-3.0, enable FIPS mode, and fix incompatible function paths.

https://www.openssl.org/docs/fips.html

https://github.com/apache/apisix/pull/8298

### body transformer plugin

*2023.01 - 2023.01 API7*

The plug-in recognizes the content format of request and response (currently supports XML and JSON), and then converts it into the content required by the customer through templates.

https://apisix.apache.org/docs/apisix/plugins/body-transformer/

### openresty code contributions

*2022.01 - 2023.01 API7*

* bugfix: define busy_bufs per cosocket #2119
https://github.com/openresty/lua-nginx-module/pull/2119

* bugfix: avoid buf double free in ngx.print if lua body filter enabled #2115
https://github.com/openresty/lua-nginx-module/pull/2115

* feature: add shdict APIs into worker thread #2074
https://github.com/openresty/lua-nginx-module/pull/2074

* api inject bugfix #1996
https://github.com/openresty/lua-nginx-module/pull/1996

### APISIX authentication plugins

*2022.08 - 2022.12 API7*

* feat(ldap-auth): use lua-resty-ldap instead of lualdap #7590
https://github.com/apache/apisix/pull/7590

* feat(saml-auth): add saml-auth plugin #7827
https://github.com/apache/apisix/pull/7827

* feat: add cas-auth plugin #7932
https://github.com/apache/apisix/pull/7932

### Code contributions to APISIX upstream library

*2022.11 - 2022.11 API7*

* Prometheus memory leak problem

https://github.com/apache/apisix/issues/9627

https://github.com/knyar/nginx-lua-prometheus/pull/151

* Prometheus shared memory read and write failure problem

https://github.com/knyar/nginx-lua-prometheus/pull/147

### lua-resty-ffi

*2022 open-source project*

https://github.com/kingluo/lua-resty-ffi

lua-resty-ffi provides an efficient and generic API to do hybrid programming in openresty/envoy with mainstream languages
(Go, Python, Java, Rust, etc.).

**Features:**

* nonblocking, in a coroutine way
* simple but extensible interface, supports any C ABI compliant language
* once and for all, no need to write C/Lua codes to do coupling anymore
* high performance, faster than unix domain socket way
* generic loader library for Python/java
* any serialization message format you like

### pg_watch_demo

*2022 open-source project*

https://github.com/kingluo/pg_watch_demo

With trigger and notify, you could re-implement a complete (even better) etcd watch mechanism in Postgresql.

It mimics etcd features:

* watch
* read the value in historical data, i.e. get the key by revision
* set key
* del key
* Compact, either by revision or date retention

### lua-resty-inspect

*2022 open-source project*

https://github.com/kingluo/lua-resty-inspect/

It's useful to set arbitrary breakpoints in any lua file to inspect the context information, e.g. print local variables if some condition is satisfied.

In this way, you don't need to modify the source codes of your project and just get diagnose information on demand, i.e. dynamic logging.

This library supports setting breakpoints within both interpreted function and the jit compiled function. The breakpoint could be at any position within the function. The function could be global/local/module/anonymous.

It works for luajit2.1 or lua5.1.

### pgcat

*2019 open-source project*

https://github.com/kingluo/pgcat

Enhanced Postgresql logical replication.

Similar to [BDR](https://www.enterprisedb.com/docs/bdr/latest/), it does not depend on a specific Postgresql version and is non-invasive.

The Postgresql built-in logical replication has below shortages:

* only support base table as a replication target
* do not filter any origin, which will cause a bi-directional dead loop
* could not do table name mapping
* no conflict resolution

pgcat makes below enhancements:

* supports any table type as a replication target
e.g. view, fdw, partitioned table, citus distributed table
* only replicates local changes
so that you could make bi-directional replication,
e.g. replicates data between two data centers
* table name mapping
* optional lww (last-writer-win) conflict resolution
* save replication progress in a table, so that it will be logged
when subscriber failovers, it would retain the progress. In contrast,
the built-in logical replication of pg saves the progress in a non-logged file.

### Cassandra Encryption query handler

*2018 Ecrission*

*written in Java*

Implements encryption via Cassandra Query Handler.

* encrypt specific fields of tables transparently from the client
* enable encrypt variant implementations, e.g. AES, fuzz
* Only authorized users could access the encrypted fields
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
* Write at master, Read at slaves in a load-balance manner
* Sharding (supports prepared statement)
* Data re-balancing among Nodes
* Configuration and Metadata stored on zookeeper, could be changed on runtime

### luajit.io

*2015 open-source project*

Pure Lua io framework, which re-implements the functionalities and performance of nginx and ngx_lua.

Why reinvent the wheel? Well, the nginx and ngx_lua are renowned for efficiency and extensibility, but they are written in C language, so you need to be as smart as the authors to contribute codes. What if the core is written in pure Lua language, but without any efficiency tradeoff? Then not only the web apps are extensible, but also the server core is extensible at ease by any level of developers!

The Luajit is a perfect JIT engine to improve Lua performance, so with a dedicated and luajit-oriented design, the luajit.io would reassemble the advantages of nginx and ngx_lua, but provides extra benefit: simple and extensible at the core.

Just to emphasize luajit.io already implements the most common functionalities of http1.1, e.g. gzip, SSL, if_not_modified. Besides HTTP, you could use it as a general tcp server, just like what ngx_stream module does.

luajit.io simulates nginx architecture, including master-workers model, ip/port/domain matching, location matching (besides the Nginx location directive semantics, you could define a function to do arbitrary matching), response filters chaining, signal controlling, and configuration file syntax.

And, the API is compatible with ngx_lua, including exec/redirect flow control, shared memory dictionary, DFA-pattern socket read, enhanced coroutine API, etc, so that luajit.io could reuse almost all lua-resty-* libraries directly (with some trivial naming changes).

See https://github.com/kingluo/luajit.io for details.

### Image Server Cluster Refactoring

*2015 UCWeb*

The image server is used to convert image formats, which is a CPU-bound program. The time cost per request ranges from 5ms to 5s, randomly.

In the first release, the front load balancer dispatches HTTP requests from app servers (clients) to the backend image servers. The Apache TrafficServer runs at the backend server: the master thread dispatches HTTP requests to the thread pool in a round-robin way. The number of the thread pool is the number of the CPU cores of the machine. The flaw of this design is that the requests are not dispatched among the CPU cores of machines evenly, then the average latency is high and unstable.

Think that at a given moment, some threads get many requests pending in the thread-specific queue, while the other threads are idle but those pending requests could not be migrated to them.

How to improve it? Well, each thread is available only when they finish the previous request, it should announce its availability to clients. So I use one Redis server, create a queue on it, and apply the producer-consumer model to dispatch the requests. When the thread is available, it pushes itself (ip/port info) to the queue. Each client would pop the queue to determine the handling thread. The pop operation is blocking the Redis queue, and the overhead is small. The HTTP protocol between the client and the server is replaced by a simple and clear protocol.

After refactoring, the overall performance of the cluster improves a lot!

### Diameter Route Agent

*2016 Elephanttalk*

Diameter Route Agent is like a fully functional IP router, but it's a diameter message dedicated router. The highlight of this project is you could use Lua script to extend the route logic:

* host validation logic
* message routing logic, e.g. round-robin, weighted load-balance
* message mangling just like what iptables does

### GSM SMSC

*2014-2015 Elephanttalk*

The SMSC is a message router. The message originator or terminator could be a mobile phone or SME (short message entity) via SMPP or HTTP. The most common usage is phone-to-phone messages. And the same provides vendor-specific service, For example, you could send a message from your phone to a short number to query information about your bank account. Then the bank could in turn send back the result message to the phone.

The SMSC sits between the GSM network and the Internet. For the GSM, it communicates with various network entities, e.g. MSC, and HLR to complete the MO and MT operations. Similarly, each service party (SME) connects to the SMSC via SMPP or http protocol.

PostgreSQL is used to store persistent data, e.g. SMS and processing records, And it also does the message delivery scheduling, handling retry rules, routing rules, and barring rules. The database would pg_notify the protocol adapter to do the delivery task. Table partitioning is used to archive history messages in a month. The logic is written in pl/pgsql.

### CDMA SSP

*2014-2015 Elephanttalk*

CDMA SSP is a service switching point, which connects to the SCP to apply pre-paid accounting, call forwarding, play announcement, Three-Way Calling, and other customized services upon the voice process.

The light spot of this project is that I embedded the luajit upon the low-level protocol stack so that we could implement the business logic in pure Lua. The system is then extensible at ease for any use case.

I implement the WIN protocol (CDMA intelligent network) in the SIGTRAN stack, with API exported via CORBA. We have a ton of test cases, while C language is not so productive. Lua is a good embedding language, simple but powerful. And the luajit's ffi could access the IDL-generated structures without conversion. Then I decided to wrap the whole SSP core with Lua API.

Just like ngx_lua, the CORBA C API (generated from the IDL) is asynchronous, but the Lua API is coroutine-based, synchronous, and nonblocking. The Lua API is high-level, which may involve multiple low-level CORBA operations invocations, for example, opening dialog, sending operations, handling operation callback, and closing dialog. The core handles all low-level stuff, e.g. the dialog management, the state machine, and the memory management.

### boost-reflection

*2012-2013 open-source project*

As known, C++ lacks reflection, which is important when you're building a large-scale framework, like Java Spring.

I bring Java reflection, Java annotations, and Java proxy objects into C++ land, in a non-intrusive way.

It supports all modern compilers, no generator or additional tools are needed.

See the GitHub page for source codes:

https://github.com/kingluo/boost-reflection

### Diameter stack

*2012-2013 Elephanttalk*

Diameter is an important AAA (Authenticate, Authorize, Accounting) protocol in telecom.

Implemented protocols layers:

* SCTP (kernel-based)
* diameter base
* DCCA

### SIGTRAN stack

*2008-2013 Elephanttalk*

SS7 over IP network.

It's the base of telecom networking, just like the role of TCP/IP stack on the Internet.

Implemented protocols layers:

* SCTP (kernel-based)
* M3UA
* SCCP
* TCAP
* MAP / CAP / WIN

### High-performance CORBA implementation

*2008-2013 Elephanttalk*

CORBA is a well-known RPC standard, but it has disadvantages:

* heavy weighted, e.g. object proxy, message marshaling
* complex async call extension
* transport (GIOP) is too strict and low-efficiency

This project is to design and develop a RPC framework based on a simplified and optimized CORBA version, used by components from our product lines.

It consists of:

* IDL compiler (compile IDL into C stubs and skeleton files), written in Perl and TCL
* configuration compiler (TCL)
* API library
* management tools
* launcher
* tracker daemon (process tracker, site clustering)

Features:

* supporting Linux and Windows
* application in the shared object library, loaded by the launcher
* configuration via tcl (the schema is defined in IDL, and you could retrieve them in C types at runtime)
* object failover and load-balance
* fully asynchronous call just like nodejs, as well as traditional synchronous call
* make full use of shared memory and Unix domain socket
* address resolution via in-shared-memory distributed db
* no message marshaling and unmarshaling
* lz4 compression for large message
* support CORBA_Any type in IDL (think about what Golang interface could do)
* failure awareness and callback
* managed memory access and recycle for call arguments

### Server Bootstrap CD

*2006 GuangDong Linux Center*

The Bootstrap CD is a special compact disc to guide the Administrators to install OS onto the bare server machine with ease. It boots the bare machine into a special in-memory Gentoo Linux, which provides an installation wizard that guides the user to configure the target OS (Redhat, Suse, Windows) installation, then it installs the OS automatically, and finally after reboot, the user get a full system with OS installed and integrated drivers for the particular machines.

The system is based on a dedicated MVC framework. The backend is written in Bash scripts following the configuration-driven model of 'portage' (the flexible package management system of Gentoo Linux distribution), while the front end is written in Java (tomcat). The backend and the front end are communicated through a .ini file, which is composed of fields used to synchronize the function logic of each other.

It makes use of many techniques, e.g. initrd, squashfs, unionfs, sysfs (devices scanning), OS installer hijacking (for Linux, it injects the drivers and post-installation scripts into the target initrd and installer, e.g. redhat Anaconda; for Windows, it constructs a special freedos image to launch auto-installation).

### Other open-source works

*2005-2008 GuangDong Linux Center*

* LVS

I submit a transparent proxy patch to the project author.

http://archive.linuxvirtualserver.org/html/lvs-users/2006-11/msg00261.html

https://archive.linuxvirtualserver.org/html/lvs-users/2007-01/msg00009.html

* DRBD

I submit some bugfix patches.

http://lists.linbit.com/pipermail/drbd-cvs/2006-February/000993.html

* U-Boot

I implemented the shell history extension and new hardware ports.

http://u-boot.10912.n7.nabble.com/My-3-patches-td2038.html

* ibox livecd

I created my own Linux distribution based on Gentoo at school:

http://distrowatch.com/table.php?distribution=ibox
