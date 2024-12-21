# Title

A minimal Rust kernel for the x86_64 architecture. The design goal for this project is to create a universal microkernel that can load ELF64 programs as kernel modules.

The core microkernel provides only the most essential services - hardware abstraction, memory management, and basic scheduling. All other functionality would be implemented as loadable ELF64 modules that interact with the kernel through a dynamic library interface.

## Design Objectives

This architecture could provide several advantages over monolithic kernels, especially for rapid prototyping in embedded systems projects. For instance, when developing new features or testing different implementations, users can hot-swap modules without rebuilding and restarting the entire kernel thanks to the dynamic library interface. This design allows for selective loading of only the features needed for a particular embedded application, while the underlying microkernel can be shared across projects.

Fault tolerance is another major focus of this design. Since only the essential services run in the kernel space, a failing module is less likely to bring down the entire system because all processes run in the address space. Consequently, features like process migration and load balancing can be handled very effectively through the existing message channels--like moving a docker container between hosts--because all the dependencies and state are contained within.

Security benefits of isolated processes are well documented elsewhere, but in essence processes have limited priveliges and can only access resources through well-defined kernel interfaces.

The trade-off with this design is performance. All communication between processes requires message passing through the kernel, which introduces some overhead.

POSIX compliance will be handled by a compatibility layer which translates POSIX syscalls into the microkernel's message-passing mechanism.

This project takes inspiration from L4, QNX, Microsoft Singularity, and the sisyphos-kernel project (https://github.com/le-jzr/sisyphos-kernel-uefi-x86_64.git).

## Build

```
cargo build
```

## Usage

```
Run in QEMU or other VM.
```

## TODO

### Core Components
- [ ] Hardware Abstraction Layer
- [ ] Module Loading System
- [ ] POSIX compatibility layer
- [ ] Memory Management (Stack and Heap implementation)
- [ ] Basic Scheduler & Process Management
- [ ] Process migration manager
- [ ] Load Balancer

### IPC and Messaging System
#### Message passing infrastructure
- [ ] define message format and protocols (hypercore?)
- [ ] implement message queues
- [ ] set up message routing between processes
#### ELF Loading System
- [ ] ELF parser for x64
- [ ] implement dynamic linking support
- [ ] set up module loading infrastructure
- [ ] define kernel module API

### System Services
#### Process Server
- [ ] Implement process creation/destruction
- [ ] Add process scheduling
- [ ] Create process migration capabilities
- [ ] Set up load balancing infrastructure

#### Memory Server
- [ ] Implement shared memory management
- [ ] Create memory mapping service
- [ ] Add virtual memory management for user processes
- [ ] Implement memory protection mechanisms

#### Device Management
- [ ] Create device driver framework
- [ ] Implement basic device drivers
- [ ] Set up device discovery system
- [ ] Create device access controls

### POSIX Compatibility Layer
#### Signal System
- [ ] Implement signal delivery mechanism
- [ ] Create signal handler management
- [ ] Set up signal permissions
- [ ] Add signal queuing system

#### File System Interface
- [ ] Create VFS abstraction
- [ ] Implement basic file operations
- [ ] Add file descriptor management
- [ ] Set up file permissions system

#### Process Control
- [ ] Implement fork() and exec()
- [ ] Add process group management
- [ ] Create session management
- [ ] Implement wait() and exit() handling

## Contributing

PRs not accepted at this time. This is for your own good.

## License

MIT © Kelly Gibson
