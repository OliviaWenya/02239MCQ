
### 1. **Types and Declarations**
In AnB, you need to declare all agents, keys, and other components of the protocol. Here are the basic elements:

#### **Agent Declarations**
- **Trusted Agents**: Declared with constants (lowercase letters).
  - Example: `s` (server or a trusted third party).
  - **Example Declaration**: `s: Agent;` – This declares `s` as a constant trusted agent (e.g., a trusted server).
- **Untrusted/Variable Agents**: Declared with variables (uppercase letters).
  - Example: `A`, `B` (represent general participants who could be honest or malicious).
  - **Example Declaration**: `A,B: Agent;` – These are general agents that can be instantiated as any participant (even the intruder).

### 2. **Keys**
#### **Symmetric Keys**
Symmetric keys are shared between two parties. They are declared using the type `Symmetric_key`.

- **Declaration**: `KAB: Symmetric_key;`
  - This declares `KAB` as a symmetric key that could be shared between two agents, say `A` and `B`.

#### **Asymmetric Keys (Public and Private Keys)**
Asymmetric cryptography involves public/private key pairs. In AnB, public keys are declared using `Public_key` and their corresponding private keys are denoted with the built-in function `inv(P)` (inverse of public key `P`).

- **Declaration**: `pk(A), pk(B): Public_key;`
  - This declares public keys for agents `A` and `B`.
- **Private Key**: The private key for agent `A` would be `inv(pk(A))`, which is automatically associated with the public key.
  - **Example**: `inv(pk(A))` – Represents `A`’s private key.

### 3. **Message Exchange Syntax**
Messages are exchanged between agents using the `A->B: message` notation.

- **Example (Sending a Symmetric Key)**:
  ```AnB
  A -> B: {|message|}KAB;
  ```
  - This represents `A` sending `message` encrypted with the symmetric key `KAB` to `B`.

- **Example (Sending an Asymmetric Key)**:
  ```AnB
  A -> B: {|message|}pk(B);
  ```
  - This represents `A` sending `message` encrypted with `B`'s public key.

### 4. **Common Functions**
- **Symmetric Encryption**: `{|M|}K` – Encrypt message `M` with symmetric key `K`.
- **Asymmetric Encryption**: `{M}pk(A)` – Encrypt message `M` with the public key of `A`.
- **Digital Signatures**: `{M}inv(pk(A))` – Sign message `M` with `A`’s private key.

### 5. **Capitalized vs Lowercase Letters**
- **Uppercase Letters**: Represent variables (can be instantiated by any agent, including the intruder). Examples: `A`, `B`.
- **Lowercase Letters**: Represent constants (trusted agents, like servers or a trusted third party). Example: `s` (a trusted server).

Example of Key exchange using NSSK:

Protocol: KeyEx

Types:
  Agent A,B,s;
  Number NA,NB;
  Symmetric_key KAB;
  Function sk,pre

Knowledge:
  A: A,B,s,sk(A,s),pre;
  B: A,B,s,sk(B,s),pre;
  s: A,B,s,sk(A,s),sk(B,s),pre
  where A!=B

Actions:

# B creates NB
B->A: A,B,NB
A->s: A,B,NA,NB
# s creates key KAB
s->A: {| KAB,A,B,NA |}sk(A,s), {| KAB,A,B,NB |}sk(B,s)
A->B: {| KAB,A,B,NB |}sk(B,s)
 
Goals: 

A authenticates s on KAB,B
B authenticates s on KAB,A

KAB secret between A,B,s

