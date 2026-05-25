# Google Cirq Quantum Simulations

This is a personal repository where I am learning how to use Google Quantum AI's **Cirq** library. 

### Why I switched to Cirq:
I originally started my quantum journey using IBM's Qiskit. However, while coding on my tablet using Google Colab, the IBM environments were lagging and getting stuck in server queues today. I decided to shift and try Google's native library instead because it runs more instantly in a cloud notebook environment.

### My First Cirq Script (`hello_qubit.py`):
```python
import cirq

# Pick a qubit on the grid
qubit = cirq.GridQubit(0, 0)

# Create a circuit that applies a square root of NOT gate and measures it
circuit = cirq.Circuit(
    cirq.X(qubit) ** 0.5, 
    cirq.measure(qubit, key='m')
)

print("Circuit Schematic:")
print(circuit)

# Simulate the circuit 20 times
simulator = cirq.Simulator()
result = simulator.run(circuit, repetitions=20)
print("Simulation Results:")
print(result)

## Update: Creating Quantum Entanglement (Bell State)
I wanted to push things further today so I built a 2-qubit circuit to test quantum entanglement using a Bell State. 

Here is the code I wrote and successfully ran:
```python
import cirq

q0 = cirq.GridQubit(0, 0)
q1 = cirq.GridQubit(0, 1)

qc = cirq.Circuit()

# Put q0 into superposition with a Hadamard gate
qc.append(cirq.H(q0))

# Entangle them using a CNOT gate
qc.append(cirq.CNOT(q0, q1))

qc.append(cirq.measure(q0, q1, key='bell_state'))

print("Circuit layout:")
print(qc)

sim = cirq.Simulator()
output = sim.run(qc, repetitions=20)

print("\n--- RESULTS ---")
print(output)
## Update: Simulating Quantum Teleportation
I wanted to try a classic quantum protocol, so I built a 3-qubit circuit to simulate quantum teleportation. The goal is to transfer the state of a message qubit over to Bob's qubit using a pre-shared entangled pair (Alice and Bob).

```python
import cirq

msg = cirq.GridQubit(0, 0)
alice = cirq.GridQubit(0, 1)
bob = cirq.GridQubit(0, 2)

circuit = cirq.Circuit()

# Initialize message state
circuit.append(cirq.X(msg)) 

# Entangle Alice & Bob
circuit.append(cirq.H(alice))
circuit.append(cirq.CNOT(alice, bob))

# Alice's operations
circuit.append(cirq.CNOT(msg, alice))
circuit.append(cirq.H(msg))
circuit.append(cirq.measure(msg, alice, key='alice_measure'))

# Bob's conditional corrections
circuit.append(cirq.CNOT(alice, bob))
circuit.append(cirq.CZ(msg, bob))

circuit.append(cirq.measure(bob, key='bob_result'))

sim = cirq.Simulator()
output = sim.run(circuit, repetitions=15)
print(output)

import cirq

# setting up 2 qubits for a basic bell state
# using simple line qubits instead of a grid layout for simplicity
q0 = cirq.LineQubit(0)
q1 = cirq.LineQubit(1)

# initializing the circuit
bell_circuit = cirq.Circuit()

# step 1: put q0 into superposition using a hadamard gate
bell_circuit.append(cirq.H(q0))

# step 2: entangle q0 and q1 using a CNOT gate
bell_circuit.append(cirq.CNOT(q0, q1))

# step 3: measure both qubits to check the correlation
bell_circuit.append(cirq.measure(q0, q1, key='result'))

# print circuit to console to make sure gates are in the right order
print("--- Circuit Diagram ---")
print(bell_circuit)

# running the local simulator matrix
sim = cirq.Simulator()
sim_data = sim.run(bell_circuit, repetitions=80) # 80 runs is plenty to see the distribution

print("\n--- Raw Counts Output ---")
# printing the raw histogram data
counts = sim_data.histogram(key='result')
print(counts)

print("\nQuick check: We should only see states 0 (00) and 3 (11) if they are perfectly entangled.")
