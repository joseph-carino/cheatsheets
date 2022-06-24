[back to overview](/../..)

# GraphQL Cheatsheet<!-- omit in toc -->

- [Generally](#generally)
- [For Ruby](#for-ruby)
- [Cusco](#cusco)
- [Structure at SFN](#structure-at-sfn)
    - [ROOT Entry Points:](#root-entry-points)
- [Tips](#tips)

# Generally
[Learn](https://graphql.org/learn/queries/)

# For Ruby
[Intro](https://graphql-ruby.org/fields/introduction.html)
[Mutations](https://graphql-ruby.org/mutations/mutation_root.html)

# Cusco
[Wiki](https://cusco.docs.shopify.io/)
[Using](https://development.shopify.io/engineering/developing_at_Shopify/apis/consuming/cusco)
[Repo](https://github.com/Shopify/cusco)

# Structure at SFN
- Each GraphQL entry point has similar structure.
- Scoping is defined by `context` method on graphql_controller.rb
- Auth is defined by graphql_controller.rb
- `graphql_schema_klass` link
- ROOT entry points have schema.rb, all may have schema_helper.rb to stich into ROOTs
-
## ROOT Entry Points:
| Name | Component Path | Scoped By | Auth |
| ------------ | -------------- | --------- | ---------- |
| Merchant App | components/merchant_app/app/public/merchant_app | shop | shop admin |
| Mission Control | components/mission_control/app/public/mission_control | all shops/all fulfillment_orders | internal |
| Fulfillment Providers | components/fulfillment_providers/app/public/fulfillment_providers | fulfillment_center | http |
| Inventory Transfers | components/inventory_transfers/app/public/inventory_transfers | N/A | http |
```
components/<component>/app
└───controllers/<component>
    |   graphql_controller.rb - links to schema.rb through graphql_schema_klass
└───public/<component>/graphql
    |   schema.graphql
    |   schema.json
    |   schema.rb - links (stich) to other component schema_helper.rb for types
    |   schema_helper.rb
```

# Tips
- `String!` means NOT NULLABLE String
-
