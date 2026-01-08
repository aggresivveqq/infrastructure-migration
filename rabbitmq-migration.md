## Pre-Migration Preparation

Before starting the migration, the target servers were prepared
to ensure compatibility, performance, and operational safety.

All data are generelized and showing scaling of real numbers.

The preparation included:

- Base OS installation and security patching
- System-level tuning aligned with the target workload
- Disk layout and filesystem configuration suitable for data persistence
- Network access and firewall rules required for cluster communication
- Creation of required system users and permissions

Environment-specific values, hostnames, and credentials are
intentionally omitted or generalized.
## Pre-migration context

## Task Purpose

Migration of the RabbitMQ message broker to a new environment,
including a version upgrade as part of the migration process.

---

## Source System

- **Cluster topology:** 2-node RabbitMQ cluster
- **Nodes:**
  - `rabbitmq-server-1` (primary node)
  - `rabbitmq-server-2` (secondary node)
- **Node resolution:** static entries in `/etc/hosts`
- **Queue type:** mirrored queues
- **Total connections:** 14  
  - 7 connections on the primary node  
  - 7 connections on the secondary node  

### Platform Details
- **Operating system:** CentOS 8 (RHEL-based)
- **Filesystem:** EXT4
- **RabbitMQ version:** 3.8.35
- **Erlang/OTP version:** 22.x
- **Date of assessment:** 26.12.2025


## Target System

- **Cluster topology:** 2-node RabbitMQ cluster(planned)
- **Nodes:**
  - `rabbitmq-newserver-1`
  - `rabbitmq-newserver-2`

### Platform Details
- **Operating system:** Ubuntu 24.04 LTS
- **Filesystem:** EXT4

The target environment was provisioned as a clean system,
allowing proper cluster configuration and version alignment.

---

## Pre-Migration Assessment

At the time of migration, the RabbitMQ version was officially **End of Support (EOS)**.
Despite this, the broker was operating in a stable and consistent manner.

During the investigation, the following observations were made:

- Each node maintained approximately **4–5 passive connections**
- Only **2 connections per node** were actively used on a continuous basis
- Average message throughput was approximately **46 messages per second**
- Peak throughput reached up to **70 messages per second**
- No message read or write errors were observed
- All messages were delivered successfully

The system integrated with an **external identity provider** for user authentication.

---

## Identified Issues and Risks

- The RabbitMQ cluster was **incorrectly configured**:
  - Mnesia operations and RabbitMQ CLI commands functioned only when a
    **direct node name** was explicitly specified
- **Mnesia backup was outdated** (7 months old)
- Backup state was considered **non-reliable** for recovery purposes
- Use of an **EOS RabbitMQ version** represented an operational and security risk

### Enabled Plugins
- RabbitMQ Management Plugin

---



