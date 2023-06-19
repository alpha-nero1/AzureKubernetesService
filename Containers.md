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
- Emulation achieved by container engine.
- Great fit for microservice architecture due to the small resource nature.

- A VM might be the better option for an added layer of security. Because the containers share the host kernel, if the host is compromised it could compromise the containers as well.

## Microservices architecture
- Each service is a seperate manageable unit.
- More complex, however, provides far greater scalability, flexibility and maintainability.
- Monolithic apps beacame too large and a redeployment is needed of the entire app even if a minor thing is changed this leads to increasing amounts of application down-time.