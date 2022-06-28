[back to overview](/../..)

# Shopify Cheatsheet<!-- omit in toc -->

- [Terms](#terms)
  - [Business/Marketing General](#businessmarketing-general)
  - [Shipping](#shipping)
  - [Shopify](#shopify)
  - [SFN](#sfn)
- [SFN Goals 2022](#sfn-goals-2022)
  - [Acquisition Goals 2022](#acquisition-goals-2022)
- [Tools](#tools)
  - [Spin](#spin)
    - [Isospin](#isospin)
    - [Cartridges](#cartridges)
    - [GPG Signing](#gpg-signing)
  - [Dev](#dev)
  - [lhm](#lhm)
- [Data Analysis](#data-analysis)
  - [Mode](#mode)
- [Links](#links)
- [Random Tips](#random-tips)

# Terms

## Business/Marketing General
- `North Star` - single metric most predictive of an organization's long term success
- `green path` - long term roadmap
- `TAM` - Total Addressable/Available Market - total possible market demand
- `SAM` - Serviceable Addressable/Available Market - TAM that is within geographical reach
- `SOM` - Serviceable Obtainable Market or Share Of Market - SAM that is realistically capturable
- `ICP` - Ideal Customer Profile - firmographic, environmental and behavioral attributes of accounts
- `Contact Roles` - people external to your company that influence decision making on a sales opportunity
- `Account-Based Marketing` - marketing strategy: concentrate resources on a set of target accounts within a market
- `segment` - business description
- `persona` - person description

## Shipping
- `3PL` - Third-Party Logistics - provides outsourced logistics services
- `D2C/DTC` - "Direct to Consumer" - no wholesaler/distributor/retailer
- `Kitting` - taking different individual products and pairing them together to create a new SKU that includes multiple products but is sold as a single item.
- `ASN` - Advanced Shipping Notice - notification of pending deliveries
- `Bill of Lading` - document from carrier to shipper detailing type, quantity, destination of goods carried. Also a shipment receipt when goods are delivered
- `carrier` - transports goods
- `shipper` - supplier or owner of goods to be shipped
- `shrinkage` - loss of inventory (theft, error, fraud, damage, mislabel, miss-picked, cross-shipment)
- `cross-shipment` - order shipped to wrong destination
- `Reverse Logistics` - Managing flow of finished goods from consumer to origin - returns
- `Delivery Promise` - date by which order will be delivered - consists of the below:
  - `Order Processing Time` - Once payment is confirmed. Merchants may opt to wait to allow for cancel/change
  - `Handling Time` - 3PL SLA - pick, pack, generate shipping label
  - `Delivery Speed` - Carrier SLA
- `Cycle Count` - perpetual inventory auditing procedure, regularly repeated sequence of checks on a subset of inventory
- `Merchant Disposition` - rules set by merchants re: returned goods - can be reintegrated into inventory vs. needs to be discarded
- `Sales Velocity` - pace at which items are selling
- `Sales Lift` - increases in sales when businesses run promotional campaigns during a specific time period

## Shopify
- [Shopify Acronyms](https://vault.shopify.io/pages/2435-Common-Terms-and-Acronyms)
- `BFCM` - Black Friday/Cyber Monday - huge load
- `spike` - time box investigation
- `PXW` - Past X Weeks

## SFN
- [SFN Glossary Doc](https://docs.google.com/spreadsheets/d/1rXbSDt_MHpXhAVOcO3QO_LYJAsHK71Q_8pY8igqx-7c/edit#gid=0) OR [SFN Glossary Wiki](https://sfn.docs.shopify.io/Wiki/Glossary-&-Business-Logic)
- `Fulfillment Center`, `node`, `warehouse` - SFN merchant inventory warehouses
    - Warehouse O (OTT - ON), Yavin (USSE3 - GA), DM (USMW2 - MO), DM (USNE4 - PA), ITS (USW3 - NV)
- `FMS` - Fulfillment Managment System generally, also the **Shopify x 6RS** product
- `6RS` - **Six Rivers Systems** - warehouse automation and management technology developer acquired in 2019
- `MiCo` - [Mission Control](https://fbs.shopifycloud.com/mission_control/home)
- [`Special Projects`](https://sfn.docs.shopify.io/Wiki/Content-guidelines:-Special-projects-and-kitting) - tasks performed by fulfillment centers that aren't part of the usual fulfillment workflows (sometimes `work orders`)
    - Assembly
    - Custom Packaging
    - Specialized Service - i.e. wax boots, fold jackets specific ways
    - `Bundling` - fulfillment center takes products available for individual sale and combines them into a set to be shipped as the physical implementation of a bundle
    - `Product Reworking` - fulfillment center alters a finished product to “update it”. Includes repackaging, relabeling, changing components, repair, etc.
- [`Easy Button`](https://vault.shopify.io/missions/106) - Create the simplest direct to consumer merchant fulfillment solution with simplified inbounding, order management, inventory balancing, and accurate inventory visibility, fully integrated with Shopify’s back office and sales channels.
- [`BaseLayer`](https://vault.shopify.io/pages/15354-BaseLayer-Launch-Milestones) - Mission encompassing Features that result in easy fulfillment with simple pricing - M1 and M2 milestones
- `Reach > Lead` - MAL to PQL
- `Lead > Close` - PQL to Closed-Won
- `C2D` - Click to Deliver rate - % of SFN orders delivered on time
- `MQL/SAL/SQL` - [See here](https://www.ampliz.com/resources/sales-accepted-lead/) and [SFN](https://drive.google.com/file/d/1RmW2dOlHlThGEWVWD460wgFVLLBDyi1o/view?usp=sharing)
    - `Lead` - potential customer, part of an account, targeted for an opportunity
    - `MAL` - Marketing-Accepted Lead - marketing demographically qualified leads
    - `MQL` - Marketing-Qualified Lead - marketing behaviourly qualified leads (take some action)
    - `PQL` - Product-Qualified Lead - Lead who has experience with the product
    - `SAL` - Sales-Accepted Lead - engaged C2A to contact sales
    - `SQL` - Sales-Qualified Lead - qualified by sales team
    - `Closed-Won` - Successfully signed up shop (accept terms)
- `DAO` - Daily Active Orders
- `CTOR` - Click-To-Open Rate
- `fulfillment complexity` - everything warehouse does with products from inbound to packout - aim to reduce
- `business complexity` - everything related to getting order into 3PL (multi-channel, wholesale, ERP needs) - aim to increase

# SFN Goals 2022
[All Metrics](https://docs.google.com/spreadsheets/d/1z6hn3evKgFPXXQXa1slbhSqip5ANio9JmZU6LmJzOxg/edit#gid=1346961718)

- North Star: **# of 2 Business Day Fulfillments**
- Tripwires:
    - **Cost per Package** - node placement rather than label upgrades?
    - **% of Merchants > SLA** - failing smaller volute merchants?
    - **% of 2-Day Fulfillments** - high volume?

## Acquisition Goals 2022
- North Star: **# of merchants discovering SFN and starting onboarding**
- Primary Metrics:
    - Discovery: **# of visits to the top of our funnel** i.e shopify.com/fulfillment (leading)
    - Acceptance: **% of new leads generated starting onboarding** (leading)
    - Expansion: **# total obtainable merchants** (leading)
    - Retention:  **% of merchants exiting SFN app** (lagging & tripwire)

# Tools
## Spin
[FAQ](https://development.shopify.io/engineering/keytech/spin/faq)
[Spin in SFN](https://sfn.docs.shopify.io/Spin/Using-Spin-to-develop-SFN)
- `spin up <repo> [--no-snapshots]` - create instance of <repo> from snapshot (or direct from main with --no-snapshots)
- `spin code` - open instance in vs code
- `spin open` - run instance in browser
- `spin shell` - connect to instance shell
    - run `bundle install` after to setup bundles
- `spin list` - list all open instances
- `spin destroy [<instancename>|--all]` - destroy open instance(s)
- `update` - run in instance after pulling main to stop, update, migrate, restart
- [Connecting to DB](https://development.shopify.io/engineering/keytech/spin/instances#MySQL):
    - From VS Code - SPIN sidebar panel - shopify - db icon OR, the long way...
    - `spin ssh; cd your_project; echo $MYSQL_PORT; exit`
    - `mysql -u root -h $(spin show -o fqdn) -P <MYSQL_PORT>`
        - `GRANT ALL ON *.* TO 'root'@'%';` - grant remote login access
    - on Sequel Ace login with host='shopify.spin_instance.joseph-carino.us.spin.dev', username=root, port=MYSQL_PORT
- Use constellation: `fbs` = 1 onboard, `fbs:no_onboard` = empty, `fbs:part_onboard` = 1 onboard 1 installed

### [Isospin](https://development.shopify.io/engineering/keytech/spin/isospin/isospin)
The OS spin instances run.
- `iso procs list` - returns application names
- `iso procs [stop|start|restart] <applicationName>`

### [Cartridges](https://development.shopify.io/engineering/keytech/spin/isospin/tools#Moving_data_between_your_instances)
- `cartridge create <name>` create new cartridge at ~/.data/cartridges/<name>
- `cartridge save <name>` save <name> to cloud
- `cartridge list` list cartridges. to see inserted just `ls ~/.data/cartridges/`
- `cartridge insert <name>` retrieve <name> from cloud to ~/.data/cartridges/<name>

### [GPG Signing](https://development.shopify.io/engineering/keytech/spin/faq#How_do_I_use_GPG_signing_within_Spin_)
- if "error: gpg failed to sign the data" then killall gpg-agent; gpgconf --launch gpg-agent; spin shell

## Dev
- `dev` - when personal access token expires for local push
- `dev open pr` - open a pr in browser for branch, use from spin
- `dev annotate` - update annotations in models
- `dev g` - update graphql schema

## [lhm](https://github.com/Shopify/lhm)
- `rails generate lhm <Name>` - generate lhm
- `VERSION=<version_num> rake lhm:run` - run lhm migration with specified version number

# Data Analysis
## Mode
- Re-request access to `mode-query` every 30 days from [here](https://clouddo.shopifycloud.com/permits) see also [vault](https://vault.shopify.io/pages/9235-Clouddo-for-Mode-access)

# Links
- [Ruby Style Guide](https://github.com/Shopify/ruby-style-guide)
- [Trifecta Guide](https://github.com/Shopify/trifecta)
- [Polaris Design System](https://polaris.shopify.com/)
- [React Best Practices](https://github.com/Shopify/web-foundations/tree/main/handbook/Best%20Practices/React)
- [React Native API](https://reactnative.dev/docs/)

# Random Tips
- after adding images, run 'bin/optimize-images'
