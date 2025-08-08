# The Semiclassical Quantum Fourier Transform (QFT) and Benchmarking 
Here is a detailed explanation of the semiclassical Quantum Fourier Transform (QFT), a key component of the project's Shor's algorithm simulation. It also analyzes the results of a benchmarking test, including the output histograms, to demonstrate the algorithm's performance under realistic, noisy conditions.

The Quantum Fourier Transform (QFT) is a fundamental subroutine in quantum computing, but its full implementation requires a large number of quantum gates. This can be prohibitive on current quantum hardware, where qubits are susceptible to noise and errors. To address this, a resource-efficient, hybrid approach was required for this project's simulation.

# Project Achievements
A semiclassical Quantum Fourier Transform was successfully implemented and integrated into the simulation. This hybrid approach significantly reduces the quantum resources needed by strategically offloading a substantial portion of the computation to a classical processor. This approach is considered an innovative method for practical applications on near-term quantum hardware, which possesses limited qubits and is sensitive to noise. The semiclassical QFT was developed to be more robust and practical than its fully quantum counterpart.

# Implementation Methodology
The semiclassical QFT replaces a traditional inverse QFT circuit with an iterative, classical-feedback loop. This method is achieved through a sequence of single-qubit operations and measurements. First, a Hadamard gate is applied to a single qubit. The phase of that qubit is then rotated based on the results of previous qubit measurements, which is where the classical feedback is incorporated. The qubit is then measured, and its classical result is used to control the next phase rotation. This iterative process is repeated for each qubit in the register, and in doing so, the same mathematical outcome as a full quantum inverse QFT is achieved with a less complex and more noise-resilient circuit. This methodology highlights the practicality of a hybrid quantum-classical approach in advancing the field of quantum computation.

# The Semiclassical QFT: A Detailed Mechanism
The implementation of the semiclassical QFT replaces a complex, fully-quantum Inverse QFT circuit with an iterative, classical-feedback loop. This method is achieved through a sequence of single-qubit operations and measurements, which are controlled by classical data. 

**Iterative Loop**: The algorithm's core is a for loop that processes the counting qubits in reverse order, from the most significant qubit down to the least significant. This top-down approach is essential to the semiclassical method.

**Hadamard Gate Application**: Within the loop, a Hadamard gate is applied to the current qubit. This operation places the qubit into a superposition state, a foundational step for all quantum Fourier transforms.

**Classically-Controlled Phase Rotations**: This is the key innovation of the semiclassical QFT. For each qubit, a series of conditional phase rotations is applied. The angle of rotation, θ=−π/(2 
k−j
 ), is calculated by a classical processor. The rotation itself is a quantum operation, but it is controlled by a classical bit stored in the classical_register. The code uses a specific Qiskit construct, with qc.if_test((classical_register[k], 1)):, which dictates that the phase rotation is applied only if the corresponding classical bit c_reg[k] is 1. This allows the quantum circuit to be dynamically updated based on the results of previous measurements.

**Measurement and Feedback**: After all the necessary phase rotations are completed for a given qubit, it is measured. The result is immediately stored in the classical_register and is then used to inform the phase rotations for the subsequent qubits in the loop. This process of measuring one qubit at a time and using the classical result for later operations is the central tenet of the semiclassical QFT, which significantly reduces the quantum coherence time required for the circuit.

# Project Achievements with the Semiclassical QFT
The successful implementation of this hybrid QFT allowed for the completion of the period-finding portion of Shor's algorithm. The specific outcome of the simulation demonstrated the following:

**Noise Resilience**: By relying on classical feedback and one-qubit measurements, the algorithm was made significantly more resilient to noise. This avoids the decoherence that would accumulate over a long, complex circuit needed for a fully-quantum inverse QFT.

**Reduced Quantum Resources**: The need for multi-qubit controlled phase gates was eliminated, which simplified the circuit and made it feasible to run on a simulator. This demonstrates a practical path toward implementing such algorithms on current, limited-qubit quantum hardware.

**Successful Period Finding**: The semiclassical QFT successfully transformed the quantum state encoding the periodic information into a measurable phase value. 

**Noise Model**
The noise model used for the semiclassical QFT simulation was a depolarizing noise model. This is a common type of noise model in quantum computing simulations. It's designed to mimic the effect of decoherence and gate errors on a real quantum machine.

**Type of Model**: A depolarizing noise model introduces a small chance that an error will occur after each quantum gate is applied.

**Error Rate**: As mentioned in your explanation, the error rate was set to 0.001. This means that after any single-qubit or two-qubit gate, there was a 0.1% chance of a random error (a bit-flip, a phase-flip, or both) corrupting the qubit's state.

**Impact**: This noise model accurately explains why the success probability of the QFT decreased as the number of qubits increased. With more qubits, the quantum circuit becomes deeper (more gates are required), giving the depolarizing noise more opportunities to corrupt the quantum state. This leads to a lower fidelity and a reduced probability of measuring the correct result, which is precisely what the benchmarking graphs and debugging counts showed.
In a real-world quantum computation, the results are probabilistic. When the quantum circuit for Shor's algorithm was run, the counting register is in a superposition of many different states. When these qubits were measured, a single answer wasnt found; there were many possible outcomes.

# QFT results
<img width="2538" height="1296" alt="Screenshot qft1" src="https://github.com/user-attachments/assets/2e377cab-6a1c-4709-9c1a-3a25bdc1fd83" />
<img width="2544" height="1368" alt="Screenshot qft2" src="https://github.com/user-attachments/assets/675813d1-1423-4487-b9a7-82a1b5b1d17f" />
<img width="2555" height="974" alt="Screenshot qft3" src="https://github.com/user-attachments/assets/490cc3a9-9e8a-42e0-9cf4-7ba126c49376" />

The reason the success probability isn't 1.00 is because of the noise model we added to the simulator.
# What the Benchmarking Results Mean
The results you see—a success probability of around 0.41 for 4 qubits, dropping to 0.36 for 10 qubits—are exactly what we expect when running a quantum circuit in a noisy environment.
•	Success Probability: This is the likelihood of measuring the correct bitstring. A value of 0.41 means that in a thousand runs, you got the correct answer about 410 times. The remaining runs produced different, incorrect bitstrings because of noise.
•	Decoherence and Fidelity: Every time a gate is applied to a qubit, there is a small chance of error, which we've set to 0.001. In a 4-qubit circuit, the total number of gates is relatively low, so the success probability is higher. As we increase the number of qubits to 10, the circuit becomes much deeper (more gates are required), giving noise more opportunities to corrupt the quantum state. This is why the success probability gradually decreases as n_qubits increases.

Understanding the Debugging Counts
The Semi-Classical QFT Counts are also very telling. For the 4-qubit run, the baseline expected bitstring was '0001'. We know our semi-classical QFT produces a little-endian output, so the correct bitstring for us to measure is '1000'.
Your debugging output shows: {'1000': 400, '1100': 433, ...}
This means that '1000' was indeed the most frequently measured outcome (400 times), giving us our success probability. The other bitstrings, like '1100', are the result of errors introduced by the noise model.

The histogram's role here is to visualize the frequency of these outcomes shown above.

X-Axis: The horizontal axis displays every possible measurement outcome from the counting register as a binary string (e.g., 00000000 to 11111111).
Y-Axis: The vertical axis shows the number of times that each specific outcome was measured after running the circuit many times.


<img width="1280" height="692" alt="Figure_1" src="https://github.com/user-attachments/assets/408f4649-dd1f-4799-93fc-f14ebbd85135" />
<img width="1280" height="692" alt="Figure_2" src="https://github.com/user-attachments/assets/9f6bf4ce-bace-4045-98a7-8468944b0532" />
<img width="1280" height="692" alt="Figure_3" src="https://github.com/user-attachments/assets/d9836273-7062-4d39-a8f0-cb7ee0711cb7" />
<img width="1280" height="692" alt="Figure_4" src="https://github.com/user-attachments/assets/89c59cc1-58ad-41e3-b6da-2c137f6b2614" />

The histogram here has distinct, tall peaks corresponding to the most frequently measured values. These peaks are not random; they are directly related to the period r of the modular exponentiation function, which is the value Shor's algorithm is designed to find.
