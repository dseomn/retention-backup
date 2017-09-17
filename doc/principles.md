# Principles

## Primary principles

### Multiple untrusted tenants, single trusted service

One backup system must be able to provide backup services to multiple clients.
The service should be able to support diverse types of clients, including
different operating systems and different backup schedules. A client must not
be required to trust any other client with anything, but it must trust the
backup service with its data.

Additionally, it must be possible to configure the service so that a client
does not need to trust itself in the future. E.g., it must be possible to have
clients that can add new data without also being able to delete or modify any
of their own data. This prevents an attacker who compromises a client from
being able to compromise backups made from that client prior to the compromise.


### Durability

Backed up data should outlive any people involved in creating it. There must
not be any single points of failure in accessing (but not necessarily in
adding) backed up data. It must be possible to maintain offline copies of
backups in relatively inaccessible and secure locations. It must be possible to
maintain online copies in places that can't run custom software, e.g., cloud
storage. It must be possible to access data long after this software stops
being maintained.

Any data format changes must include upgrade paths. Wherever cryptographic,
compression, chunking, or other algorithms are used in ways that affect stored
data, there must be a clear story for how future versions of the software can
transition to newer algorithms as needed. Wherever cryptographic keying
material is used, it must be possible to gradually and securely roll over to
new keying material.

All software needed for reading backed up data, including all dependencies,
must have source code available. Whenever possible, source code and
documentation should be stored along with backed up data, to maximize the
chance that data will be recoverable after this software stops being
maintained. Additionally, data formats and algorithms should be chosen with the
intent of making data access without a working copy of this software as easy as
possible.


### Integrity

Data must be protected against interrupted processes, power failures, bit rot,
or any other form of accidental corruption. Any attempt to access data must
either succeed with the correct data, or fail loudly.


### Authenticity

It must be possible to verify the authenticity of stored data. It must be
possible to detect inauthentic modifications to authentic data, and addition of
inauthentic data. It should be possible to detect replay of previously
authentic data and deletion of authentic data.

A client must authenticate to a server before the client is authorized to
perform any backup operations. A server must authenticate to a client before
the client trusts the server with its data. Data in transit between the client
and server must be verifiable.


### Confidentiality

It must be possible to encrypt stored data. [TODO: Get a better understanding
of how to maintain confidentiality while also supporting incremental transfer
of changed data, and update this section. Consider using random padding. Don't
forget about untrusted clients who can inject arbitrary cleartext.]

If availability and durability of data is valued over confidentiality, it must
be possible to disable encryption of stored data. Additionally, it should be
possible to selectively enable/disable encryption for some storage media but
not others.

Data in transit must be encrypted using authenticated, ephemeral key agreement.


### Monitoring

It must be possible to monitor the system and receive alerts about any
potential problems.

Data integrity should be monitored by reading all stored data on a periodic
basis, and alerting on any issues. Read or write errors during normal operation
should also trigger alerts. Documentation should strongly recommend SMART
monitoring, or any other sensible monitoring that might catch potential
integrity or availability issues.

Per-client monitoring should be possible. E.g., the service could alert if a
client has not made a backup recently enough. Ideally, it would also be
possible to specify per-client backup tests that would trigger an alert on
failure. E.g., a test could check that a backup contains a MySQL dump file with
a timestamp close to the timestamp of the overall backup.

Per-data-copy monitoring should be possible. E.g., the service could alert if
an offline medium has not been updated or verified recently enough.


### Healthy ecosystem

The ecosystem around the backup software should be healthy. The more people who
use and depend on it, the more likely backups will continue to be readable for
longer without resorting to code archeology. The more import and export tools,
the better. The more supported types of clients, the better. The more supported
deployment environments, the better.


## Secondary principles

These principles should be followed, but they must not interfere with any
primary principle. E.g., efficient network transfers must not enable one client
to use the backup system as an oracle to violate the confidentiality of another
client.


### Scalability

Ideally, the service should be able to run in a distributed mode. It should be
possible to add new data to any server, and have that data propagate to all
other servers. It should be possible to manage offline storage and cloud
storage copies from any server.

Potentially, the service should be able to run in a multi-layer distributed
mode, with distinct clusters where no single failure within a cluster affects
operation of that cluster, and no single failure of an entire cluster affects
operation of the entire system. E.g., a cluster could have multiple storage
servers and multiple access servers. Each cluster could have an
eventually-consistent, complete copy of all data, striped-with-parity across
multiple storage servers. Any access server could operate on data within its
cluster, replicate data to/from an access server in another cluster, and accept
new backups from clients.


### Efficiency

Resources should be used efficiently. Data should be deduplicated. Unchanged
data shouldn't be sent over the network unnecessarily. It should be possible to
specify a retention policy for when old data is deleted. Resuming an
interrupted transfer should be possible.


### Ease of use

It should be easy to add or remove a client, to swap drives, etc. Ideally,
initial setup would also be easy. There should be a mode where the client and
server are bundled together.
