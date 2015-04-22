As noted in https://github.com/gbv/paia/issues/31 and https://github.com/gbv/paia/issues/36 there should be a way to confirm and/or select before an action. A list of confirmation objects can be used for this purpose:

## confirmation request

A **confirmation request** (field `doc.confirm`) is a non-empty key-value mapping from `type` URIs (indicating the type of confirmation e.g. storage selection, confirmation of fees, special loan conditions...) to key-value objects with the following keys:

* `multiple (0..1)`: true/false whether multiple options can be selected (false by default)
* `option`(1..n)`: unordered list of options to select from.
* `option.id` (0..1)?`: URI identifying the option. If not given, `option.default` MUST be true or not given.
* `option.amount` (0..1): Optional fee amount. Values matching `/^0+\.00/` MUST be treated equal no amount.
* `option.about` (1..1)?: Textual description or label of the option
* `option.default` (0..1): true/false whether this option is selected by default (if not given: false if option.id is given, true otherwise)

The confirmation request, if given by `doc.confirm`, MUST include all confirmations, including those already confirmed by the client with a confirmation response. It's up to the client how to show multiple confirmations (step-by-step, all-in-one-screen...).

The field `doc.error` MUST also be given if `doc.confirm` exists (for backwards compatibility with clients not aware of confirmations)

## confirmation response

Sent with request parameter `confirm` having one of this values:

* `true` to confirm all defaults values (this is the default for backwards compatibility with clients unaware of confirmations. These clients will not be able to perform actions with required confirmations but just show the document error.
* `false` to not confirm defaults *nor* actions that don't require any confirmation at all (to preview options and possible confirmations)
* A key-value mapping from confirmation types to selected options:
    * options with default can be confirmed with `true`
    * otherwise an item (by its `option.id`) or multiple items (by a list of `option.id` if `multiple` is `true`)

Open questions: 

* how to handle broken confirmation responses, e.g. unknown/invalid identifiers?



All Default values MUST explicitly be confirmed too:

## examples

Confirmation request, sent by the server. Server MUST always send all confirmations, even if some of them have already been confirmed by the client:

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

Confirmation response sent by the client:

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
