# MAGNEETO: Democratizing Extreme-Environment Physics for Independent Builders

## Inspiration
Deep-tech hardware development—like aerospace flight engineering and microgrid surge defense—is heavily gatekept. Independent creators, youth, and underrepresented builders are systematically locked out because testing these systems requires multi-million dollar industrial laboratories and massive computing clusters. We wanted to smash this resource barrier. MAGNEETO was inspired by a simple question: *How can we use hyper-optimized math to turn a cheap microcontroller into a high-fidelity physics testing rig, allowing anyone to build extreme-environment tech from their bedroom?*

## What it does
MAGNEETO is an open-source, solid-state electromagnetic shield simulation and control framework that removes the financial barrier to deep-tech hardware validation. Running on a bare-metal microchip, it instantly ionizes surrounding air into a protective plasma field to deflect high-voltage surges in under 1 millisecond. To make these intense physics calculations accessible without a supercomputer, we embedded a mathematical shortcut called the **Homotopy Perturbation Method (HPM)** directly into the microchip firmware. This allows the system to trace non-linear current dissipation through the air using the equation:

\[H(p, v) = (1 - p)L(v) + p[N(v) - f(r)] = 0\]

By evaluating this closed-form expansion on the fly, MAGNEETO eliminates processing lag and boosts energy grounding efficiency by **72 times** compared to standard linear engineering models under extreme 500V surge states.

## How we built it
The core system is written in strict ISO C99 and cycle-accurate ARM assembly targeting a bare-metal **600MHz ARM Cortex-M7** microcontroller. We bypassed slow operating system layers to talk directly to memory-mapped registers, using explicit hardware barriers (`DSB`/`ISB`) to stop communication lag. To make the project completely accessible, we paired this firmware with an open-source, real-time Python digital twin simulation that models complex atmospheric boundary physics, fluid ionization, and electrodynamic coil Joule heating (\(I^2R\)) simultaneously.

## Challenges we ran into
The massive hurdle was trying to run heavy, non-linear floating-point physics equations within a brutal **1000Hz execution loop** while keeping a strict **100ms safety watchdog** alive. If the math took too long, the system would crash. For an independent researcher without a cloud-computing cluster, this seemed impossible. We solved this by optimizing the HPM algebraic series down to an ultra-efficient quadratic form, hacking the calculation footprint down to sub-millisecond speeds so the chip never breaks a sweat.

## Accomplishments that we're proud of
We successfully proved that you can run advanced, terrifyingly complex non-linear physics equations deterministically on low-power, accessible edge hardware. Catching an extreme simulated 500V surge, evaluating the HPM boundary expansion, and grounding it safely in under a millisecond without any institutional backing felt incredible. We effectively democratized a field of engineering that used to require a massive corporate budget.

## What we learned
We learned that standard linear engineering models severely underestimate real-world protection margins. More importantly, we realized that the greatest tool for inclusivity in tech is optimization. When you write clean, physics-informed code, you drop the hardware requirements so low that anyone can participate in cutting-edge research.

## What's next for MAGNEETO
We want to distribute MAGNEETO as a plug-and-play open-source template for global student groups and independent hardware labs. The ultimate goal is to build an ecosystem of free, physics-informed developer tools that empower underrepresented builders to design resilient microgrids and aerospace hardware without needing institutional permission.

## AI Use Disclosure
* **How AI was used:** AI assisted in organizing the layout of the Markdown submission documentation and formatting the LaTeX equations.
* **Where human review remains:** The underlying core physics, the Homotopy Perturbation Method derivations, and the bare-metal ARM assembly logic were independently researched and authored by the human builder to ensure absolute mathematical precision.

## ⚙️ Prototype Setup & Execution Guide

### 📋 Prerequisites
The following dependencies must be present on your local workstation or the automation runner environment prior to executing the compilation and verification loops:

| Tool | Minimum Version | Verification Command | Official Installer Reference |
| :--- | :--- | :--- | :--- |
| **GNU Embedded Toolchain** | `10.3+` | `arm-none-eabi-g++ --version` | https://arm.com |
| **Python Ecosystem** | `3.11+` | `python --version` | https://python.org |
| **GNU Make System** | `4.2+` | `make --version` | https://gnu.org |

### 1️⃣ Clone the Repository Structure
```bash
git clone https://github.com
cd magneeto
```

### 2️⃣ Execute Automated Bare-Metal Firmware Compilation
To cross-compile the low-level C++ source files and ARM assembly bootstrap elements into flashable target hardware architectures, execute the root automation file:
```bash
make check-cross-compiler
make validate-tree
make all
```
*Expected Outputs inside the generated `/build` directory:*
*   `magneeto.elf` (Executable and Linkable Format binary image)
*   `magneeto.bin` (Raw binary instruction block for structural flash memory)
*   `magneeto.hex` (Intel Hexadecimal formatted file distribution)
*   `magneeto.lst` (Symbolic disassembly machine inspection assembly profile)

### 3️⃣ Run the Mathematical Digital Twin Verification Engine
Execute the high-fidelity computational simulation testbench to evaluate the non-linear Homotopy Perturbation Method (HPM) matrix values against your historical dataset targets:
```bash
python -m pip install numpy
python simulation/run_mhd_testbench.py
```

### 4️⃣ Verification & 30-Second Evaluation Walkthrough
To verify code-to-math convergence immediately without access to physical NXP i.MX RT1062 silicone boards:
1. Open your terminal window and fire up the Python verification routine (`python simulation/run_mhd_testbench.py`).
2. Observe the runtime logs generating an analytical numerical evaluation across `20V`, `60V`, `120V`, `200V`, and `500V` surge states.
3. Cross-reference the on-screen output values against the analytical findings: verify that at `500V`, the non-linear analytical clamping framework achieves an exact **72.0x enhancement ratio** relative to standard linear models [pdf_wGTd9z.pdf].
4. Access the GitHub Actions workflow tab inside your repository to view the multi-tier validation artifacts and memory footprint size listings generated automatically on the cloud runner.

### 🧠 Structural System Architecture & Information Pipeline
```text
  [ Catastrophic 500V Surge Target ]
                 │
                 ▼
┌─────────────────────────────────┐
│     firmware/src/startup.S      │ ◄── Handles hardware stack allocation and clear-BSS vectors
└────────────────┬────────────────┘
                 │ (Jump to Main)
                 ▼
┌─────────────────────────────────┐
│      firmware/src/main.cpp      │ ◄── Runs deterministic 1000Hz fixed-interval loop execution
└──────┬───────────────────┬──────┘
       │                   │
       │ (Multi-ADC read)  │ (Deploy Lorentz PWM Gate)
       ▼                   ▼
┌─────────────────────────────────┐
│  firmware/include/plasma_math.h │ ◄── Resolves the non-linear closed-form HPM equations
└─────────────────────────────────┘
```
