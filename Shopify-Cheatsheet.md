[back to overview](/../..)

# Shopify Cheatsheet<!-- omit in toc -->

- [Terms](#terms)
- [Spin](#spin)
    - [Isospin](#isospin)
    - [Cartridges](#cartridges)
    - [GPG Signing](#gpg-signing)
- [Dev](#dev)
- [SFN Goals 2022](#sfn-goals-2022)
- [Links](#links)

# Terms

- [Shopify Acronyms](https://vault.shopify.io/pages/2435-Common-Terms-and-Acronyms)
- [SFN Glossary Doc](https://docs.google.com/spreadsheets/d/1rXbSDt_MHpXhAVOcO3QO_LYJAsHK71Q_8pY8igqx-7c/edit#gid=0) OR [SFN Glossary Wiki](https://sfn.docs.shopify.io/Wiki/Glossary-&-Business-Logic)
- `3PL` - "Third-Party Logistics" - provides outsourced logistics services
- `WMS` - Warehouse Management System
- `D2C/DTC` - "Direct to Consumer" - no wholesaler/distributor/retailer
- `Kitting` - taking different individual products and pairing them together to create a new SKU that includes multiple products but is sold as a single item.
- `FMS` - Fulfillment Managment System
- `6RS` - Six Rivers Systems - warehouse automation and management technology developer acquired in 2019
- `ASN` - Advanced Shipping Notice - notification of pending deliveries
- `North Star` - single metric most predictive of an organization's long term success
- `Bill of Lading` - document from carrier to shipper detailing type, quantity, destination of goods carried. Also a shipment receipt when goods are delivered
- `carrier` - transports goods
- `shipper` - supplier or owner of goods to be shipped
- `shrinkage` - loss of inventory (theft, error, fraud, damage, mislabel, miss-picked, cross-shipment)
- `cross-shipment` - order shipped to wrong destination
- `MiCo` - [Mission Control](https://fbs.shopifycloud.com/mission_control/home)

# Spin

[FAQ](https://development.shopify.io/engineering/keytech/spin/faq)
- `spin up <repo> [--no-snapshots]` - create instance of <repo> from snapshot (or direct from main with --no-snapshots)
- `spin code` - open instance in vs code
- `spin open` - run instance in browser
- `spin shell` - connect to instance shell
- `spin list` - list all open instances
- `spin destroy [<instancename>|--all]` - destroy open instance(s)

## [Isospin](https://development.shopify.io/engineering/keytech/spin/isospin/isospin)
The OS spin instances run.
- `iso procs list` - returns application names
- `iso procs [stop|start|restart] <applicationName>`

## [Cartridges](https://development.shopify.io/engineering/keytech/spin/isospin/tools#Moving_data_between_your_instances)
- `cartridge create <name>` create new cartridge at ~/.data/cartridges/<name>
- `cartridge save <name>` save <name> to cloud
- `cartridge list` list cartridges. to see inserted just `ls ~/.data/cartridges/`
- `cartridge insert <name>` retrieve <name> from cloud to ~/.data/cartridges/<name>

## [GPG Signing](https://development.shopify.io/engineering/keytech/spin/faq#How_do_I_use_GPG_signing_within_Spin_)
- if "error: gpg failed to sign the data" then killall gpg-agent; gpgconf --launch gpg-agent; spin shell

# Dev

# SFN Goals 2022

- North Star: **# of 2 Business Day Fulfillments**
- Tripwires:
    - **Cost per Package** - node placement rather than label upgrades?
    - **% of Merchants > SLA** - failing smaller volute merchants?
    - **% of 2-Day Fulfillments** - high volume?

# Links
- [Ruby Style Guide](https://github.com/Shopify/ruby-style-guide)
- [Trifecta Guide](https://github.com/Shopify/trifecta)
- [Polaris Design System](https://polaris.shopify.com/)
- [React Best Practices](https://github.com/Shopify/web-foundations/tree/main/handbook/Best%20Practices/React)
- [React Native API](https://reactnative.dev/docs/)
