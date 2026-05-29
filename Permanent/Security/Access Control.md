2026-05-25 22:48

Status:  #new

Tags: [[security]]

# Access Control

There is a big, big problem regarding, networked, multi-user and multi-tasking computers: allow each process, machine and user to do only the things they HAVE to do, nothing more, nothing less. Problems of this specie can be grouped by their scope:
- **File access control**: what files user, and processes (both local and remote) can access
- **Program access control**; what can be executed
- **Sharing access control**: what and how some data can be shared

> For instance, **cloud computing**e single-handedly exists thanks to virtualisation, the act of "splitting up" a single physical machine into standalone, independent logical machines. Core need for this setup is that VMs do not talk to each other, as they should barely (if not) know of the existence of sibling VMs.

Access control is everywhere, at any level of computing. From hardware, to low-level OS operations, to middleware and, last but not least, software applications. Many access control techniques are replicated in many of these levels. 

Let's dive right into OS-level access controls.

## OS Access Controls

Operating Systems typically limit what users and processes can read, write and execute. It is trivial to imagine an $n*m$ matrix for $n$ users and $m$ files that is populated with some kind of flags, marking the level of permission the user $i$ have on the file $j$.

### Groups and ACLs

This may work on personal computers, but in larger systems this won't cut, as more and more users and files are created the system becomes harder to manage and the lookup of this table become slower. Two mechanism have been created to mitigate this problem:
- **Groups and roles**
- **ACLs** 

Groups are a clever way to organise users by their level of access to files and programs, while role are is fixed set of access permissions given to one or more groups.

ACL (Access Control List) is a way of storing permissions for a resource. This technique is used by unix and unix-like operating systems. 
#### An example: Unix

On Unix and Unix-like operating systems, access control is a really straightforward mechanism, but this simplicity comes at the cost of a tedious access control management. 
For each file there is traditionally associated one user (the **owner**) and one group. Access levels (read, write and execute) are set for the owner, the group and everyone else not the owner or the associated group (also called **others**).

There is then the **supervisor**, with unrestricted access to every resource on the machine. Startup process runs as the supervisor, but system administrator may be granted this role is they have the credentials.

The Unix implementation of ACLs works for both users and processes: an executable may be run with the level of privileged of the user who started it, or it may have an attribute, called `suid`, that specifies a user id.

> [!warning]
> With this setup, it is really hard to obtain the set of resource a specific user or group have access to, as a complete scan of the file system is necessary.

### Capabilities

An alternative to ACLs are **capabilities**, a way of storing and using access control in a principal-oriented fashion, whereas ACLs are resource-oriented.
With capabilities, we can easily lookup every file a specific user/group has access to, and it's easy to delegate to other users our work. On the other hand, it is difficult to determine the amount level of access a resource have throughout the system and all its users/programs.
### DAC and MAC

**Discretionary access control** is a type of access control where the controls are discretionary, meaning that a superuser, or administrator, can easily change any user's access control, even give other users admin-level permissions. 
#### MACs

In some contexts, like in military-grade networks, you wouldn't give a user who has, say, access to top-secret documents the rights to give other users his same level of visibility to this documents. In this cases, it is **mandatory** that the operating system itself places some constraints as to how users of any level can manage access controls themselves. So systems that offer **mandatory access control (MAC)** are born. 

MACs are behave following some kind of **policy**, and those are configured by a remote administrator; here some examples:

> Many systems support for MAC and DAC access controls techniques, usually in different and non-overlapping contexts.
## Middleware Access Controls

Nowadays access control is needed on other levels of the tech stack, both above and below the OS Access control mechanisms described before. An example of a very common AC is done on the **middleware-level**, programs that communicate with the OS. 

 Like browsers and databases, as these programs grows in complexity and user base, the need for thorough access control mechanisms grows. These techniques are usually as complex as the OS ones, and sometimes are based on the host OS access control mechanisms.
  
## Virtualization and containers

> Virtualization is the core of cloud computing

**Virtualization** is the technique that allows a hardware machine to be *seen* and *used* like multiple machines, possibility concurrently by different users.
Virtualization is either implemented on the software level with a **hypervisor**, or on the hardware level. In every case, the real advantage of VMs is that those are cheap, possibly remunerative (e.g. with pay-as-you-use plans) and, last but not least, grant a theoretical absolute isolation from the host machine and other sibling VMs.

Still, communication with the host or other VMs is sometimes inevitable, and that's where access control needs to be really tight, otherwise unintended users or malware installed on a VM may gain control over the host machine or other sibling VMs.
#### Containters

**Containers** are a modern alternative to virtual machines. Their killer feature is that it is used only what's really necessary from an operating system, the other parts are provided by a kernel shared between all containers. This paradigms allows for faster boot times while keeping the security-related features of VMs, which make use of entire OS images.

Containers are deployed by orchestration tools like **Docker**, or **Kubernetes** on medium to high complexity systems that needs horizontal scalability and redundancy.

--- 

## OS-level access control models

**Covert channels** are a type of attack where the access control mechanism is, intentionally or not, bypassed, thus creating an data breach of some sorts, where unintended users have gained access to confidential data.

One simple example of a covert channel occurs when user or process with read access to sensitive files copy and pastes the content of this file to another file accessible to everyone. 

> [!Important]
> Covert channels are hard to eliminate. Still, they can be significantly reduced with a good system and access control mechanisms design.

Here we'll show one way the DoD implemented a multi-level security policy that tackles covert channels, and in general information leaking. 
### Bell-LaPadula model

The **Bell-LaPadula** model is a an access control model based on state-machine transitions based on a security policy where items and subjects are labeled with a **level of clearance**.

> The Bell-LaPadula focuses on confidentiality, granting that **no higher-level objects can be written by read by lower-level subjects** and that **higher-level objects cannot be copied into lower-level ones**.

The model defines three rules, two MACs and one DAC
- **Simple Security Property**: one subject with a given security level cannot read objects at a higher level of security level
- **Start Security Property**: one subject with a given security level cannot write into objects with lower security levels. This means that he can write to object with the same or higher clearance level.
- **Discretionary property**: an access matrix is defined to specify the DAC. 

In this model, covert channels can still be a problem. Think for example that one user may not have write access to an object because its security level is higher that his. Still, by invoking `ls` there user may still see the filename of the object, thus gaining potentially confidential information coded inside the filename.

#### Compartment extension

The Bell-LaPadula model can be extended with specifying a **compartment** for subjects and objects, so that access controls now also depend on the compartment of the item and of the user/process.

### Biba model

The **Biba Model** is an access control model that, unlike Bell-LaPadula, focuses on data integrity, and to confidentiality. Like Bell-LaPadula, the access control policy is described by a set of finite states on subjects and objects, with transitions only allowed when a policy is respected.

Subjects and objects are grouped by levels of integrity, with an additional extension with compartments. The policy of the Biba model is described by the following rules:

- **Simple Integrity property**: subjects can only write objects at the same level of integrity or lower.
- **Start integrity property**: subjects can only read objects of the same level of integrity or higher

> A real-life analogy of the Biba model is the chain of command in the army, where Generals write order to the colonels and below, while following commands  only given from the above, without being able to tamper them before forwarding to the colonels, thus respecting integrity.



 