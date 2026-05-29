2026-05-02 22:03

Status:  #new

Tags: [[security]]
# Authentication schemes

**Authentication protocol** are a core topic in security engineering. They are used by principals to authenticate themselves, and usually they involve some sort of [[Cryptography]] technique.

### First of all: passwords

The most famous example of an authentication mechanism is the **password**, a string of text, usually constrained to have numbers, special characters, etc., that is known only the user who created it in the first place.  

Whilst passwords are appropriate in most of real-analog-life scenarios, this cannot be said in the realm of internet, where passwords have become less and less reliable, independently of their strength.

A problem regarding password are **rainbow tables**, pre-computed password-hash dictionaries used by an attacked during a brute force or dictionary attack. Rainbow tables are especially useful to gain access to user credentials whose password is common or very weak.
To mitigate this problem, **salt** is used: this is a random sequence of character used internally by the server to store passwords and to make the authenticating process more robust.

Salts are appended to the principal password, and then the hash is computed. With this technique, even weak passwords are really hard for the attackers to determine.

Let's now dive into a the broader category of authentication schemes called that go by the name of **challenge response**.

## Challenge and response

**Challenge-response** authentication is an authentication scheme where one party presents a question (a challenge), of which the party that wants to be authenticated must present a response.

> Password authentication is a simple example of a challenge-response authentication. The user is presented with a blank input entry that needs to be filled.

So, the principal must provide a 'fit' response. The correctness of the response is evaluated with an algorithm known to both the principal and the entity that provided the challenge.

A hard requirement of challenge-response schemes is that, say, "Bob" issues a different challenge to "Alice" each time, so that previous correct responses cannot be used by an attacker to determine the response for the current challenge.

### Insecure channel problem

On the internet, we cannot be sure that the protected system is really the one the principal wants to access. So there is now the need for both parties to verify they have knowledge of a shared secret, without it ever being transmitted in the clear (so to avoid **eavesdropping**).

One way this is done involves using the shared password as the encryption key to transmit some random message as the challenge, whereupon the other end must return an encrypted response (one the receiver is able to decrypt), thus proving that it was able to decrypt the challenge and to produce a proper response.

> The random message guards against the possibility of a **replay attack**, where a malicious user records the exchanged data and retransmits at a later time to trick the other end that a new connection has been established.

The uniqueness of each challenge-response is granted by a **nonce** (*number only used once*). The nonce can be created with a pseudorandom number generator or a hash function.

> It is important to NOT use time-based nonces when the two ends of the authentication process are located in different timezones.

### Reflection attack

A **reflection attack** is a method of attacking challenge-response authentication algorithms that use the same protocol in both directions in a mutual authentication sequence.

The idea is to trick the target into providing a response to its own challenge. The attack outline is as follows:
- The attacker initiates a connection to a target
- To authenticate the attacker, the target send a challenge
- The attacker opens another connection to the target. To authenticate the target, the attacker sends the same challenge
- The target responds to the challenge and sends it back to the attacker
- On the first connection, the attacker responds to the target's challenge with the response obtained in the second connection. If the authentication protocol is not carefully designed, the target will treat the response as a successful one, thus authenticating the malicious user.

A simple solution is to use MACs, calculated with the use of an identifier and a nonce, encrypted using the shared secret key.
With this solution, the other end can make sure the message has been sent from the same principal (using the identifier) and that is not a message already sent in the past (using the nonce).
