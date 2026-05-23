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