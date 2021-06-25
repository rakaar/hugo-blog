---
date: "2021-02-12T00:00:00Z"
subtitle: Also a bit about order reversal in measurement
tags:
- quantum
- qiskit
title: Measuring only m qubits out of n regiters in Qiskit
---

Suppose you have declared n registers in your circuit, but at the end you want to measure only m qubits. Here the same is done using an example, where out of 4 registers, you need to measure only 2 qubits.

```python
from qiskit import QuantumRegister, ClassicalRegister, QuantumCircuit, execute, Aer

q = QuantumRegister(4)
# we need only 2 classical bits as we are measuring only 2 qubits
c = ClassicalRegister(2)
qc = QuantumCircuit(q,c)

# affecting the 2 qubits, that I am planning to measure
qc.h(q[0])
qc.x(q[3])

# Measuring the first and last qubit only
qc.measure([0,3],[1,0])
job = execute(qc,Aer.get_backend('qasm_simulator'),shots=1000)
counts = job.result().get_counts(qc)
print(counts)
```

The above code results in 

```
{'01': 503, '11': 497}
```

In short, you need to provide the array of Qubits' indices that you want to measure in the first parameter of `qc.measure`. And also, don't make the mistake of declaring more classical registers than required.

#### Why [1,0] as second parameter instead of  simply putting  `c`  ?

In classical bits, counting starts from the right. If there are 2 bits, say `10` , then `0` is the 0th bit and `1` is the 1st bit. Now if we provide simply `c`  as the second parameter in `qc.measure`, by default `q[0]` is mapped to the least significant bit - 0th bit(the right most bit). And that would result in output as `{'10': 503, '11': 497}` because the right  most bit(`q[0]`) gives 0 and 1 with nearly equal probability.

This leads to confusion because we passed `[0,3] ` in the first parameter and while reading the output we expect the outcome of `q[0]` to be on the left most side rather than the right most side. To avoid this confusion, we map the outcome of q[0] to the most significant bit, which is the right most classical bit - 1st bit in this case. 

**NOTE**: Alternatively you could pass [3,0] as first parameter and c as second parameter in `qc.measure`, but I didn't like the idea of disturbing the order of qubits.



## Credits:

It was really frustrating for me, when I couldn't find how to do this on the google. Thanks to [Jack Woehr](https://github.com/jwoehr)  for helping me out with this thing in Qiskit slack.
