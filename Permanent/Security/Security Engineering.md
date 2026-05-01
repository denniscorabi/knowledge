2026-04-22 21:01

Status:  #new

Tags: [[security]] 

# Security engineering

From *Security Engineering* by *Ross Anderson*

Security engineering is all about making a system that is always reliable. It requires cross-disciplinary expertise, ranging from technology, applied math, psychology, economics and law.
It is also fundamental that the security engineer knows how a malicious user behaves and acts to know how to efficiently counter him.

Safety and security are becoming more and more important traits of good software as we get software for everything in our lives. Inevitably, there is software whose resilience to failures and malicious attempts is necessary to protect humans and the environment alike.

### A dependable system

To build dependable systems, four characteristics are required:
1. **Policy**: what you are supposed to achieve
2. **Mechanism**: an implementation for a policy (software following requirements)
3. **Assurance**: amount of reliance you can put on each mechanism
4. **Incentive**: motive system maintainers have to do to do their job correctly

Whilst every one of these traits is equally important, there is usually a bias to make decisions based on the *public* side of things, meaning decisions that revolve around making the system *feel* secure, rather that *being* secure.

> [!information]
> This is what Ross Anderson jokingly defines as **security theatre**.

A reliable software is built on the expertise of the security engineers involved in the project, where they have to know how it will work and, most importantly, how a previous version of it or similar other software have failed in the past.

### Vulnerability: a definition

A **vulnerability** is a malfunction of the system (i.e. unexpected behaviour that system designers didn't think about) which, in conjunction with an internal or external **threat** can lead to **security failure**

A **critical system** is one whose failure could lead to an accident that may involve individuals. It is then really important that in those systems the **hazards** and **dangers** are detected and mitigated.

A **safety policy** describes the risks regarding the system under scrutiny and illustrate a way to keep them below a certain, acceptable threshold.

