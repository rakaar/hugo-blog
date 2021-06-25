---
date: "2020-12-04T00:00:00Z"
mathjax: true
subtitle: Prerequisite - Basic understanding of Qubit
tags:
- quantum
title: Quantum Entanglement
---

## Introduction

Suppose, we are having 2 qubits, one in state $ \vert \alpha \rangle $ = $ a \vert 0 \rangle $  + $ b \vert 1 \rangle $ , and other in the state $ \vert \beta \rangle $ = $ c \vert 0 \rangle $  + $ d \vert 1 \rangle $ 

The combined state of them can be written as the tensor product $ \vert \alpha \rangle $ $ \otimes $  $ \vert \beta \rangle $ , which results in

$ \vert \alpha \rangle $ $ \otimes $  $ \vert \beta \rangle $ =  ($ a \vert 0 \rangle $  + $ b \vert 1 \rangle $ ) $ \otimes $  ($ c \vert 0 \rangle $  + $ c \vert 1 \rangle $ ) =  $ ac \vert 00 \rangle $ +  $ ad \vert 01 \rangle $ +  $ bc \vert 10 \rangle $ + $ bd \vert 11 \rangle$

If you are someone who likes to think in terms of vectors,then...

you can think $ \vert \alpha \rangle $   as $ \begin{bmatrix} a & b \\ \end{bmatrix}^{T} $  and $ \vert \beta \rangle $ as $ \begin{bmatrix} c& d \\ \end{bmatrix}^{T} $ in your head, then still, you might think it as tensor product as those 2 vectors, that result in a $ 4 X 1 $ vector $ \begin{bmatrix}ac & ad & bc & bd \\ \end{bmatrix}^{T} $. And the final state of 2 qubits can be thought of as $ \begin{bmatrix}ac & ad & bc & bd \\ \end{bmatrix}^{T} $   $\begin{bmatrix} \vert 00 \rangle & \vert 01 \rangle & \vert 10 \rangle & \vert 11 \rangle \\ \end{bmatrix} $

Now if you want to know the state of the system, you need to measure the first qubit and then the second qubit. Then it would collpase in one of the following - 00, 01, 10, 11.

What if I said there exists a system of 2 qubits, where after measuring the first qubit itself, you can tell whether the system is in 00, 01, 10 or 11. That means, just one measurement is enough to determine the state of the system. Such states are called entangled states, there are more interesting things about entanglement, but lets define the math first.

If it is not possible to write the state of a system as a tensor product of 2 states, then we can call them as entangled state. For example, 

$\dfrac{ \vert 00 \rangle + \vert 01 \rangle}{\sqrt{2}}$ can be written as $ \vert 0 \rangle $  $ \otimes $  $\dfrac{\vert 0 \rangle + \vert 1 \rangle}{\sqrt{2}}$ 

$\dfrac{\vert 10 \rangle + \vert 11 \rangle}{\sqrt{2}}$ can be written as $\vert 1 \rangle $  $ \otimes $   $\dfrac{\vert 0 \rangle + \vert 1 \rangle}{\sqrt{2}}$ 

Now what about 

$\dfrac{\vert 00 \rangle + \vert 11 \rangle}{\sqrt{2}}$ ?

Lets see  (a $ \vert 0 \rangle $  + b$ \vert 1 \rangle $ ) $ \otimes $  (c$ \vert 0 \rangle $  + d $ \vert 1 \rangle $ ) = ac $ \vert 00 \rangle$ + ad$ \vert 01 \rangle$ + bc$ \vert 10 \rangle$ + bd$ \vert 11 \rangle$  =  $\dfrac{ \vert 00 \rangle + \vert 11 \rangle}{\sqrt{2}}$

by comparing we get 

$ ac =  \dfrac{1}{\sqrt{2}} $,

$ bd =  \dfrac{1}{\sqrt{2}} $,

$ ad = 0 $,

$ bc = 0 $

if ad  = 0 then one of a or d has to be zero, 

lets see ,if $a$ is zero then $ac$ has to be zero, and can't be equal to $\dfrac{1}{\sqrt{2}}$, hence $a$ is not zero

and if $d = 0$, then bd has to be equal to zero, but $ bd $ was expected to be equal to $\dfrac{1}{\sqrt{2}}$, hence $d$ is also not zero

since its contradicting, no values of a,b,c,d exist. Hence $\dfrac{\vert 00 \rangle + \vert 11 \rangle}{\sqrt{2}}$ is an entangled state

Similarly,$\dfrac{\vert 00 \rangle + \vert 11 \rangle}{\sqrt{2}}$ , $\dfrac{\vert 01 \rangle + \vert 10 \rangle}{\sqrt{2}}$ , $\dfrac{\vert 01 \rangle + \vert 10 \rangle}{\sqrt{2}}$ are also entangled states.

Some extra info:-

- Infact they are given special names called Bell States.

- $\dfrac{\vert 00 \rangle + \vert 11 \rangle}{\sqrt{2}}$ is denoted by $ \vert + \rangle $, and $\dfrac{\vert 00 \rangle - \vert 11 \rangle}{\sqrt{2}}$ is denoted by $ \vert - \rangle $

## What's the big deal

So, that means if you measure the first qubit, you automatically know what the result of second qubit's measurement. Itseems as if the particles are comunicating...

"Hey, I collapsed to zero state, you collapse at zero too"

This "fundamental" property is exploited for communication using qubits. The interesting part is this doesn't depend on the distance between the qubits. For example, there is an entangled pair of $ \vert + \rangle $. Person "A" takes a qubit from the pair and goes to moon. And Person "B" is having other qubit of the pair with her on the earth. After A measures and finds it 0, then immediately after B measures she finds it 0 too! Is it just a hoax? No! Experimentally, it has been verified for a distance of around 500 kilometers [ref](https://www.livescience.com/28550-how-quantum-entanglement-works-infographic.html#:~:text=A%20proposed%20experiment%20would%20send,that%20has%20been%20experimentally%20tested.). 

Now, imagine the particles are far away from each other(very far, say the distance between 2 galaxies), we measure the second particle just after a split second, they does that mean that particles have communicated at a speed greater than the speed of light?, which states the theory of relativity is wrong.  Einstein help develop this paradox(called EPR paradox) to prove Quantum Mechanics as an incomplete theory. He called entanglement as "Spooky Action at a distance." Instead  EPR proposed a theory of hidden variables, which suggested that at inception of particles itself, their states were hidden. That means their states are pre-determined  and are hidden in the variables. Later came an Irish Physicist John Bell who proved the theory of hidden variables wrong. Checkout Bell's theorem for more.

## Preparing an entangled pair

Let's prepare an entangled pair - $ \vert + \rangle $. 

You have 2 qubits  both intitall set to $ \vert 0 \rangle $ . Hence their state would be $ \vert 0 \rangle $ $ \vert 0 \rangle $ .Apply Hadamard gate to the first qubit, then the state would be represented as 

  $H( \vert 0 \rangle)$ $ \otimes $  $ \vert 0 \rangle $  = $ \vert 0 \rangle $  + $\dfrac{\vert 0 \rangle +  \vert 1 \rangle}{\sqrt{2}}$ $ \otimes $  $ \vert 0 \rangle $  = $ \vert\dfrac{ \vert 00 \rangle +  \vert 10 \rangle}{\sqrt{2}}$

Now apply CNOT gate by considering first qubit as controlled bit and second one as target bit

(CNOT gate: if controlled bit is 0, target bit remains as it is. If control bit is 1, target bit is flipped)

That state now becomes $\dfrac{ \vert 00 \rangle +  \vert 11 \rangle}{\sqrt{2}}$

(*Explanation*:

$\dfrac{\vert **0**0 \rangle +  \vert **1**0 \rangle}{\sqrt{2}}$, here after CNOT as described above, the bit after **1** is flipped. To see it more clearly the state can be written as $ \vert 0 \rangle $ $ \vert 0 \rangle $  + $ \vert 1 \rangle $ $ \vert 0 \rangle $ 

In $ \vert 0 \rangle $ $ \vert 0 \rangle $ , control bit(first bit) is 0, hence target bit remains as it is

In $ \vert 1 \rangle $ $ \vert 0 \rangle $ , control bit(first bit) is 1, hence target bit is flipped, hence 0 changes to 1 and the it becomes $ \vert 1 \rangle $ $ \vert 1 \rangle $ ) 

For matrix fans!

$\dfrac{ \vert 00 \rangle +  \vert 10 \rangle}{\sqrt{2}}$ can be represented as $ \begin{bmatrix}1/\sqrt{2} & 0 & 1/\sqrt{2} & 0 \\ \end{bmatrix}^{T} $

The applying CNOT is equialent to multiplying the above state by this 4x4 matrix

![CNOT Matrix](/img/CNOT_matrix.png)
From that we get a 4x1 vector which represents the entangled state $ \begin{bmatrix}1/\sqrt{2} & 0 & 0 & 1/\sqrt{2} \\ \end{bmatrix}^{T} $, which is $\dfrac{ \vert 00 \rangle +  \vert 11 \rangle}{\sqrt{2}}$

## Code in Qiskit

```
from qiskit import QuantumRegister, ClassicalRegister, QuantumCircuit, execute, Aer

q = QuantumRegister(2)
c = ClassicalRegister(2)
qc = QuantumCircuit(q,c)

# initially be default, qubits are set to 0
# Applying Hadamard to first bit
qc.h(q[0])
# Applying CNOT gate where first bit is control bit, and second bit is target bit
qc.cx(q[0], q[1])

qc.measure(q,c)
job = execute(qc,Aer.get_backend('qasm_simulator'),shots=1000)
counts = job.result().get_counts(qc)
print(counts)
```

I know qiskit order is in reverse, but that is not important here. As the state is $\dfrac{ \vert 00 \rangle +  \vert 11 \rangle}{\sqrt{2}}$. By changing order, it will only add a diversion to the mind, why do you want to mess with the order when learning entanglement is already troubling you XD

The above results in scores like these, which is expected as $\dfrac{ \vert 00 \rangle +  \vert 11 \rangle}{\sqrt{2}}$ means we get $ \vert 00 \rangle$ with a probability of $(1/\sqrt{2})^2$ = $1/\sqrt{2}$, and  $ \vert 11 \rangle$ with a probability of $(1/\sqrt{2})^2$ = $1/\sqrt{2}$

```,
{'00': 504, '11': 496}
{'00': 499, '11': 501}
{'00': 508, '11': 492}
...
```

