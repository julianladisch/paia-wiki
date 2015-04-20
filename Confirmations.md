As noted in https://github.com/gbv/paia/issues/31 and https://github.com/gbv/paia/issues/36 there should be a way to confirm and/or select before an action. A list of confirmation objects can be used for this purpose:

## confirmation data type

A **confirmation** is a key-value mapping from `type` URIs (indicating the type of confirmation e.g. storage selection, confirmation of fees, special loan conditions...) to key-value objects with the following keys:

* `required (1..1)`: true/false whether the confirmation is mandatory 
* `multiple (1..1)`: true/false whether multiple options can be selected
* `option`(0..n)`: unordered list of options to select from. A one-item list can be confirmed with "yes". A longer list requires or suggests to select an item from (e.g. a storage)
* `option.id` (0..1)?`: URI identifying the option
* `option.amount` (1..1): Fee amount. Can be zero if no fee is expected.
* `option.about` (1..1)?: Textual description or label of the option
* `option.default`(0..1): true/false whether this option is selected by default

Without `option.id` the option is always a default.

## examples

```json
{
  "http://example.org/confirm/fee": {
    "required": true,
    "multiple": false,
    "option": [
      {
        "about": "This loan request is not for free!",
        "amount": "5 EUR"
      }
    ]
  },
  "http://example.org/confirm/storage": {
    "required": false,
    "option": [
      { 
        "id": "http://example.org/storage/pickup",
        "about": "pickup room",
        "amount": "0 EUR",
        "default": true
      },
      { 
        "id": "http://example.org/storage/desk",
        "about": "lending desk",
        "amount": "0 EUR",
        "default": false
      }, {
        "id": "http://example.org/storage/home",
        "about": "Home delivery (shipping)",
        "amount": "5 EUR",
        "default": false
      }
    ]
  },
  "http://example.org/confirm/research": {
    "required": true,
    "multiple": false,
    "option": [
      {
        "about": "Only for adults, please confirm!",
        "amount": "0 EUR"
      }
    ]
  },
  "http://example.org/confirm/survey": {
    "required": false,
    "multiple": true,
    "option": [
      {
        "id": "http://example.org/purpose/research",
        "about": "For research purpose",
        "amount": "0 EUR"
      },
      {
        "id": "http://example.org/purpose/fun",
        "about": "Just for fun",
        "amount": "0 EUR"
      }
    ]
  },

}
```
