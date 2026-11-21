# Isolation contract — unrestricted *inside* the VM

The user wants Hermes to explore freely. The correct implementation is **real root/administrator rights in a disposable virtual machine**, not pretending that host access exists.

## Separate resources
- **Host**: Ubuntu NucBox, existing Colibri CUDA inference on RTX 5060 Ti, Hermes gateway, and unrelated Frigate/Ollama/GPT-OSS services. Must remain untouched.
- **Guest**: a newly provisioned minimal Linux VM, dedicated user workspace, scratch storage, package manager, build tools, and a Hermes agent client. No real secrets or other user's repositories.
- **Code**: only `cbw29512/hermes-retro-lab` available to push to, via a fine-grained, repository-scoped credential. Do not expose a GitHub account-wide PAT.
- **Inference**: guest may reach only a narrow, optionally authenticated route to Colibri. The host service currently bound to 127.0.0.1 cannot be assumed reachable directly from the guest.
- **Networking**: deny guest→host management services, guest→LAN/private address ranges, and cloud metadata endpoints. If public Internet access is given for researching dependencies, use explicit egress policy; default libvirt NAT is *not* an adequate LAN isolation policy by itself.
- **Host resource limit**: VM must have CPU/RAM/storage ceilings, and Colibri must retain 64K model context. Verify available RAM, storage, hardware KVM, networking, and inference gateway before provisioning.

## Recovery
Take a clean VM snapshot; use separate nonprivileged workspace; log sessions; stop/restore if it behaves badly. No GPU passthrough: Colibri owns the GPU and guest calls the inference API.

## Not yet implemented
This document is a design contract, not proof of provisioned hypervisor rules or working GitHub isolation. Do not mark `environment.provisioned` true until the actual guest/network/credential controls pass tests.
