# RawBox

### Manifesto

> Today, achieving privacy on the Internet is challenging, even for experienced professionals. Private Internet starts with connecting to the network without interaction with a third party: an Internet provider, a mobile operator, a hotel administrator, or a hotspot captive portal at the airport, etc. Privacy is often lost at the first step - paying for internet access with a credit card or the requirement to disclose personal identifiers like phone number. Our team wants everyone worldwide to get anonymous access to the network by creating a genuinely uncensored Internet gateway. This is a new-generation router with a focus on privacy that will become the foundation of the decentralized Internet.
> 

> A router is a physical device that, in most cases, is connected to the network 24/7 and is often underutilized. Personal home-office routers are undervalued. We create RawBox, NYM-enabled router that will become the closest physical access point to the NYM mixnet infrastructure for the user. Anyone can buy private internet directly from such routers by paying it in BTC/Lightning Network.
We want to allow everyone to enter the decentralized economy and become an independent privacy provider, receiving fair payment for both transport/bandwidth and additional services such as gateways, mix-network, tor, i2p, etc.
> 

## Main focus points of the Nym Squad

- Significant scaling of the Nym infrastructure by creating a privacy-focused FOSS self-sovereign router for individual use or small-medium business.
- Providing truly anonymous network access for users beginning from payment.
- Educate people on how privacy works in practice.
- Create a use case to demonstrate how the economy of the decentralized network providers should work.
- Further research on extending the list of privacy services that the RawBox can provide, such as decentralized assured backup of routers’ configuration, lightning node state, and other small but critical parts of users’ data.

## **Planned actions**

[RawBox Roadmap.mp4](https://prod-files-secure.s3.us-west-2.amazonaws.com/66a50a93-8fea-4cfd-a731-6934ecafc42f/dfe2888d-c77f-4bc5-815d-2f55d9e6677c/RawBox_Roadmap.mp4)

- Create a step-by-step guide on how to build your own router from the open hardware and software and make money on it as a self-sovereign individual.
- Create a proof-of-concept (prototype) of the RawBox router based on [OPNsense](https://opnsense.org/) (FOSS enterprise-grade firewall), BTCPay Server, and Nym services.
- Create the plugin for the OPNsense, allowing people to buy vouchers at the captive portal with BTC/Lightning via BTCPay Server directly from the router and create an isolated environment for each user (VLANs, rules, etc).
- Create deployment script for Nym gateway and mix-node to the router and build it into the web-based admin panel.
- Create reference design of RawBox for single and large deployments.
- Сreate a basic physical infrastructure for a new generation of decentralized Internet.

## Technical implementation steps

[Blue Modern Business Roadmap Presentation (1).mp4](https://prod-files-secure.s3.us-west-2.amazonaws.com/66a50a93-8fea-4cfd-a731-6934ecafc42f/5ca11b6b-27d3-4ae9-bbc6-127d51c1d898/Blue_Modern_Business_Roadmap_Presentation_(1).mp4)

1. **We set up a mix node and an exit gateway on OPNsense.** This will not be the most trivial task because we will have to build mixnode from sources for FreeBSD =)
**Result**: Home/office routers powered by Nym.
2. **We install Bitcoin & Lightning full nodes on OPNsense.**
**Result**: We obtain a self-hosted core of privacy (direct access to the Nym network from the local network; the router acts as a proxy) and sovereignty (the ability to work with Bitcoin privately through your own full node and router).
3. **Deploy a BTC Pay server** for fully non-custodial payment acceptance.
    
    **Result**: We control our money by ourselves
    
4. Create **an admin panel**. The router's owner can specify the allowed speed and set the rental price here.
5. **Create a plugin for OPNsense** that, on one side, modifies the Captive Portal (the hotspot web page):
    - Check the available network speed from the providers’ uplink.
    - Allows guest users to select their speed in megabits and rental time (slots).
    - Connects to the BTCPay Server API.
    - Ask the user to pay in BTC/Lightning at the Captive Portal.
    - Accepts payment and receives confirmation from the BTCPay Server.
    - Generates an isolated user profile with a separate VLAN, firewall rules, traffic shaping, etc.
    - Provides the user a token (credentials) for connecting to the WiFi network.
    
    **Result** of 4 & 5: the router is not the router anymore. Since this time, it's a RawBox.
    
6. Once this system is operational, we can **modify the plugin to allow the router owner to allocate slots** for exit gateways by selling part of their Nym VPN channel (next steps: exploring how to charge for this).
**Result**: scale RawBox services
7. Local users who previously purchased a raw internet channel can be **offered direct access to the mixnet or NymVPN at high speed** by selling those services for an additional fee.
    
    **Result**: Scaled Nym and privacy adoption.
    

***Note: Some steps will require some help from the Nym core team.***

---

## Why it’s important

You'll find the idea of privacy at the heart of everything Nym does. This is why ensuring privacy at every stage of information movement, from entry to exit, and at every node and participant, is crucial.

The **concept of routers** has been repeatedly discussed as a **significant element in the [Whitepaper](https://nymtech.net/nym-whitepaper.pdf)**.

### **Whitepaper significant quotes related to routers**

- The Mixnet itself is described as “a network of mix nodes, which are overlay routers that transform and reorder messages” [page 11]
- **Comparison to onion routing** is also shown there [page 11]. This ****case is also [mentioned in a recent **Messari report**](https://messari.io/report/understanding-nym) dedicated to Nym, specifically about Tor's onion routers.
- In **Section 4.1,** “Layered mixnet topology” we see some process-based descriptions like “Layered routing implies that first-layer mix nodes only receive messages from gateways while nodes in subsequent layers only receive messages from nodes in the preceding layer” [page 12]
- This also applies to Section 4.2, “Gateways”
- The practical aspect is also mentioned in the following context: “One of the most exciting avenues of research is for crucial operations in the processing of Sphinx packets to happen in router hardware; this could enable Nym to scale to use-cases previously unsuitable for mix networking, becoming the default for the entire internet”  [page 33]
- All in all, the concept of “**routing**” is described as follows: “Our goal is for the Nym mixnet to provision the future of **privacy for a large fraction of the internet**, where message-based routing is becoming increasingly relevant in domains from instant messaging to cryptocurrency transactions” [page 32]
- Also, “Verifiable and **location-aware routing** is essential to improve speed and prevent attacks” [page 33], and so on.

**As a starting point, we selected open-source solutions**, such as FOSS OPNsense firewall (acts as a router), as references that can serve as starting points for research and practical applications.

We have already preselected several HW devices/boards on which you can install the software, and it will perform efficiently.

- **Some examples** **of privacy-focused hardware devices with FOSS firmware (Coreboot).**
    
    Router boards with AES-NI-enabled CPUs (hardware accelerated encryption)
    
    - https://docs.dasharo.com/variants/overview/
    - https://eu.protectli.com/product/vp2410/
    - https://teklager.se/en/products/routers/APU2E0-open-source-router
    - https://www.deciso.com/netboard-a10-gen2/
    - https://www.deciso.com/netboard-a20/

## “Win-Win-Win Solution: What makes our router, which is the focus of our squad, valuable?

Our squad's router solution appears to be an integral part of the entire Nym architecture and ecosystem soon, a vital addition and enhancement for the upcoming NymVPN.

**Privacy issues aren't solely addressed through the mixnet. First, there needs to be a private way to connect to it.**

Another **crucial aspect** is ensuring an adequate **quantity and quality of exit gateways, especially in anticipation of the VPN launch**. This is where our **squad's solution is expected to provide significant assistance.**

### Functionality in a nutshell:

This will be a router and a self-custodial node in a single box (**RawBox**). It will be utterly autonomous hardware that handles payments, provides internet access, and routes external traffic.

### What will be beneficial for Nym:

A significant and essential increase in the number of mix nodes significantly enhances the mixing quality.

1. Dramatically increases the amount of the mix nodes and gateways.
2. These mix nodes will be much closer to users in regions, improving the quality of user network access (speed and latency). There will be no need to route through long-distant nodes like those in Poland, the United States, or elsewhere. Safe mixing can be done relatively nearby in a **location-aware routing way.**
3. Decentralization of the mixnet will increase significantly, reducing reliance on major providers like Hetzner, OVH, and DigitalOcean, where the most nodes are hosted now.
4. The number of exit gateways will substantially increase, enhancing the performance of NymVPN.

### Some use cases:

- **Example 1:** You live in Canada and pay $100 monthly for internet. I want to pay $50 or less. My neighbors (or anyone else) can buy the desired bandwidth directly from my home router, even without asking me.
- **Example 2**: I rent an apartment and don't want to negotiate with anyone. I buy internet from a neighbor's router and get guaranteed speed. I can switch to another neighbor if the previous one doesn't meet my needs. This is a market.
- **Example 3**: I'm in a hotel or airport, and I don't want to send my phone, card, hotel room number, etc., to anyone.
- **Example 4**: I'm a privacy enthusiast or a journalist, and I need to send a private message without buying seven SIM cards, and so on. In other words, it's entirely decentralized.

And in the process, I don't lose anything. I still use a high-speed, enterprise-class router.

Besides that, I **can run my mixnode on the router without buying a hosting**.

> **This will seriously enhance decentralization, scalability, and speed in the Nym mixnet.**
> 

### Globally for the worldwide community, this will:

- Allow the creation of a 'people's' internet on top of the existing centralized one (only the uplink is needed from the provider; everything inside is mixed and encrypted).
- Enable router owners to partially or wholly offset their provider costs.
- It will be easy for node operators to install or purchase a node, connect it to the internet at home, and attach their keys. The node will receive regular verified updates and more.

---

## Our members:

- @oleksky
- crptcpchk
- Beyond
- Den Freeman

## Reach us here:

[RawBox - anonymous internet gateway](https://t.me/+G4ixLEgYBXJkZWRi)
