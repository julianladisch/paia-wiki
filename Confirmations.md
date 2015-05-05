As noted in https://github.com/gbv/paia/issues/31 and https://github.com/gbv/paia/issues/36 there should be a way to confirm and/or select before an action. The [confirmations branch](https://github.com/gbv/paia/tree/confirmations) contains a draft of this feature, details still to be discussed:

http://gbv.github.io/paia/confirmations.html

## User stories

1. user can select alternative pickup location
2. user must select among pickup locations
3. user wants preferred pickup location if possible, any location otherwise
4. user must confirm fee
5. user needs to confirm to use special document for research purpose only
6. user should be noticed about special loan condition

#### client without condition support

1. can request but cannot select location
2. cannot request (error)
3. can request but cannot select location
4. cannot request (error)
5. cannot request (error) 
6. can request but no notice

Client does not send `doc.confirm`, server assumes default value for `doc.confirm`, which is...?

#### client with condition support

1. is noticed about alternative pickup location to select
2. is noticed to select
3. is not noticed, direct request
4. is noticed to confirm
5. is noticed to confirm
6. is noticed to confirm

Client sends with first request to always get noticed except for storage:

    {
        "http://purl.org/ontology/paia#StorageCondition": [
          ...preferred pickup locations...
        ]
    }

Question: what if preferred pickup location is not possible? Will server require another confirm, so user gets notice although does not want?

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
