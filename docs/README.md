# Kagan documentation

This folder holds the Kagan **design documents**: a roadmap and one design document per development phase, from the
toolchain to release 1.0 on x86 and the RISC-V and AArch64 ports of release 2.0. It also holds the setup guide and the
engineering records that the project keeps alongside the code. For the project overview, see the
[main README](../README.md).

## Contents

1. [Documents at a glance](#documents-at-a-glance)
2. [Design documents](#design-documents)
3. [Structure of a phase document](#structure-of-a-phase-document)
4. [References](#references)
5. [Engineering records](#engineering-records)
6. [Building the PDFs](#building-the-pdfs)
7. [Editing or adding a document](#editing-or-adding-a-document)

---

## Documents at a glance

| File | Contents |
|---|---|
| [`setup.md`](setup.md) | Development environment setup for Arch, Fedora, Ubuntu/Debian, macOS and Windows (WSL2): host packages, the verified cross compiler, editor, verification, and the toolchains for the ports (section 9) |
| [`journal.md`](journal.md) | The engineering journal: decisions, hypotheses and debugging notes, one entry per work session |
| [`assets/`](assets/) | Diagrams used by the main README (SVG, light and dark) |
| [`pdf/`](pdf/) | The design documents as PDFs (committed to Git) |
| [`00-roadmap.tex`](00-roadmap.tex) | Source of the roadmap |
| [`phases/`](phases/) | Sources of the nineteen phase documents |
| [`common/kagan.sty`](common/kagan.sty) | Shared LaTeX style: colours, title banner, boxes, tables, code listings, reference chips |
| [`Makefile`](Makefile), [`.latexmkrc`](.latexmkrc) | Build rules for the PDFs |

---

## Design documents

The roadmap is the entry point. Phases are implemented in order, and a phase starts only when every exit criterion of
the previous one holds. Phases 0–15 (Stages I–IV) produce release 1.0; Stage V (Phases 16–18) produces release 2.0.

| Document | Stage | Subject |
|---|---|---|
| [Roadmap](pdf/00-roadmap.pdf) · [source](00-roadmap.tex) | | Design goals, references, key technical decisions, phase map and summary, the 1.0 and 2.0 architectures, risks, exit criteria of every phase |
| [Phase 0](pdf/phase-00-foundations.pdf) · [source](phases/phase-00-foundations.tex) | I · Bare metal | Foundations and toolchain |
| [Phase 1](pdf/phase-01-assembly-and-boot.pdf) · [source](phases/phase-01-assembly-and-boot.tex) | I · Bare metal | x86 assembly and the boot process |
| [Phase 2](pdf/phase-02-output-drivers.pdf) · [source](phases/phase-02-output-drivers.tex) | I · Bare metal | Consoles and logging: screen, serial and `kprintf` |
| [Phase 3](pdf/phase-03-gdt-idt-exceptions.pdf) · [source](phases/phase-03-gdt-idt-exceptions.tex) | I · Bare metal | Segmentation, the IDT and CPU exceptions |
| [Phase 4](pdf/phase-04-hardware-interrupts.pdf) · [source](phases/phase-04-hardware-interrupts.tex) | I · Bare metal | Hardware interrupts: PIC, timer and keyboard |
| [Phase 5](pdf/phase-05-physical-memory.pdf) · [source](phases/phase-05-physical-memory.tex) | II · Memory | Physical memory management |
| [Phase 6](pdf/phase-06-paging-higher-half.pdf) · [source](phases/phase-06-paging-higher-half.tex) | II · Memory | Paging and the higher-half kernel |
| [Phase 7](pdf/phase-07-kernel-heap.pdf) · [source](phases/phase-07-kernel-heap.tex) | II · Memory | Kernel heap, core data structures and test infrastructure |
| [Phase 8](pdf/phase-08-processes-scheduling.pdf) · [source](phases/phase-08-processes-scheduling.tex) | III · Processes | Processes, context switching and scheduling |
| [Phase 9](pdf/phase-09-synchronization.pdf) · [source](phases/phase-09-synchronization.tex) | III · Processes | Synchronization and concurrency in the kernel |
| [Phase 10](pdf/phase-10-user-mode-syscalls.pdf) · [source](phases/phase-10-user-mode-syscalls.tex) | III · Processes | User mode, system calls and the protection boundary |
| [Phase 11](pdf/phase-11-process-api.pdf) · [source](phases/phase-11-process-api.tex) | III · Processes | Process API: `fork`, `exec`, `wait` and `exit` |
| [Phase 12](pdf/phase-12-storage-drivers.pdf) · [source](phases/phase-12-storage-drivers.tex) | IV · Persistence | Storage: block devices, ATA driver and buffer cache |
| [Phase 13](pdf/phase-13-file-systems.pdf) · [source](phases/phase-13-file-systems.tex) | IV · Persistence | File systems: VFS, initrd and KaganFS |
| [Phase 14](pdf/phase-14-userland-shell.pdf) · [source](phases/phase-14-userland-shell.tex) | IV · Persistence | Userland: libc, shell and utilities |
| [Phase 15](pdf/phase-15-hardening-release.pdf) · [source](phases/phase-15-hardening-release.tex) | IV · Persistence | Hardening and release 1.0 |
| [Phase 16](pdf/phase-16-portability-layer.pdf) · [source](phases/phase-16-portability-layer.tex) | V · Ports | Portability: an architecture layer |
| [Phase 17](pdf/phase-17-riscv-port.pdf) · [source](phases/phase-17-riscv-port.tex) | V · Ports | The RISC-V port |
| [Phase 18](pdf/phase-18-arm-port.pdf) · [source](phases/phase-18-arm-port.tex) | V · Ports | The AArch64 port and release 2.0 |

### What each phase covers

<details>
<summary><b>Stage I: Bare metal (Phases 0–4)</b>. Toolchain, boot path, consoles, exceptions and interrupts.</summary>

| Phase | Tasks |
|---|---|
| **0** Foundations | Install the host tools · make the editor's terminal see them · build the `i686-elf` cross compiler · create the repository skeleton · write and test the C warm-up library (`string`, `convert`, `bitmap`, `ringbuf`, `list.h`) · read compiler-generated assembly · smoke-test QEMU and GDB · start the engineering journal · threat-model stub |
| **1** Assembly and boot | Read compiler output · write assembly that C can call (`max`, `sum_array`) · a 512-byte boot sector that prints "Merhaba, Kagan!" · "Hello Cafebabe": a Multiboot kernel that puts `0xCAFEBABE` in `EAX` · boot through a real GRUB ISO · a Makefile (`all`, `run`, `iso`, `debug`, `clean`) · the first kernel debugging session |
| **2** Getting to C | Give the loader a stack and call C · compile C for the kernel · port I/O helpers · the VGA text driver · the serial driver · `kprintf`, log levels and the console layer · `panic` and `ASSERT` · read the Multiboot information |
| **3** GDT, IDT, exceptions | Describe the GDT in C · load it and reload the segment registers · build and load the IDT · write the interrupt stubs · the C exception handler · a handler registration API · trigger and verify every exception |
| **4** Hardware interrupts | Remap and mask the PIC · IRQ stubs and dispatch · the timer · the keyboard driver · the Turkish Q layout (optional) · line input and the kernel monitor · keep handlers short: top and bottom halves |

</details>

<details>
<summary><b>Stage II: Memory (Phases 5–7)</b>. Physical memory, paging, the higher half, the heap, tests and CI.</summary>

| Phase | Tasks |
|---|---|
| **5** Physical memory | Walk the memory map · know where the kernel ends · the bitmap allocator · a tiny test framework · test the allocator hard · a real `mem` command · a buddy allocator behind the same interface |
| **6** Paging | Warm-up: identity paging from C · a page-fault handler that explains itself · move the kernel to the higher half · fix every address that assumed physical = virtual · the VMM: mapping 4 KiB pages · pre-allocate the kernel's page tables · test the fault paths deliberately |
| **7** Kernel heap | Whole pages from the direct map · a heap region that grows · `kmalloc` and `kfree` · the rest of the API and a heap checker · an intrusive list · `make test` · stress the heap · slab caches under kmalloc |

</details>

<details>
<summary><b>Stage III: Processes and protection (Phases 8–11)</b>. Multitasking, locks, ring 3, system calls and the UNIX process model.</summary>

| Phase | Tasks |
|---|---|
| **8** Processes | The task structure · kernel stacks with guard pages · `switch_context` · creating a task · cooperative multitasking · preemption from the timer · sleeping, idling and exiting · `ps` and accounting · a fair scheduler (optional) |
| **9** Synchronization | Interrupt control that nests · spinlocks · wait queues without lost wake-ups · semaphores, mutexes and condition variables · a keyboard that blocks · the classic problems as tests · audit the kernel and write the lock order · reference counts that cannot wrap · check the lock order at run time |
| **10** User mode | Install a TSS · give every process an address space · the first jump into ring 3 · the system call gate · a user runtime · load programs as boot modules · the ELF loader · guard the door · write evil programs |
| **11** Process API | Processes, parents and the trap frame · frame reference counts · `fork`, the simple version · copy-on-write · `exit` and `waitpid` · `execve` with arguments · `brk` and a growing stack · minimal signals · `init` and the tests |

</details>

<details>
<summary><b>Stage IV: Persistence and userland (Phases 12–15)</b>. Disks, files, a C library, a shell, then hardening and the release.</summary>

| Phase | Tasks |
|---|---|
| **12** Storage | A disk for QEMU · find the drive with IDENTIFY · read and write sectors, polling · let the disk interrupt · a block device layer · partitions · a RAM disk · the buffer cache · tools and tests |
| **13** File systems | The VFS objects · descriptors and the basic calls · devices as files · a read-only root from a tar archive · path lookup · KaganFS, reading · KaganFS, writing · `stat`, `fstat` and `getdents` · tests |
| **14** Userland | Kagan libc: the skeleton · `malloc` in user space · `stdio` · pipes in the kernel · a real terminal · `kash`, the Kagan shell · core utilities · boot into the shell · test the userland |
| **15** Release | Fuzz the system call interface · audit the boundary · harden the kernel itself · users and permissions · benchmarks and one real optimisation · write the documentation · release v1.0 · write the design review · extensions after v1.0 |

</details>

<details>
<summary><b>Stage V: Ports (Phases 16–18)</b>. An architecture layer, then Kagan on RISC-V and AArch64: release 2.0.</summary>

| Phase | Tasks |
|---|---|
| **16** Portability | Two more cross compilers, two QEMU boards, dtc and a multi-architecture GDB · find every place x86 leaked · move x86 into its corner · the architecture interface · one trap frame for generic code · boot information without Multiboot · memory layout and the MMU behind an interface · consoles and input that any board can provide · make the generic code 64-bit clean · read a devicetree · one Makefile, three architectures · prove that nothing broke |
| **17** RISC-V | Meet the board · the first instructions · the 16550 again, memory-mapped · traps · time and interrupts · memory from the devicetree · Sv32 and the higher half · tasks and locks · into user mode and back · a disk on virtio · userland for RV32 · `make test` on RISC-V · optional: Kagan on Timur-RV32IMC |
| **18** ARM | Meet the board · the first instructions · a new UART: the PL011 · exception vectors · interrupts and time · the MMU, carefully · sixty-four bits everywhere · tasks and locks · into EL0 and back · virtio again, and the whole userland · Kagan 2.0 · optional: Raspberry Pi 4 |

</details>

---

## Structure of a phase document

Every phase document has the same sections in the same order.

| Section | Contents |
|---|---|
| **Title banner** | Phase number, title and a one-line statement of the phase's goal |
| **At a glance** | What the phase depends on and what it delivers |
| **Scope and motivation** | Where the phase fits and which problem it solves |
| **Background** | The concepts the implementation relies on, with diagrams; blue boxes hold key definitions |
| **References** | Chapters, sections and printed page numbers in the reference texts, weighted PRIMARY, SECONDARY or OPTIONAL, with their relevance |
| **Prior art** | How MINIX 3 implements the same subsystem, with pointers into its printed source, and how Linux does it. Stage V documents compare with xv6-riscv and Linux |
| **Implementation** | Numbered tasks (Task 3.4 is phase 3, task 4) with reference code. Each ends with a green **Verify** line, the acceptance check that closes the task |
| **Testing and debugging** | Failure modes as symptom → likely cause → resolution |
| **Exit criteria** | The conditions that close the phase |
| **Review questions** | Technical questions the design must be able to answer |
| **Extensions** | Optional work beyond the phase's scope |
| **References legend** | The reference codes at the end of every document; Stage V documents add the port references |

Coloured boxes used throughout:

| Box | Meaning |
|---|---|
| Concept (blue) | A key definition |
| Note (green) | Practical guidance or a shortcut |
| Caution (amber) | A common failure, or a command that can destroy data |
| Prior art | How MINIX 3 and Linux (in Stage V, xv6-riscv or Linux) solve the same problem |
| Decision (navy) | A design decision and its rationale |
| Exit criteria (red) | The conditions that close the phase |
| Review questions | Questions the design must answer |

---

## References

Each reference row has this shape:

| Weight | Source | Section | Pages | Relevance |
|---|---|---|---|---|
| PRIMARY | OSTEP | Ch. 1, Ch. 2 *Introduction to Operating Systems* | 1–22 | The three core abstractions the kernel implements |

- **PRIMARY**: the material the phase's design depends on.
- **SECONDARY**: a second treatment or useful depth.
- **OPTIONAL**: background.

| Code | Source | Role | Availability |
|---|---|---|---|
| **LOB** | Helin and Renberg, *The little book about OS development* (2015) | Practical x86 bring-up | Free at [littleosbook.github.io](https://littleosbook.github.io/) |
| **OSTEP** | Arpaci-Dusseau and Arpaci-Dusseau, *Operating Systems: Three Easy Pieces* (v0.90, 2015) | Primary theory reference | Free at [pages.cs.wisc.edu/~remzi/OSTEP](https://pages.cs.wisc.edu/~remzi/OSTEP/) |
| **OSC** | Silberschatz, Galvin and Gagne, *Operating System Concepts* (10th ed., Wiley, 2018) | Comprehensive reference | Wiley |
| **MOS** | Tanenbaum and Bos, *Modern Operating Systems* (4th ed., Pearson, 2015) | Hardware, I/O, security, OS design | Pearson |
| **OSDI** | Tanenbaum and Woodhull, *Operating Systems: Design and Implementation* (3rd ed., Pearson Prentice Hall, 2006) | MINIX 3 with its full source: the main prior-art comparison | Pearson Prentice Hall |
| **LKD** | Love, *Linux Kernel Development* (3rd ed., Addison-Wesley, 2010) | How Linux implements each subsystem | Addison-Wesley |
| **LKP** | Billimoria, *Linux Kernel Programming* (Packt, 2021) | Current Linux practice: logging, allocators, scheduler, locking | Packt |

The roadmap lists the ISBNs of the cited editions.

Stage V (Phases 16–18) also cites free references with chips of their own. Specifications are revised often, so
those rows give the chapter or section title and `--` in the Pages column.

| Code | Reference | Role | Availability |
|---|---|---|---|
| **XV6** | Cox, Kaashoek and Morris, *xv6: a simple, Unix-like teaching operating system*, RISC-V edition | A small UNIX kernel on RISC-V | Free at [pdos.csail.mit.edu/6.1810](https://pdos.csail.mit.edu/6.1810/), source at [github.com/mit-pdos/xv6-riscv](https://github.com/mit-pdos/xv6-riscv) |
| **RISCV** | *The RISC-V Instruction Set Manual*, Volumes I and II; the *RISC-V Supervisor Binary Interface Specification* | Instructions, CSRs, traps, Sv32 paging, SBI calls | Free at [riscv.org](https://riscv.org/specifications/ratified/) and [github.com/riscv-non-isa/riscv-sbi-doc](https://github.com/riscv-non-isa/riscv-sbi-doc) |
| **ARM** | *Arm Architecture Reference Manual for A-profile architecture* (DDI 0487); Arm's *Learn the architecture* guides | Exception levels, the MMU, the generic timer, the GIC | Free at [developer.arm.com/documentation](https://developer.arm.com/documentation) |
| **SPEC** | *Devicetree Specification*; OASIS *Virtio*; QEMU's documentation of the `virt` boards | The devicetree parser, the virtio-blk driver, the boards | Free at [devicetree.org](https://www.devicetree.org/specifications/), [docs.oasis-open.org/virtio](https://docs.oasis-open.org/virtio/virtio/), [qemu.org](https://www.qemu.org/docs/master/system/) |

Conventions:

- **Page numbers are printed page numbers** of the editions above, not PDF page indices.
- **With any other edition, use chapter and section.** Every reference row gives both. For OSTEP this is the normal
  case: each free chapter PDF starts at page 1, and the current version (1.10) is paginated differently from v0.90.
  Chapters 1–43, and every section cited, have the same numbers in both versions; only chapter 23 has a new title
  (*Complete Virtual Memory Systems*, formerly *The VAX/VMS Virtual Memory System*).
- **MINIX source references** such as `kernel/proc.c (07400)` give the file and its starting line number in OSDI
  Appendix B.

---

## Engineering records

These files live in `docs/` next to the design documents; each is created by the phase listed.

| File | Created in | Contents |
|---|---|---|
| [`journal.md`](journal.md) | Phase 0 | Engineering journal: date, what was tried, what happened, what was learned; a written hypothesis before every attempt on a long-running defect |
| `toolchain.lock` | Phase 0 | Versions of every tool the tree is built and tested with |
| `security/threat-model.md` | Phase 0 (stub), Phase 10 | Assets, adversaries, trust boundaries, out-of-scope threats and residual risks |
| `syscalls.md` | Phase 10 | System call reference: number, arguments, return value and errors |
| `security/syscalls.md` | Phase 10 | Validation contract of every system call argument |
| Benchmark results | Phase 15 | The benchmark table, with one optimisation measured before and after |
| Architecture document | Phase 15 | Overview, each subsystem, memory maps, system call table, lock order, KaganFS format, build/run/test/debug, known limitations |
| Design review | Phase 15 | Each Kagan subsystem compared with MINIX 3 and Linux; Phase 18 adds a porting chapter |
| `porting.md` | Phase 16 | The x86 leak audit (file, line, leak, fix) and notes on the architecture interface |

The lock order itself lives in code, in a comment block in `include/locks.h` (Phase 9).

---

## Building the PDFs

The PDFs in [`pdf/`](pdf/) are committed; rebuild them after changing a `.tex` file.

### Requirements

`latexmk` and a TeX Live installation with pdfLaTeX. The install command for each platform (Arch, Fedora, Ubuntu/Debian,
macOS, Windows through WSL2) is in [setup.md, section 8](setup.md#8-latex-for-the-design-documents-optional). The
simplest choices are:

| Platform | Command |
|---|---|
| Arch | `sudo pacman -S --needed texlive-basic texlive-latex texlive-latexrecommended texlive-latexextra texlive-fontsrecommended texlive-fontsextra texlive-pictures texlive-binextra` |
| Fedora | `sudo dnf install latexmk texlive-scheme-full` |
| Ubuntu / Debian / WSL2 | `sudo apt install latexmk texlive-latex-extra texlive-fonts-extra texlive-fonts-recommended texlive-pictures` |
| macOS | `brew install --cask mactex-no-gui` |

On Fedora you can also start from a smaller scheme and install LaTeX packages by the file they provide:

```sh
sudo dnf install latexmk texlive-scheme-basic \
    'tex(XCharter.sty)' 'tex(zi4.sty)' 'tex(fontawesome5.sty)' 'tex(tcolorbox.sty)' \
    'tex(xltabular.sty)' 'tex(titlesec.sty)' 'tex(needspace.sty)' 'tex(enumitem.sty)'
```

If a build stops with `File 'something.sty' not found`, install the package that provides it: `'tex(something.sty)'`
on Fedora, or search [packages.ubuntu.com](https://packages.ubuntu.com) by file name on Ubuntu and Debian.

The style loads: `geometry`, `fontenc`, `inputenc`, `XCharter`, `helvet`, `zi4` (Inconsolata), `microtype`, `parskip`,
`xcolor`, `graphicx`, `array`, `booktabs`, `tabularx`, `xltabular`, `multirow`, `float`, `enumitem`, `amsmath`,
`amssymb`, `tcolorbox` (with `most`), `listings`, `tikz`, `fontawesome5`, `titlesec`, `fancyhdr`, `etoolbox`,
`xspace`, `caption`, `needspace`, `hyperref` and `bookmark`.

> **VS Code as a Flatpak (Linux):** its integrated terminal runs in a sandbox that can't see host programs such as
> `latexmk`. Build from a normal terminal, or use the host terminal profile from
> [setup.md](setup.md#vs-code-installed-as-a-flatpak-linux).

### Commands

Run these inside `docs/` (or from the repository root with `make -C docs ...`):

| Command | Result |
|---|---|
| `make` | Builds the roadmap and every phase document into `pdf/` |
| `make pdf/phase-03-gdt-idt-exceptions.pdf` | Builds one document |
| `make clean` | Deletes `build/` (auxiliary files) |
| `make distclean` | Deletes `build/` and `pdf/` |

`make` only rebuilds a PDF when its `.tex` file or `common/kagan.sty` has changed. Auxiliary files (`.aux`, `.log`,
`.toc`, ...) go to `build/`, which Git ignores; the finished PDFs go to `pdf/`, which Git tracks.

Each document runs `latexmk -pdf -interaction=nonstopmode -halt-on-error -outdir=pdf -auxdir=build`. The
[`.latexmkrc`](.latexmkrc) adds `common/` to `TEXINPUTS`, so every document finds `kagan.sty` no matter which folder
its source is in.

---

## Editing or adding a document

A new phase document only needs to be saved as `phases/phase-NN-short-name.tex`; the Makefile picks up every `.tex`
file in `phases/` automatically. Start from this skeleton:

```latex
\documentclass[11pt,a4paper]{article}
\usepackage{kagan}

\begin{document}
% number, title, one-line goal, (unused), (unused)
\phaseheader{19}{Title}{One-line goal of the phase}{}{}

% (unused), (unused), dependencies, deliverables
\ataglance{}{}{Phases this one depends on}{What the phase delivers}

\section{Scope and motivation}
\section{Background}
\section{References}
\begin{readingplan}
\must & \OSTEP & Ch.~1 \emph{Title} & 1--10 & Relevance. \\
\end{readingplan}

\section{Implementation}
\task{First task}
Explain the step.
\begin{lst}{c}{kernel/example.c}
int example(void) { return 0; }
\end{lst}
\verify{How to check that it works.}

\section{Testing and debugging}
\begin{pitfalls}
Symptom & Likely cause & Resolution. \\
\end{pitfalls}

\section{Exit criteria}
\begin{milestonebox}[Name of the milestone]
\begin{checklist}
  \item First exit criterion.
\end{checklist}
\end{milestonebox}

\begin{questionbox}
\begin{enumerate}
  \item A review question.
\end{enumerate}
\end{questionbox}

\section{Extensions}
\booklegend
\end{document}
```

A Stage V document also ends with `\portlegend` after `\booklegend`.

The commands and environments `common/kagan.sty` provides:

| Name | Use |
|---|---|
| `\phaseheader{n}{title}{goal}{}{}` | Title banner (the last two arguments are unused) |
| `\ataglance{}{}{dependencies}{deliverables}` | The *at a glance* box (the first two arguments are unused) |
| `\task{title}`, `\verify{text}` | A numbered implementation task and its acceptance check |
| `\begin{lst}{language}{caption}` | A code listing with a file name or caption (`c`, `asm` for NASM, `rvasm` and `a64asm` for GNU as on RISC-V and AArch64, `make`, `sh`, `plain`) |
| `\code{text}` | Inline code |
| `readingplan`, `\must`, `\should`, `\could` | The references table and its PRIMARY / SECONDARY / OPTIONAL markers |
| `\LOB`, `\OSTEP`, `\OSC`, `\MOS`, `\OSDI`, `\LKD`, `\LKP` | Coloured reference chips |
| `\XVSIX`, `\RVSPEC`, `\ARMDOC`, `\SPEC` | Chips for the Stage V references (printed as XV6, RISCV, ARM, SPEC) |
| `pitfalls` | The symptom / cause / resolution table |
| `conceptbox{title}`, `tipbox[title]`, `warnbox[title]`, `decisionbox{title}`, `minixbox[title]` | Concept, note, caution, decision and prior-art boxes |
| `milestonebox[title]`, `checklist`, `questionbox` | Exit criteria box, tick-box list and review questions |
| `\tkth{text}` | A table header cell in the house style |
| `\nextphase{n}{title}`, `\booklegend`, `\portlegend` | The "next phase" link, the references legend at the end, and the Stage V reference legend |

Colours are defined once in the style: `tkNavy`, `tkRed`, `tkBlue`, `tkGreen`, `tkAmber`, `tkGray`, `tkViolet` (the
Stage V colour), the backgrounds `tkLight` and `tkCode`, one colour per reference text (`bkLOB`, `bkOSTEP`, `bkOSC`, `bkMOS`,
`bkOSDI`, `bkLKD`, `bkLKP`) and one per Stage V reference (`bkXV`, `bkRV`, `bkARM`, `bkSPEC`). Use these names instead of new colours
so every document looks the same.

Two LaTeX traps to avoid: `%` inside `\code{...}` starts a comment when the code sits in the
argument of another command such as `\verify`, so write `\texttt{\%lx}` there; and a TikZ style must not be called
`x` or `y`, which are built-in TikZ keys.
