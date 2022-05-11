---
layout: product
name: Vault
image: /public/images/vault_logo.png
active: false
timeline: Oct 2019 - March 2020
description: Vault is a complete cross-banking financial app for India 🇮🇳
link: /products/vault
---

# Vault - A complete finance app for 🇮🇳

Vault was a mobile app (Android and iOS) which allowed users in India to connect their bank accounts and credit cards in one single app.

> Vault automatically summarizes data from all your Bank Accounts, Stocks, Mutual Funds, Loans, Insurance and helps you keep track through one single app.<br/>And it lets you invest your spare money, pays your bills, set saving goals, personally advises on your investments, and helps secure your financial future.

![Vault](/public/images/vault-screen.png){:style="display:block; margin-left:auto; margin-right:auto; width:400px;"}

## What happened? 💔

### Technical difficulties

- I build the first version using Yodlee APIs. But these APIs were very flaky and most of the times went down. So I had to build a lot of workflows to automtically detect failures.
- Over the time during beta phase we realized that we have to build the base solution ourselves.

### Localized problems

- Very few or almost none of the leading banks in India have sophisticated and off-the-shelf usable APIs. This made our job really difficult.
- Eventually we built something like Yodlee in-house based on the scraping mechanism. This solution worked really well and much better than Yodlee.

### Legal problems

- The RBI approved Account Aggregater licence and made it mandotory for building businesses like Vault. And it was a major hurdle for us. We had to go through a lot of legal and regulatory hurdles plus it had some monetory cost which was difficult for us to pay without external funding.
- Getting that licence itself was a big deal. On top of there was a lot of confusion around the capabilities, process and data handling.

{% include disqus.html %}
