As noted in https://github.com/gbv/paia/issues/31 and https://github.com/gbv/paia/issues/36 there should be a way to confirm and/or select before an action. A list of confirmation objects can be used for this purpose:

## confirmation data type

A **confirmation** is a key-value mapping from `type` URIs (indicating the type of confirmation e.g. storage selection, confirmation of fees, special loan conditions...) to key-value objects with the following keys:

* `required (0..1)`: true/false whether the confirmation is mandatory (false by default)
* `multiple (0..1)`: true/false whether multiple options can be selected (false by default)
* `option`(1..n)`: unordered list of options to select from.
* `option.id` (0..1)?`: URI identifying the option. If not given, `option.default` MUST be true or not given.
* `option.amount` (0..1): Optional fee amount. Values matching `/^0+\.00/` MUST be treated equal no amount.
* `option.about` (1..1)?: Textual description or label of the option
* `option.default` (0..1): true/false whether this option is selected by default (if not given: false if option.id is given, true otherwise)

Options with default can be confirmed with `true`. Otherwise an item or multiple items (if multiple=true) can or must be selected by returning the corresponding `option.id`. 

## examples

Confirmation request, sent by the server:

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
  }
}
```

Confirmation sent by the client:

```
{
  "http://example.org/confirm/fee": true,
  "http://example.org/confirm/storage": [
    "http://example.org/storage/desk"
  ],
  "http://example.org/confirm/survey": [
    "http://example.org/purpose/research",
    "http://example.org/purpose/fun"
  ]
}
```

Open questions: 

* Remove field `required` because all confirmations are required (implicitly by default or explicitly otherwise)
* what kind of error to return on missing confirmation?
* how to indicate which confirmations have still required and which are fulfilled
* ...