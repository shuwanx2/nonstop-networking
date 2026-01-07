# Nonstop Networking — Implementation Summary

This repository contains my implementation of a concurrent file-sharing
client–server system in C using **non-blocking I/O** and **epoll**.
The goal of this project was to demonstrate low-level systems programming,
network protocol design, and scalable event-driven architecture.
---

## Files I Implemented / Modified

### Client
- **`client.c`**
  - Implemented a protocol-compliant client supporting `GET`, `PUT`, `LIST`, and `DELETE`
  - Handled partial `read()` / `write()` using looped I/O
  - Streamed large files without loading them fully into memory
  - Performed a socket half-close (`shutdown`) after sending requests
  - Parsed and validated server responses, including error handling

### Shared Utilities
- **`common.c` / `common.h`**
  - Implemented shared protocol helpers for request/response formatting
  - Added safe read/write wrappers to handle short reads and interrupted system calls
  - Centralized constants and helper functions used by both client and server

### Server
- **`server.c`**
  - Implemented a fully **event-driven, non-blocking server** using `epoll`
  - Managed multiple concurrent client connections without threads
  - Designed a per-connection state machine to track request progress
  - Implemented streaming file transfers using fixed-size buffers
  - Handled protocol errors, client disconnects, and malformed requests
  - Created and managed a temporary storage directory for uploaded files
  - Ensured graceful cleanup of resources on server shutdown (`SIGINT`)

---

## Key Functionality Implemented

- Custom **text-based network protocol** over TCP
- Support for:
  - `GET` — download files
  - `PUT` — upload files
  - `LIST` — list server-side files
  - `DELETE` — remove files from server
- **Non-blocking socket I/O** with `epoll`
- Per-connection finite state machine
- Robust error handling for:
  - Invalid requests
  - Incorrect file sizes
  - Missing files
  - Client disconnects
- Efficient handling of **large files** via chunked streaming
- No use of threads (`pthread`) — fully event-driven design

---

## Why This Project Matters

This project demonstrates my ability to:
- Work close to the OS and networking stack
- Design scalable systems without relying on threads
- Write robust, production-style C code
- Translate protocol specifications into correct implementations
- Debug and reason about complex I/O state and concurrency

The repository is intended to showcase **systems-level coding skill**, not just
assignment completion.
