# Lab Setup

This document records how the lab was built, at a level of detail that lets
someone reproduce the workflow without exposing any of my own infrastructure.
No IP addresses, hostnames tied to real systems, VPN keys, or credentials
appear here. Supply your own.

## Host

A small always-on mini-PC running Ubuntu Server is the foundation. Any machine
with a modern multi-core CPU and 16 GB of memory will comfortably run the
containerized stack below.

## Base hardening

Before installing anything, the host is brought to a sane baseline:

1. Apply all updates.
2. Create a non-root administrative user and disable direct root login.
3. Configure key-based SSH and disable password authentication.
4. Enable the host firewall, allowing only the services the lab needs.
5. Enable automatic security updates.

## Container runtime

Docker provides the runtime and Portainer provides a management interface:

1. Install Docker Engine from the official repository.
2. Add the administrative user to the docker group.
3. Deploy Portainer as a container for visual management of the stack.

## Wazuh

Wazuh is deployed as a single-node stack using the official Docker Compose
deployment. After the manager is running:

1. Access the Wazuh dashboard over the private network only.
2. Enroll agents on the lab endpoints (a Windows test VM and a Linux test VM).
3. Confirm agent telemetry is flowing into the manager.

## Microsoft Sentinel

The cloud side uses a Microsoft 365 lab tenant feeding a Sentinel workspace:

1. Create a Log Analytics workspace and enable Sentinel on it.
2. Connect the Microsoft Entra ID data connector to stream sign-in and audit
   logs.
3. Confirm `SigninLogs` is populating before building identity detections.

## Remote access

Access to the lab is handled over a private mesh VPN. Nothing in the lab is
published to the public internet. The dashboards and management interfaces are
reachable only from enrolled devices on the private network.

## Sanitization reminder

If you fork this lab, replace every value that identifies your own setup before
publishing. The point of documenting the lab is to demonstrate the workflow,
not to expose its attack surface.
