# Self-hosted Mail Server

The self hosted mail server is the foundational component for Email Alias Service V2. It's role is to support SMTP administration and handle the forwarding and managment of emails. 

An evaluation of open source SMTP servers can be found in [Appendix A -SMTP Server Review](#appendix-a---smtp-server-review).

**Example**

Following is an example scenario from [Email Alias Service](#appendix-d---email-alias-service):

> 1. Buy a .country domain (e.g., fsociety.country)
> 2. Activate Email Alias Service by setting a forwarding email address (e.g., mr.robot@gmail.com)
> 3. Done! Now you can receive emails at hello@fsociety.country. All emails will be secretly forwarded to mr.robot@gmail.com

Here is the same example broken down in to the technical components, in this example we assume the use of Maddy email server[^71]

 *Note: Maddy was chosen over mailcow due to it's modular framework [^72] support allowing for development of web3 plugins*

**Technical Process Flow (High Level)**

1. Buy a .country domain (e.g., fsociety.country)
   1. User interacts with dot-country fronted [^5]
   2. Pricing is retrieved from [DC.sol](https://github.com/polymorpher/dot-country/tree/main/contracts/contracts) via the relayer
   3. User Authorizes and purchases the domain using the 1ns contracts deployed via ens-deployer [^9].
2. Activate Email Alias Service by setting a forwarding email address (e.g., mr.robot@gmail.com)
   1. User interacts with EAS Frontend[^6]
   2. EAS Frontend calls an DEES API which interacts with the DEES Maddy sender /recepient rewriting(DMSR) custom developed module [^74] or alternatively interacts with the server EAS server[^6].
   3. MSR Module interacts with go-1ns [^10] to update the EAS Contract[^6] in the Web3 Backend.
3. Done! Now you can receive emails at hello@fsociety.country. All emails will be secretly forwarded to mr.robot@gmail.com
   1. An email is sent to hello@fsociety.country.
   2. DEES SMTP Server Receives the email and processes it via the SMTP Pipeline [^73] which updates the recipient via DEES Maddy sender /recepient rewriting(DMSR) custom developed module [^74] and then forwards the email 


**Infrastructure Components and Development Tasks**

*Note for the DEES API development we can either enhance the existing Email Alias Service [^6} function to add SMTP administration API's or build a standlone server in go (Option 2). Initial thoughts is option 1 may be the preferred approach however both options have been included below for completeness.*


1. Email Alias Service [^6]
   1. Server: Modify to use DEES API instead of ImprovX
2. ENS-Deployer [^9]
   1. Include Deployment of EAS.sol (optional)
3. go-1ns [^10]
   1. Add EAS.sol abi
   2. Add read methods for Email Alias Services
   3. *Add update methods for Email Alias Service (Option 2 only)*
4. DEES - MSR Plugin (New Development)
   1. Integrate with go-1ns
   2. Add read functionality for Email Alias Service
5. DEES - API SMTP Administration 
   1. Email Alias Service - Server [^6] (Option 1)
      1.  Develop [API Layer for SMTP administation](#api-layer-smtp-administation)
   2. DEES API - (Option 2)
      1. Build lightweight go server
      2. Integrate with go-1ns
      3. Develop [API Layer for SMTP administation](#api-layer-smtp-administation)

**Deployment Tasks**

1. Deploy DEES Server - leveraging ns1.hiddenstate.xyz (web2 backend) or ns3.hiddenstate.xyz (web3 backend)

## Web3 Plugin for SMTP administration

DEES SMTP Server Receives the email and processes it via the SMTP Pipeline [^73] which updates the recipient via DEES Maddy sender /recepient rewriting(DMSR) custom developed module [^74] and then forwards the email 

**Infrastructure Components and Development Tasks**

DEES - MSR Plugin (New Development)
1. Integrate with go-1ns
2. Add read functionality for Email Alias Service


## API Layer (SMTP administation)

The DEES mail server provides the following functionality (exposed by API's[^61] and backed by a web3 backend using IPFS for storage):

1. Account Management: provides the ability to retrieve your DEES account information and a list of whitelabeled domains.
2. Domain Managment: provides the ability to manage domains (list, add, get, update, delete) and check mx entries for the domain.
3. Aliases: provides the ability to manage aliases for a given domain (list, add, bulk add, update, delete).
4. Logs: Provides the ability to retrieve logs for both domains and aliases.
5. SMTP Credentials:  provides the ability to manage SMTP credentials (list, add, update, delete).

For a comparison of SMTP servers see [Appendix A - SMTP Server Review](#appendix-a---smtp-server-review) and for build out approach see [Appendix B - Implementation Strategy](#appendix-b---implementation-strategy).

**Infrastructure Components and Development Tasks**

*Note for the DEES API development we can either enhance the existing Email Alias Service [^6} function to add SMTP administration API's or build a standlone server in go (Option 2). Initial thoughts is option 1 may be the preferred approach however both options have been included below for completeness.*

DEES - API SMTP Administration 
1. Email Alias Service - Server [^6] (Option 1)
  1.  Develop [API Layer for SMTP administation](#api-layer-smtp-administation)
2. DEES API - (Option 2)
  1. Build lightweight go server
  2. Integrate with go-1ns
  3. Develop [API Layer for SMTP administation](#api-layer-smtp-administation)

## Encrypted Mail

We could generate a key-pair for EAS user, and have the public key committed to EAS contract. We can implement something on our mail server such that all mails will be encrypted using the public key (if the user opts in), so only the user who holds the private key may decrypt and view the emails. It is hard to make this work for existing wallets (such as MetaMask), since the private key is not exposed and there is generally no "decrypt" API offered by the wallet. However, we may be able to add this as an interesting feature to SMS Wallet, since the private key is readily available in the local store.


## Decentralized Storage Plugin

Storage of emails could be done via IPFS and optionally encrypted.
For a reference implementation see Message Lifecyle in the Mailchain Whitepaper[^31].

**Infrastructure and developement tasks**
1. Develop IPFS Storage Plugin [^75]
2. Replace DKIM Plugin[^76] with web3 encryprton module.


## Appendices

### Appendix A - SMTP Server Review

Following is an overview of some [open source smtp github repositories](https://github.com/search?o=desc&q=smtp&s=forks&type=Repositories) selection criteria include

1. Language (Go, Node are preferred)
2. Adoption (Number of forks and quality of documentation)
3. Plugin capability (ability to add a plugin or modify the code modularly to integrate with web3 backend and optionally decentralized storage)
4. Extensibility capabilities to extend an API layer to support SMTP administration

Sample Repositories

* [GoLang](https://go.dev/#) [search](https://github.com/search?l=Go&o=desc&q=smtp&s=forks&type=Repositories)
  * [Maddy Mail Server](https://github.com/foxcpp/maddy): Maddy Mail Server implements all functionality required to run a e-mail server. It supports [modules](https://maddy.email/reference/modules/) this is the [Issue capturing Modular Design](https://github.com/foxcpp/maddy/issues/15) and [here are the go docs](https://pkg.go.dev/github.com/foxcpp/maddy@v0.6.2#section-readme). Here is an article[Self-Hosted Email With maddy: A Naive First Attempt](https://mediocregopher.com/posts/selfhosted-email-with-maddy).
  * [go-smtp](https://github.com/emersion/go-smtp)
  * [mail-cow](https://github.com/mailcow/mailcow-dockerized): The integrated mailcow UI allows administrative work on your mail server instance as well as separated domain administrator and mailbox user access.
  * [MailHog](https://github.com/mailhog/MailHog): MailHog is an email testing tool for developers. [smtp](https://github.com/mailhog/smtp)
  * [go-guerilla](https://github.com/flashmob/go-guerrilla)
* [Nodejs](https://nodejs.org/en/) [search](https://github.com/search?l=JavaScript&o=desc&q=smtp&s=stars&type=Repositories)
  * [Haraka](https://github.com/haraka/Haraka): Haraka is a highly scalable node.js email server with a modular plugin architecture. ([configuration](https://github.com/haraka/Haraka#configure-haraka), [plugins](https://github.com/haraka/Haraka/blob/master/config/plugins))
  * [nodemailer smtp-server](https://github.com/nodemailer/smtp-server): Nodemailer is a module for Node.js applications  [docs](https://nodemailer.com/extras/smtp-server/)
* [C, c++](https://en.wikipedia.org/wiki/C_(programming_language)) (searches [C](https://github.com/search?l=C&o=desc&q=smtp&s=stars&type=Repositories), [C#](https://github.com/search?l=C%23&o=desc&q=smtp&s=stars&type=Repositories), [C++](https://github.com/search?l=C%2B%2B&o=desc&q=smtp&s=stars&type=Repositories) )
  * [https://github.com/hmailserver/hmailserver](https://github.com/hmailserver/hmailserver)
* [Python](https://www.python.org/)
  * [https://github.com/modoboa](https://github.com/modoboa)
* others
    * [https://github.com/iredmail/iRedMail](https://github.com/iredmail/iRedMail)


### Appendix B - Implementation Strategy

**Infrastructure Components and Development Tasks**

*Note for the DEES API development we can either enhance the existing Email Alias Service [^6} function to add SMTP administration API's or build a standlone server in go (Option 2). Initial thoughts is option 1 may be the preferred approach however both options have been included below for completeness.*


1. Email Alias Service [^6]
   1. Server: Modify to use DEES API instead of ImprovX
2. ENS-Deployer [^9]
   1. Include Deployment of EAS.sol (optional)
3. go-1ns [^10]
   1. Add EAS.sol abi
   2. Add read methods for Email Alias Services
   3. *Add update methods for Email Alias Service (Option 2 only)*
4. DEES - MSR Plugin (New Development)
   1. Integrate with go-1ns
   2. Add read functionality for Email Alias Service
5. DEES - API SMTP Administration 
   1. Email Alias Service - Server [^6] (Option 1)
      1.  Develop [API Layer for SMTP administation](#api-layer-smtp-administation)
   2. DEES API - (Option 2)
      1. Build lightweight go server
      2. Integrate with go-1ns
      3. Develop [API Layer for SMTP administation](#api-layer-smtp-administation)


### Appendix C - Reference Architectures

Following are some reference architectures

**Modulo 1wallet components**

The following is a componet diagram for 1wallet [^7]. This can be leveraged as well as the sms-wallet [^8] for [Digital Identity Support](#digitial-identity-support) and [Crypto Asset Management](#crypto-asset-management).

![Modulo Components](/assets/research/modulo-components.jpeg "Modulo Components")

**Dmail architecture**

This architecure is from the Dmail Network litepaper [^42]. Their plan is to build 5 technical modules:

1. Mail services
2. Distributed storage
3. Authentication Gateway
4. Mail Gateway
5. Asset Gateway

![Dmail Architecture](/assets/research/dmail-architecture.png "Dmail Architecture")

**Mailchain Ecosystem**

This ecosystem diagram is from the Mailchain Whitepaper [^41]. Their implementation [^53] includes three core, independent technical components:

1. Registry: provides an immutable history of records and changes
2. Storage: provides data availability guarantees to meet data lifecycle and retention policies.
3. Transport: accepts and stores “delivery requests”, also known as message transactions, which are immutable and contain the information needed for a recipient to determine if a delivery request is intended for them, then locate their messages.

![Mailchain Ecosystem](/assets/research/mailchain-ecosystem.png "Mailchain Ecosystem")


### Appendix D - Email Alias Service

The Email Alias Service (EAS) provides .country domain owners email alias addresses which they can privately forward to their existing email addresses.

Example:

1. Buy a .country domain (e.g., fsociety.country)
2. Activate Email Alias Service by setting a forwarding email address (e.g., `mr.robot@gmail.com`)
3. Done! Now you can receive emails at `hello@fsociety.country`. All emails will be secretly forwarded to `mr.robot@gmail.com`

Your forwarding address will remain private. It will not be publicly visible or stored on-chain.  

**Initial Design**

The initial version will be implemented using a smart contract to store the activation and commitment from the user. The email forwarding will be performed by [ImprovMX](https://improvmx.com), a web2 email forwarding service. In subsequent versions, we may implement a customized email server interacting with smart contracts to authenticate domain ownership and read mail forwarding rules.

Multiple addresses (in addition to `hello@...`) and regex capturing support (e.g., `([a-z]+)\.([a-z]+)` captured as $1 and $2, a typical pattern for first name and last name in company email accounts) can be added in later versions

**EAS Smart Contract**

EAS is activated in a smart contract separate from dot-country (DC). The activation is free but requires the user to own an existing DC domain. During activation, the user needs to provide the following:

1. the SLD (second-level domain) name (e.g., `fscoiety`)
2. the alias to receive email under the user's .country domain, defaults to be `hello` (the initial version will only support a single alias)
3. the commitment hash computed by `keccak256(bytes.concat(alias,forwardAddress,signature))`, where:
   - `forwardAddress` is the destination email address (e.g., `mr.robot@gmail.com`), and 
   - `signature` is the EIP-197 signature produced by the user over a user-friendly message that includes the forwardAddress (e.g., `You are about to authorize forwarding all emails from hello@fsociety.country to mr.robot@gmail.com`)

The smart contract will verify that the user's address indeed owns the domain name under the DC contract, and if so, store the mapping entry of (domain name) -> (alias, commitment hash). EAS can be marked for deactivation by the user at any time. In the initial version, deactivation alone on the smart contract is insufficient to deactivate the service. The EAS server must be notified to complete the deactivation.

**Necessity of Smart Contract**

One may ask why a smart contract is necessary at all for the initial version of EAS - can't we ask the user to sign a message to prove wallet-address ownership, send the signature and configuration to the server, and have the server check the address owns the corresponding DC domain?

The concern here is security and configuration visibility to the user. A hacker could request the user to produce the required signature and mix the request in a series of legitimate requests (e.g., NFT trading, airdrop, defi app). Many unsuspecting users may fall into this trap and unwittingly allows the hacker to hijack their .country emails. The result is that the hacker could configure the user's EAS and intercept all emails meant for the user - it is a major security risk.

By requiring configurations to be committed to the contract, any change would require a contract interaction. It is much harder to mix or spoof compared to signature requests. The user would have more opportunities to review the details. Plus, the user could gain visibility on any change to their EAS configuration. The client could actively monitor for unexpected changes and alert the user if necessary.


**EAS server**

EAS server provides APIs for users to complete the activation and deactivation of the service. When an API is called, the EAS server verifies the smart contract's state concerning the corresponding user, domain, and alias before communicating with our DNS server and ImprovMX API to complete the action.

**APIs**

Sever root: `1ns-eas.hiddenstate.xyz`

**Response Formats**

All responses are in JSON. Response codes are below:

- 200 OK: It's working
- 400 Bad Request: Something is wrong with your request parameters. See the `error` field in the response for details
- 401 Unauthorized: Signature verification failed
- 500 Internal Server Error: Please report to us if you see this error, with as much detail as possible, including the entire response and the request


**Routes**

**Activate an alias for a domain.**

```
POST /activate

{
   "alias": "hello",
   "sld": "fsociety",
   "forwardAddress": "mr.robot@gmail.com",
   "signature": "0x123456..."
}

```

The server checks:

- in the EAS contract, the stored commitment hash corresponding to alias and domain (SLD) equals `keccak256(bytes.concat(alias,forwardAddress,signature))`
- in the DC contract, the address of the owner of the domain (SLD) is the signer of the signature, given the message corresponding to alias and forwardingAddress (format shared between EAS server and frontend clients)


**Deactivate an alias for a domain**

```
POST /deactivate

{
   "alias": "hello",
   "sld": "fsociety",
}

```

The server checks that the commitment hash corresponding to the alias and domain (SLD) in the EAS contract is cleared to empty value.


**EAS Client**

EAS Client can be in both the command line and web interfaces. Its job is simple: 

- collect three parameters alias, forwardAddress, and domain
- connect with the user's wallet to produce the required signature
- interact with EAS smart contract via the user's wallet to configure EAS
- interact with the EAS server to complete the configurations


## Footnotes
<!-- Footnotes -->

<!-- Prior Work -->

[^1]: [RADICAL Market for Internet Domains, Crypto Names & Top-Level .country](https://open.harmony.one/rnft-market): Harmony is developing a maketplace, called RADICAL, specialized for domain names as nonfungible tokens (NFT).

[^2]: [.1.country subdomains](https://harmonyone.notion.site/1-country-subdomains-206b895b0b7841d18dd2d3ad6415ed26): Rent domains under .1.country (”ONE Country”) like r/place or “The Million Dollar Homepage”.

[^3]: [Browser extension for .1 redirect](https://harmonyone.notion.site/Browser-extension-for-1-redirect-fd249c09c1414e3786b929b1fe265a62): Develop browser extensions to resolve .1 Web3 crypto names to .1.country Web2 website domains.

[^4]: [.country Top-Level Domain](https://harmonyone.notion.site/country-Top-Level-Domain-5db61512025a4db88114785b4f899d7e): Minimal demo: Register new proper domains like s.country with ONE tokens as registrar extension – such that Tuscow manages the normal flows of fiat purchases and record maintenance. Custody and contact information can be just “Hidden States LLC” for now.

[^5]: [Project .country](https://github.com/harmony-one/dot-country/blob/main/README.md): the DC contract is meant to provide some "extra service" on top of ENS contracts. Therefore, it charges some additional fees on top of purchasing an ENS domain from the ENS app. (github)

[^6]: [Email Alias Service](https://github.com/harmony-one/dot-country/blob/main/mail.md): The Email Alias Service (EAS) provides .country domain owners email alias addresses which they can privately forward to their existing email addresses. (github)

[^7]: [1wallet](https://github.com/polymorpher/one-wallet/blob/master/README.md): 1wallet is designed for people who want the best and the latest from the world of crypto, but do not want to deal with senseless "mnemonic words", "private keys", or "seed phrases".  (github). 

[^8]: [SMS Wallet](https://github.com/polymorpher/sms-wallet): The SMS Wallet is a lightweight non-custodial wallet solution for users to quickly create a wallet bound to their phone number, receive (or purchase) inexpensive NFTs, showcase their NFTs, and to hold a small amount of crypto assets. (github)

[^9]: [ens-deployer](https://github.com/harmony-one/ens-deployer): 1NS related contract deployment, customized based on ENS contracts.

[^10]: [go-1ns](https://github.com/jw-1ns/go-1ns): Go module to simplify interacting with the 1 Name Service contracts. Initial version copied from go-ens

[^11]: [coredns-1ns](https://github.com/jw-1ns/coredns-1ns): CoreDNS-ONENS is a CoreDNS plugin that resolves DNS information over ONENS. It has two primary purposes 1. A general-purpose DNS resolver for DNS records stored on the Ethereum blockchain, 2. A specialised resolver for IPFS content hashes and gatways.


<!-- Blockchain email articles -->

[^21]: [Blockchain Email Now and in the Future Perspective](https://mailtrap.io/blog/email-and-blockchain/): The promise of blockchain in email (2022)

[^22]: [4 Best decentralized email clients for Windows 10/11](https://windowsreport.com/decentralized-email/): Some of the best apps for you to try in order to have more secure protection over your emails. (2021)

[^23]: [Blockchain-Based Email: Does the World Really Need It?](https://www.bitrates.com/news/p/blockchain-based-email-does-the-world-really-need-it): Some projects are developing blockchain-based email services. This could enhance security and privacy―but are these services truly useful? (2019)

[^24]: [OpenTechFund secure-email](https://github.com/OpenTechFund/secure-email): There are an increasing number of projects working on next generation secure email or email-like communication. This is an initial draft report highlighting the projects and comparing the approaches. (2017)

<!-- Blockchain email projects -->

[^31]: [Mailchain](https://mailchain.com/): All of Web3. All in one Inbox. Blockchain mail project with [whitepaper](https://uploads-ssl.webflow.com/6193af32c4bb83588771612b/63615c7c45b9763f80123418_Mailchain%20Whitepaper%20v0.3.pdf), [documentation](https://docs.mailchain.com/developer/) and [github](https://github.com/mailchain).

[^32]: [Dmail Network](https://dmail.ai/): Construct DID in Web 3.0, Not just an Email. Blockchain digital identity project with [light paper](https://dmail.ai/Dmail_litepaper.pdf), [documentation](https://dmailofficial.gitbook.io/helpcenter/v/english/product-tutorial/how-to-use-dmail), [medium](https://medium.com/@dmail_official) and [github](https://github.com/dmailofficial) is no longer visible.

[^33]: [LedgerMail](https://ledgermail.io/): World's FirstBlockchain Email Service. Blockchain email project with [blog](https://ledgermail.io/blog.html) and [github](https://github.com/Ledgermail).


<!-- Whitepapers -->

[^41]: [Mailchain: The Communication Protocol For Web3](https://uploads-ssl.webflow.com/6193af32c4bb83588771612b/63615c7c45b9763f80123418_Mailchain%20Whitepaper%20v0.3.pdf): Mailchain is the communication layer for Web3 designed to serve billions of users, each with multiple identities, providing a private, end-to-end encrypted mail experience. This paper outlines the background, design approach, technology, and roadmap of the Mailchain protocol.

[^42]: [Dmail Network](https://dmail.ai/Dmail_litepaper.pdf): The transmission of a string of words opened the door to the Internet, thus beginning the booming development of the Internet. In the Crypto World, we also hope to take information transmission as a starting point and promote a leapfrog development of applications in the blockchain ecosystem. Therefore, we established a decentralized version of NFT mail application called "Dmail."

<!-- Github Repositories Blockchain email projects-->

[^51]: [DonaldKwan/dmail](https://github.com/DonaldKwan/dmail): Decentralized Email is an Ethereum DApp that combines messaging, modern cryptographic techniques, and the blockchain.

[^52]: [noman-land/Dmail](https://github.com/noman-land/dMail): Distributed, decentralized mail. Built on Ethereum and IPFS

[^53]: [mailchain](https://github.com/mailchain): Mailchain github organization. Includes [mailchain-sdk-js/packages](https://github.com/mailchain/mailchain-sdk-js/tree/main/packages).

<!-- Additional References and Standards-->

[^61]: [ImprovMX](https://improvmx.com/api/): The [Improvmx](https://improvmx.com/) hosted smtp server api's provide a good reference point for the DEES web3 implementation.

[^62]: [DomainKeys Identified Mail](https://en.wikipedia.org/wiki/DomainKeys_Identified_Mail): is an email authentication method designed to detect forged sender addresses in email (email spoofing), a technique often used in phishing and email spam.


<!-- Maddy email server-->

[^71]: [Maddy Mail Server](https://github.com/foxcpp/maddy): Maddy Mail Server implements all functionality required to run a e-mail server. [Here are the go docs](https://pkg.go.dev/github.com/foxcpp/maddy@v0.6.2#section-readme). Here is an article[Self-Hosted Email With maddy: A Naive First Attempt](https://mediocregopher.com/posts/selfhosted-email-with-maddy). [Here is a comparison with MailCow](https://maddy.email/faq/#how-maddy-compares-to-mailcow-or-mail-in-the-box).

[^72]: [Maddy Modular Design](https://maddy.email/reference/modules/): maddy is built of many small components called "modules". Each module does one certain well-defined task.  This is the [Issue capturing Modular Design](https://github.com/foxcpp/maddy/issues/15).

[^73]: [Maddy SMTP message routing pipeline](https://maddy.email/reference/smtp-pipeline/): Message pipeline is a set of module references and associated rules that describe how to handle messages.

[^74]: [Maddy Envelope sender / recipient rewriting](https://maddy.email/reference/modifiers/envelope/): `replace_sender` and `replace_rcpt` modules replace SMTP envelope addresses based on the mapping defined by the table module (maddy-tables(5)). It is possible to specify 1:N mappings. This allows, for example, implementing mailing lists.

[^75]: [Maddy Storage](https://maddy.email/reference/storage/imap-filters/): Maddy provides a suite of storage capabilities including [IMAP Fileters](https://maddy.email/reference/storage/imap-filters/) allows code to change target folder and add IMAP flags (keywords) to the message, [SQL-indexed storage](https://maddy.email/reference/storage/imapsql/) database for IMAP index and message metadata using SQL-based relational database and Blob Storage to store message bodies currently supporting [Amazon S3](https://maddy.email/reference/blob/s3/) and [File System](https://maddy.email/reference/blob/fs/). *Note: custom modules could be developed to suport IPFS and provide a decentralized storage layer.*

[^76]: [Maddy DKIM Signing](https://maddy.email/reference/modifiers/dkim/): modify.dkim module is a modifier that signs messages using DKIM protocol (RFC 6376).





<!-- Additional Items -->