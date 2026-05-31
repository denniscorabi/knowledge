2026-05-29 23:29

Status:  #new

Tags: [[security]]
# Application security

Applications are everywhere. They are part of nearly every aspect of our lives, and so the desire for many people or groups of people to *exploit them*, to gain some form of advantage by using softwares in unintended way.

The **attacker** seeks **vulnerabilities** inside the code, and creates an exploits in the form of virures and worms to make one or many of these actions:
- Gain control over the host machine, or over the data used by the program
- Make the process crash, thus *denying a service* (aha!)

> Attacks are mitigated with the rollout of countermeasures, which are then deployed to production systems as **patches**, or even worse, **hot fixes**.

## Catalogs

It is fundamental that whenever vulnerabilities, attack schemes or weaknesses are found there is a catalogue where to remember them. 

> **OWASP** and **MITRE** are the most catalogues. 

In particular, OWASP keeps track on the 10 most important weaknesses, and for each of them there are guidelines as to how avoid introducing them in your code.
The MITRE catalog is way more broad, and any item is roughly part of one of these taxonomies:
- **Common Vulnerabilities Exposure** (CVE): known vulnerabilities
- **Common Weaknesses Enumeration** (CWE): known weaknesses
- **Common Attack Pattern Enumeration and Classification** (CAPEC): known attack schemes,  describes how weaknesses are actually exploited.
- **ATT&CK**: knowledge base of attack techniques

The most important one is of course the CVE. It is recommended that each software that uses some kind of external library is checked for potential vulnerabilities.

### CWE 707: improper neutralisation

One of the most common weaknesses found in code are the ones whose parent is the CWE 707 **improper neutralisation**. What does that even mean? Well, *improper neutralisation* occurs when an input passed by the user, usually a string, is not **sanitised**, meaning that potential malicious content is not filtered out.

> What makes the content of a string, or part of it, dangerous? that depends of the context.

- When the user input is used to invoke commands to execute OS commands, that is an **OS command injection** (CWE 78)
- When the user input is used (usually interpolated) into a constant string resulting in a query to a data structure (e.g. a database), that is a **SQL injection** (CWE 89)
- When the input is used to generate a server-side HTML page, then returned to the client, this is a **cross-site scripting** (CWE 79)
- ...And so on....

Let's dive deep into the problem of handling XSS attack schemes.

#### CWE: 79 Cross Site Scripting (XSS)

**Cross site scripting** is an type of command injection attack scheme where the attacker tries to inject malicious code (usually in form of javascript code embedded in the HTML) into the web page the user is visiting.

> Even thought the browser is relatively isolated from the host machine, many confidential data is stored in the form of cookies, or content in some DOM elements. **Key logging** is a serious threat.

There are three types of XSS attacks:
- Persistent XSS (or HTML injection)
- Reflected XSS
- DOM-based XSS

In a **persistent XSS** attack, the malicious code in stored inside a trusted data store, and then the same code is included into the dynamic content returned to the client.

> For example, if a username is of the type `user<script>...</script>`, when navigating to the user profile page that displays the username, the HTML returned to client executed the `script` part of the name.

In **reflected XSS**, the attacker creates for his website an URL containing malicious code; this URL is then shared to the victims, tricked to click to the link, and the dynamic code returned by the server contains the malicious code embedded into the string.

**DOM-based XSS** attacks leverage on the dynamic execution of script client-side. Like reflection attacks, the user is tricked into clicking of a URL containing malicious code. This code is NOT rendered on the dynamic content, instead the response contains javascript code that extracts the code from the URL and executes it.
#### CWE 89: SQL injection

**SQL injection** is another type of command injection attack scheme whose target is not the victim's browser, but a data store that supports SQL as a language for making queries and manipulating data.

As all the other CWE 77 attacks, the source of the malicious code is the same: user input. In this case the attacker may add malicious SQL code into an input whose value is then used in a query against the database. The resulting query also executes the SQL code embedded in the user input, impacting the integrity of the database.

One solution is to **sanitise the user input**, but this is a very hard task to do. So nowadays the recommended way to handle queries invoked by code is to use first or third-party libraries to connect and manipulate the database (e.g. EF CORE for .NET, Hibernate for Java)

---

### CWE 787: Out-of-bounds write

A **buffer** is a memory region whose purpose is to store a certain amount of data. Buffers are commonly abstracted in programming languages as arrays of a primitive type, like `byte[]`.
In some programming languages, like C, there is no check that buffer writes or reads do not exceed the buffer size, and attackers had found ways to exploit this behaviour.

Let's first dive deep into one of the most famous attack schemes: **buffer overflow**.
There are more specific weaknesses related to out-of-bounds writes, like:
- CWE 120: Buffer copy without checking input size
- CWE 121: Stack-based buffer overflow
- CWE 122: Heap-based buffer overflow
- CWE 124: Buffer underwrite

A **buffer overflow** occurs when memory regions adjacent to the buffer region is written, this replacing existing data, or **even executable code**.
Each process has an allocated region of memory (not necessarily contiguous), called the **address space**; its structure is known, and without the necessary countermeasures, one attacker may predict where the **return address** will be inside the stack frame, and will use buffer overflow to overwrite the return address with a reference to where malicious code (usually a shell-code) is located.

#### Countermeasures

Buffer overflow attacks were once the most popular types of attacks; nowadays they are less dangerous thanks to some countermeasures:
- **Canary values**
- **Address space layout randomisation**
- **Executable space protection**

**Canary values** are random instructions generated at compile-time. Their value is inserted into every new stack frame that is pushed on top of the stack segment; Canary values are then used as an integrity-check of the frame, and their value is compared before exiting the function. 

> If the values are the same, then the frame has not been tampered. Otherwise, an error message is displayed, and execution aborted.

**Address space layout randomisation (ASLR)** tackles the predictable nature of the address space layout by randomising locations of the process virtual memory, thus making it harder for an attacker to determine the address of the return address.

On the hardware side of things, by implementing **executable space protection** it is possible to distinguish between regions of memory containing data and regions containing code. 

> On the other hand, **just-in-time compilers** always need executable pages, so there are ways to circumvent the executable space protection.


>[!important]
>Higher-level languages like C#, Java and python have security measures against buffer overflow attacks. Still, native code that converts the program to executable code may be vulnerable to attacks.

### CWE 125: Out-of-bounds read

Now let's talk about the other main problem regarding the intentional (or not) misuse of buffers and unsafe programming languages: **buffer overread**.
A buffer overread occurs when the amount of content read from a buffer exceeds its size, thus creating a leak of information stored in adjacent regions of memory.

> A famous attack that uses buffer overread is **Heartbleed**.

---

### CWE 476: NULL pointer dereference

A pointer is a variable containing a memory address. A special pointer is the NULL pointer, whose points to address 0 inside the virtual address space of each process.

NULL pointer cannot be dereferenced, otherwise a segmentation fault is thrown. Still, it is a memory address like the others, so attackers had found ways to map the virtual address 0 to a real page inside the physical memory.

> No problems doing that, since operating systems use provide API to programmatically map virtual pages to the physical memory. In unix this command is `mmap`.

In older versions of `mmap` there was not check that the requested page to map was the one with address 0, so attackers could inject malicious code to the page references by the NULL pointer.
And if the NULL pointer is dereferenced when invoking a function, the malicious function injected by the attacker is executed.

What's even worse is that in a virtual memory setup is it really common for part of the kernel to be mapped inside the address space of each process, so as to reduce the overhead of invoking system calls. If the NULL pointer is dereferenced when calling a kernel function, the malicious code is executing in kernel mode, so without any restrictions.
#### Countermeasures

Newer versions of operating system kernels constraint the virtual memory pages that can be mapped to physical ones. Take per example Linux, where the `mmap` now takes a configuration parameter describing the minimum virtual address that can be mapped with the command. 

> By default this is 4096, so NULL pointer dereference attacks cannot use this weakness anymore. Still, there are ways to exploit the NULL pointer to gain control over the machine.
