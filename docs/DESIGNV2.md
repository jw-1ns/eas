# Email Services

# Abstract

This follows on from the work on subdomains[^1], domains[^2].


## Self-hosted Mail Server

We should move away from ImprovMX and host our own mail servers. Ideally, we can select one of the existing open source mail server solutions and build a blockchain-plugin for that for reading configuration and data on-chain, similar to what we did for hosting our own DNS server using CoreDNS. Here are a few options to consider:

- https://github.com/iredmail/iRedMail
- https://github.com/hmailserver/hmailserver
- https://github.com/haraka/Haraka
- https://github.com/modoboa
- https://github.com/emersion/go-smtp

The list is just a result of 15-minute search. Please feel free to add more and contribute some comparative analysis

## Encrypted Mail

We could generate a key-pair for EAS user, and have the public key committed to EAS contract. We can implement something on our mail server such that all mails will be encrypted using the public key (if the user opts in), so only the user who holds the private key may decrypt and view the emails. It is hard to make this work for existing wallets (such as MetaMask), since the private key is not exposed and there is generally no "decrypt" API offered by the wallet. However, we may be able to add this as an interesting feature to SMS Wallet, since the private key is readily available in the local store.



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

<!-- Blockchain email articles -->

[^21]: [Blockchain Email Now and in the Future Perspective](https://mailtrap.io/blog/email-and-blockchain/): The promise of blockchain in email (2022)

[^22]: [4 Best decentralized email clients for Windows 10/11](https://windowsreport.com/decentralized-email/): Some of the best apps for you to try in order to have more secure protection over your emails. (2021)

[^23]: [Blockchain-Based Email: Does the World Really Need It?](https://www.bitrates.com/news/p/blockchain-based-email-does-the-world-really-need-it): Some projects are developing blockchain-based email services. This could enhance security and privacy―but are these services truly useful? (2019)

[^24]; [OpenTechFund secure-email](https://github.com/OpenTechFund/secure-email): There are an increasing number of projects working on next generation secure email or email-like communication. This is an initial draft report highlighting the projects and comparing the approaches. (2017)

<!-- Blockchain email projects -->

[^31]: [Mailchain](https://mailchain.com/): All of Web3. All in one Inbox. Blockchain mail project with [whitepaper](https://uploads-ssl.webflow.com/6193af32c4bb83588771612b/63615c7c45b9763f80123418_Mailchain%20Whitepaper%20v0.3.pdf), [documentation](https://docs.mailchain.com/developer/) and [github](https://github.com/mailchain).

[^32]: [Dmail Network](https://dmail.ai/): Construct DID in Web 3.0, Not just an Email. Blockchain digital identity project with [light paper](https://dmail.ai/Dmail_litepaper.pdf), [documentation](https://dmailofficial.gitbook.io/helpcenter/v/english/product-tutorial/how-to-use-dmail), [medium](https://medium.com/@dmail_official) and [github](https://github.com/dmailofficial) is no longer visible.

[^33]: [LedgerMail](https://ledgermail.io/): World's FirstBlockchain Email Service. Blockchain email project with [blog](https://ledgermail.io/blog.html) and [github](https://github.com/Ledgermail).


<!-- Whitepapers -->

[^41]: [Mailchain: The Communication Protocol For Web3](https://uploads-ssl.webflow.com/6193af32c4bb83588771612b/63615c7c45b9763f80123418_Mailchain%20Whitepaper%20v0.3.pdf): Mailchain is the communication layer for Web3 designed to serve billions of users, each with multiple identities, providing a private, end-to-end encrypted mail experience. This paper outlines the background, design approach, technology, and roadmap of the Mailchain protocol.

[^42]: [Dmail Network](https://dmail.ai/Dmail_litepaper.pdf): The transmission of a string of words opened the door to the Internet, thus beginning the booming development of the Internet. In the Crypto World, we also hope to take information transmission as a starting point and promote a leapfrog development of applications in the blockchain ecosystem. Therefore, we established a decentralized version of NFT mail application called "Dmail."

<!-- Github Repositories Blockchain email projects-->

[^51]: [DonaldKwan/dmail](https://github.com/DonaldKwan/dmail): Decentralized Email is an Ethereum DApp that combines messaging, modern cryptographic techniques, and the blockchain.

[^52]: [noman-land/Dmail](https://github.com/noman-land/dMail): Distributed, decentralized mail. Built on Ethereum and IPFS ()


