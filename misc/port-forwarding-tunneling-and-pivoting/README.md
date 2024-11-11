---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Port Forwarding, Tunneling and Pivoting

## **Tunneling**

Transferring data using a certain protocol by encapsulating it in another protocol. This can be used for encrypted transmission of information.&#x20;

For instance, if we want to communicate with a machine we reached without the firewall dropping the packet, we can obscure it by creating a tunnel using a protocol that passes through the firewall’s filtering.

## **Port Forwarding**

A method that allows us, as attackers, to transfer data from one port to another.

This helps us by allowing access to internal resources whose ports are filtered through a machine we’ve successfully compromised.

## **Pivoting**

When we've gained our foothold and want to access more internal networks in the organization, we need to reach our **pivot host**, which will lead us to another segment in the network, allowing us to go deeper.

For example: When we gain control over a host and want to explore more stations in the organization, and we find an IT manager's machine with multiple network interfaces, it can serve as a pivot point, enabling us to advance further into the next network, and so on.
