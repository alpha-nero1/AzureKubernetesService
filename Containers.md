# Containers
Traditional approach was:
- Each app runs on dedicated server.

Moved to VMs where multiple apps could then run on one server.

## Issues with VM
- Disruptions can happen when multiple apps run on the same VM
- High RAM and CPU utilisation of server.
- Startup is slow
- Requires license for each OS

## Containers
- Packages containing everything app needs to run
- Lightweight, standalone and portable.
- Wrapped in kernel groups and run on the kernel of the host.
- Container file must be compatible with host OS. If host is linux then container must be linux based.

- A VM might be the better option for an added layer of security. Because the containers share the host kernel, if the host is compromised it could compromise the containers as well.