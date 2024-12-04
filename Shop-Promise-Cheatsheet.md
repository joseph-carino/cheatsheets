[back to overview](/../..)

# Shop Promise Cheatsheet<!-- omit in toc -->

- [Terms](#terms)
- [Criteria](#criteria)
- [Potential issues](#potential-issues)
- [ATC](#atc)
- [Dev](#dev)

## Terms
- `Recall` - % of orders actually delivered in 0-5 days (where we made a prediction)
- `Precision`, `On-Time Delivery`, `OTD` - % of orders (where we made a prediction) that were delivered by our prediction
- `Accuracy` - % of orders where we made a prediction that were delivered exactly as estimated
- `On time transit`, `On time shipping` - Same as precision???
- `Model` vs `Program` - in Program metrics, population is orders we made a prediction on. Model metric population is Shop Promise TAM
- `Delivery Estimate` - When a item will arrive at the buyer's location (AKA "Expected Delivery Dates") (processing time + transit time)
- `Primitive Dates` - Dates shown at checkout that come from shipping rate transit time + processing time from the manual delivery dates page
- `Predicted Dates` - Dates shown at checkout that come from Shop Promise ML models (if less than 5 days, a promise badge will be shown)
- `Branded Promise` - The delivery date gets a brand which indicates a higher level of confidence to the buyer
- `Unbranded Promise` - Only occurs from DPP, just the delivery date, no brand. Indicates confidence in the delivery estimate
- `3P Dates` - 3rd party dates that come from the Delivery Promise Platform (DPP)
- `Flat Rates` - Shipping rates that show transit time it takes for item to be delivered
- `Dynamic Rates` - Shipping rates that are calculated i.e. from Shipify GRC (Generic Rate Cards), BYOA (Bring your own Account, as in with your carrier directly), etc.
- `Precomputed Promise API` - Platform where third party logistics can supply predicted delivery estimates for Shop Promises (formerly Delivery Promise Platform (DPP))
- `Manual Processing Time` - Number set in the Shipping Settings section by a merchant which indicates how long they take to get it to a carrier.
- `Transit Time` - Number of days a carrier takes to delivery the product. This is set within our default shipping rates like "Express (1-2 days)"
- `Shop Promise Settings Page` - Page in the Shop sales channel. displays info about merchant's Shop Promise program and allows Turn on/off
- `OTD` - on time delivery
- `OTS` - on time shipping
- `PDP` - Product Detail Page
- `ODP` - Order Details Page
- `CCR` - Carrier Calculated Rates

## Criteria
- `Meets Criteria` - merchant meets criteria if they pass Base Criteria and Delivery Standards qualifiers
- `Base Criteria` - US fulfillment location, Shop Pay, supported plan, don't use legacy shipping, valid rate names
- `Delivery Standards` - orders with tracking, fulfilling within 12 business hours, affordable shipping, 10% deliver in 5 days or less
- `Turned on/off` - merchant consents to opt-in/opt-out of promises
- `Turned on and meets criteria` -  This Shop will now show Shop Promises in PDP and Checkout if the inputs for those scenarios qualifies (there are many inputs)
- Rates:
  - `Flat rates` - supported on Checkout and PDP
  - `CCR` - not supported on PDP

## Potential issues
- shopPromiseProgram.criteria query - why isn't this a single boolean with a list of issues if false? see [figma](https://www.figma.com/board/N3tj37hYn9FvH7qOzwWDcq/Shop-Promise-Merchant-Experience-Admin-Breakdown?node-id=3-1497&t=14QaMEZjqTHFYa7i-4)
-

## ATC
- Weekly shifts

## Dev
- To setup spin for tophat:
  - run `bin/rake dev:shop_promise:spin_constellation_setup_1p`
  - from `dev c`:
    - `Shop.first.beta.enable(:shop_promise_test_mode_meets_criteria)`
    - `Shop.first.beta.enable(:shop_promise_test_mode_is_1p_model_prediction_accurate_for_display)`
- Clear cache: `bin/rake memcached:clear`
- Checkout card details:
  - Card Number: "1"
  - Card Name: "Test Card"
  - any future date, any CVV
- To display "Fulfill By" on admin: `Shop.first.beta.enable(:display_fulfill_by_features)`
