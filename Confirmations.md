As noted in https://github.com/gbv/paia/issues/31 and https://github.com/gbv/paia/issues/36 there should be a way to confirm and/or select before an action. The [confirmations branch](https://github.com/gbv/paia/tree/confirmations) contains a draft of this feature, details still to be discussed:

http://gbv.github.io/paia/confirmations.html

## User stories

...

## Additional examples

Condition sent by the server:

```json
{
  "http://example.org/confirm/research": {
    "multiple": false,
    "option": [
      {
        "about": "Only for adults, please confirm!"
      }
    ]
  },
  "http://example.org/confirm/survey": {
    "multiple": true,
    "option": [
      {
        "id": "http://example.org/purpose/research"
        "about": "For research purpose",
        "amount": "0 EUR"
      },
      {
        "id": "http://example.org/purpose/fun"
        "about": "Just for fun",
        "amount": "0 EUR"
      }
    ]
  }
}
```
