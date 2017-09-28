# Threat Model for Backups

This document attempts to list as many threats to a successful backup system as
possible. It does not contain any solutions or mitigations. Some of these
threats may be much more important to address than others; this document does
not attempt to rank the threats by importance.


## Software failure

Backup software has bugs. Libraries used by backup software have bugs. Bugs can
lead to deletion or corruption of existing backups, or (possibly silent) errors
in the creation of new backups.


## Hardware failure

Storage media can fail with or without warning. [TODO: How correlated are
storage media failures?] Storage media can fail subtly, e.g., returning corrupt
data. [TODO: Is it possible for storage media to claim a write was successful,
when it wasn't?] Computers can crash at any time, e.g., due to power failure.


## Process failure

People make mistakes. A person can swap the wrong drive, or mistype a command.

Anything that can't be automated, can be forgotten. Without proper monitoring,
anything automated can silently stop working.

Any process that's unnecessarily difficult, runs the risk of being ignored.


## Business/product failure

Businesses can fail with or without warning, or stop offering a product with or
without warning. E.g., data in cloud storage can dissappear, and backup
software can stop being supported.


## Death

Everybody dies eventually. Sometimes, people die unexpectedly. Passwords, and
information about where backups are stored, can die with a person.

[TODO: Do biometrics have any role in backups? If they do, figure out how death
and/or injury affect backups in that context.]


## Large-scale disaster

Large-scale disasters (e.g., weather events, power grid issues, electromagnetic
pulses, etc.) can cause correlated failures in otherwise independent systems.
[TODO: Describe the extent of correlated failures for different types of
disasters.]

[TODO: Figure out what threats warfare and climate change pose to backups. They
might be fundamentally different than other types of large-scale disasters.]


## Neglect

Over time, data formats become obsolete, storage technology changes, and
companies go out of business. The threats against backups will probably change
too. In the long term, regular maintenance and attention to changing
environments is probably necessary for a successful backup system.


## Denial of service

Ransomware might attempt to prevent access to backed up data, for profit. The
ransomware might have already compromised one or more authorized clients of the
backup system. [TODO: Describe threat posed by ransomware. E.g., do they
generally need to move quickly from initial compromise to ransom demand?]
[TODO: Any other threats in this category? Denial of service to do reputation
damage? Harassment?]

A client to a backup system might (accidentally or purposefully) disrupt the
backup system for itself and/or other clients, e.g., by attempting to back up
more data than the system can handle, or by congesting a network link.


## Tampering

Note: the below attacks could all be performed against data at rest, or data in
transit. Additionally, an attacker might have access to one or more authorized
clients of the backup system.

An attacker might attempt to tamper with backups, in order to compromise the
security of systems later restored from the backups. For example, backed up key
material could be replaced with material known to the attacker. Or, backed up
executables or configuration could be replaced, to open a back door.

An attacker might attempt to delete all copies of specific information,
including backed up copies. If the backup system includes automated pruning of
old backups, an attacker might attempt to exploit the pruning algorithm to
remove specific data. An attacker might attempt to fill in gaps using
authentic, but replayed data.

[TODO: Is there any threat of tampering to add specific data?]


## Extracting confidential data

An attacker might attempt to directly extract confidential information from
backups, or from data in transit to the backup system.

An attacker might attempt to extract confidential information by watching
changes to encrypted data over time. The attacker might be able to correlate
some ciphertext changes with passive observations of non-confidential
cleartext, or of ciphertext in transit between backup system components. The
attacker might also be able to inject chosen plaintext and watch the resulting
ciphertext changes. A particularly sophisticated attacker might be able to
watch changes to encrypted data during the backup system's algorithm or key
rollovers.

An attacker might attempt to extract (or read in transit) confidential key
material or configuration, in order to compromise an active system that still
uses that material or configuration. An attacker might attempt to extract key
material in order to decrypt other confidential information, possibly with a
large gap in time (in either direction) between extracting the key material and
acquiring the encrypted data.


## Backup system as an attack vector

An attacker might attempt to masquerade as the backup system to a client, in
order to receive confidential information from that client.

If the backup system has too much privilege on the systems it maintains backups
for, an attacker might attempt to use the backup system itself (or masquerade
as the backup system) to compromise those systems.
