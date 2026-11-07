# Example Universal Turing Machine

A UTM created using Tuatara Turing Machine Simulator v2.1.

This UTM can simulate the execution of any Turing Machine encoded in Code Word Language (CWL) with tape alphabet $\\{a,b,\Delta,\\#\\}$ on any string over the language $\\{a,b\\}$.

To use the UTM, set up the tape using the following structure: $ZeZZPx\Delta\ldots$, where $e$ is an encoded Turing Machine, and $x$ is an input string.

The UTM will halt if and only if the encoded simulated Turing Machine halts.

## Design

Given the UTM is designed in Tuatara, it is subject to some restrictions:

- It can be slow to simulate the execution of large Turing Machines
- Individual parts of the UTM cannot be easily labelled or documented
- Although the wildcard matcher $?$ is available, I have tended to match individual letters for clarity
- Only one tape is availalble; it must use delimiters to separate sections of the input
  - Using different delimiters instead of just $Z$ would help minimise the size of the UTM, but would increase the size of the tape alphabet

Please open an [issue](https://github.com/jejebecarte/universal-tm/issues) if you discover any bugs.

### CWL

A Turing Machine encoded using CWL matches the regular expression: $(aa^∗baa^∗b(a\cup b)^5)^∗$. Each transition is concatenated together to create an expression for the full Turing Machine.

A clause for each transition is constructed as follows:

- origin state, in unary: $n\mapsto a^nb$
- target state, in unary: $n\mapsto a^nb$
- character to read
  - $a\mapsto aa$
  - $b\mapsto ab$
  - $\Delta\mapsto ba$
  - $\\#\mapsto bb$
- character to write
  - $a\mapsto aa$
  - $b\mapsto ab$
  - $\Delta\mapsto ba$
  - $\\#\mapsto bb$
- move direction
  - $L\mapsto a$
  - $R\mapsto b$

### Execution

Broadly speaking, the UTM uses the following algorithm:

1. Initialise an indicator for the current state, ensuring the space given to this indicator is large enough to fit the highest number state in the input.
2. If there is no transition for the current state that reads the letter currently at the tape head, halt.
3. Otherwise, write the specified character, and move in the specified direction.
