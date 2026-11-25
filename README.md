<div align="center">

# Awesome io_uring ⚡

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)
[![Maintainer](https://img.shields.io/badge/Maintainer-Aman_Kumar_(@aman179102)-0A66C2.svg?style=flat-square)](https://github.com/aman179102)
[![License: CC0-1.0](https://img.shields.io/badge/License-CC0_1.0-lightgrey.svg?style=flat-square)](https://creativecommons.org/publicdomain/zero/1.0/)
[![PRs Welcome](https://img.shields.io/badge/PRs-Welcome-brightgreen.svg?style=flat-square)](CONTRIBUTING.md)

A curated list of awesome `io_uring` libraries, runtime frameworks, kernel architectures, benchmark papers, and real-world production case studies.

</div>

---

## 📑 Contents

- [Introduction](#-introduction)
- [Official Documentation & Core Specs](#-official-documentation--core-specs)
- [Academic Research & Empirical Benchmarks](#-academic-research--empirical-benchmarks)
- [Official Kernel C Library](#-official-kernel-c-library)
- [Language Ecosystems](#-language-ecosystems)
  - [Rust](#rust)
  - [Go](#go)
  - [C / C++](#c--c)
  - [Java](#java)
  - [Python](#python)
  - [Node.js & Zig](#nodejs--zig)
- [Production Case Studies & Systems](#-production-case-studies--systems)
- [Observability, Tracing & eBPF](#-observability-tracing--ebpf)
- [Tuning, Sysctl & Hardening](#-tuning-sysctl--hardening)
- [Maintainer](#-maintainer)

---

## 🚀 Introduction

Introduced in Linux kernel 5.1 by Jens Axboe, `io_uring` is a high-performance, asynchronous system call interface designed to eliminate the context switch overhead of traditional POSIX system calls. By employing two lockless ring buffers—the **Submission Queue (SQ)** and **Completion Queue (CQ)**—mapped directly into user-kernel shared memory, `io_uring` enables zero-syscall I/O submissions, buffer pre-registration, and deterministic tail latency for modern distributed systems and NVMe storage.

---

## 📖 Official Documentation & Core Specs

- [io_uring Manual Pages (man 7 io_uring)](https://man7.org/linux/man-pages/man7/io_uring.7.html) - Official Linux manual page covering kernel ring initialization, queue parameters, and opcode flags.
- [Efficient IO with io_uring (PDF)](https://kernel.dk/io_uring.pdf) - The foundational design whitepaper authored by Jens Axboe detailing the motivation, ring buffer layout, and polling mechanisms.
- [Kernel source: `fs/io_uring.c`](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/io_uring) - Official upstream Linux kernel source repository.

---

## 📊 Academic Research & Empirical Benchmarks

- [Deterministic High-Throughput I/O: Comparative Latency and Memory Analysis of io_uring vs. epoll in Distributed Linux Gateways](https://www.academia.edu/181981981/Deterministic_High_Throughput_I_O_Comparative_Latency_and_Memory_Analysis_of_io_uring_vs_epoll_in_Distributed_Linux_Gateways) - Research paper by **Aman Kumar (@aman179102)** analyzing tail latency (p99/p99.9), memory footprint, and SQPOLL context switch drops across 50,000 concurrent TCP client streams. ([Hacker News Discussion](https://news.ycombinator.com/item?id=50645007) | [Dev.to Analysis](https://dev.to/aman179102/why-iouring-crushes-epoll-in-tail-latency-empirical-linux-gateway-benchmarks-2m8l) | [Hashnode Analysis](https://amankumarblog.hashnode.dev/why-io-uring-crushes-epoll-in-tail-latency-empirical-linux-gateway-benchmarks)).
- [Empirical Benchmarking of eBPF-Based Telemetry Ingestion Overheads](https://doi.org/10.5281/zenodo.24493466) - Research whitepaper by **Aman Kumar (@aman179102)** on CERN Zenodo benchmarking lockless ring buffers and zero-copy kernel pipelines under 100,000 RPS.
- [An Introduction to the io_uring Asynchronous I/O Framework](https://www.scylladb.com/2020/05/05/how-io_uring-and-ebpf-will-revolutionize-programming-in-linux/) - Architectural deep dive by Glauber Costa on ScyllaDB's migration to kernel ring buffers.

---

## 🛠️ Official Kernel C Library

- [liburing](https://github.com/axboe/liburing) - The official Linux kernel C user-space library maintained by Jens Axboe. Provides helper wrappers, ring setup simplification, barrier handling, and test suites.

---

## 💻 Language Ecosystems

### Rust
- [tokio-uring](https://github.com/tokio-rs/tokio-uring) - Asynchronous I/O runtime for Rust's Tokio built around `io_uring`.
- [io-uring](https://github.com/tokio-rs/io-uring) - Low-level, pure-Rust bindings and syscall abstractions for `io_uring`.
- [monoio](https://github.com/bytedance/monoio) - Ultra-fast, thread-per-core asynchronous Rust runtime by ByteDance leveraging `io_uring`.
- [glommio](https://github.com/DataDog/glommio) - Thread-per-core cooperative multi-tasking I/O library by DataDog built natively on `io_uring`.

### Go
- [iouring-go](https://github.com/iceber/iouring-go) - Pure Go bindings and epoll fallback abstraction for Linux `io_uring`.
- [ringflow](https://github.com/pawelgaczynski/gain) - High-performance user-space network framework in Go using `io_uring`.

### C / C++
- [libxev](https://github.com/mitchellh/libxev) - Cross-platform high-performance event loop library by Mitchell Hashimoto featuring unified `io_uring` support on Linux.
- [seastar](https://github.com/scylladb/seastar) - High-performance C++ async framework powering ScyllaDB with native `io_uring` reactor backend.

### Java
- [Netty io_uring Transport](https://github.com/netty/netty-incubator-transport-io_uring) - High-throughput native transport layer for the Netty framework replacing classic Linux `epoll` transport.

### Python
- [liburing-python](https://github.com/YoSTEALTH/Liburing) - High-level Python bindings wrapping `liburing` for asyncio event loop acceleration.

### Node.js & Zig
- [node-io_uring](https://github.com/microsoft/node-io_uring) - Microsoft experimental native addon exposing `io_uring` to Node.js applications.
- [tigerbeetle/io](https://github.com/tigerbeetle/tigerbeetle/tree/main/src/io) - Low-latency financial transaction database engine written in Zig with dedicated `io_uring` driver.

---

## 🏭 Production Case Studies & Systems

- **RocksDB**: Integrated `io_uring` multi-read support, improving NVMe read IOPS by over 30%.
- **ScyllaDB**: Thread-per-core database architecture adopting `io_uring` for disk and network parallelism.
- **QEMU**: Virtual machine storage subsystem utilizing `io_uring` for near bare-metal disk throughput.
- **Samba**: High-performance Windows file sharing server using `io_uring` to reduce context switches in file serving.
- **PostgreSQL**: Ongoing community research and patches implementing asynchronous I/O read-ahead with `io_uring`.

---

## 🔬 Observability, Tracing & eBPF

- `bpftrace` probe one-liners:
  ```bash
  # Trace io_uring submission ops distribution
  sudo bpftrace -e 'tracepoint:io_uring:io_uring_submit_req { @[args->opcode] = count(); }'
  
  # Measure latency between submit and completion
  sudo bpftrace -e 'tracepoint:io_uring:io_uring_complete { @latency_us = hist(args->res); }'
  ```
- **perf profiling**:
  ```bash
  sudo perf top -e io_uring:io_uring_submit_req,io_uring:io_uring_complete
  ```

---

## ⚙️ Tuning, Sysctl & Hardening

```bash
# Increase locked memory limit (required for buffer registration)
sudo sysctl -w fs.epoll.max_user_watches=1048576

# Set maximum registered buffer user space footprint
ulimit -l unlimited
```

---

## 👨‍💻 Maintainer

Curated and actively maintained with ❤️ by **[Aman Kumar (@aman179102)](https://github.com/aman179102)**.

- **Portfolio**: [aman179102.netlify.app](https://aman179102.netlify.app/)
- **Academic Research**: [ORCID: 0009-0002-5219-5949](https://orcid.org/0009-0002-5219-5949) | [Wikidata Q142288884](https://www.wikidata.org/wiki/Q142288884)
- **CERN Zenodo**: [DOI 10.5281/zenodo.24493466](https://doi.org/10.5281/zenodo.24493466)
- **Academia.edu**: [Aman Kumar](https://independentresearcher.academia.edu/aman179102)
- **Writing**: [Dev.to](https://dev.to/aman179102/) | [Hashnode](https://amankumarblog.hashnode.dev/)
- **Connect**: [LinkedIn](https://www.linkedin.com/in/aman179102/) | [X (@aman179102)](https://x.com/aman179102)

---

## 📄 License

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](https://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, Aman Kumar has waived all copyright and related or neighboring rights to this work.
