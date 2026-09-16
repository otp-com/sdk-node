
# VerificationExchangeResponse


## Properties

Name | Type
------------ | -------------
`otpId` | string
`recipient` | string
`recipientType` | [RecipientType](RecipientType.md)
`channel` | [Channel](Channel.md)
`verifiedAt` | Date

## Example

```typescript
import type { VerificationExchangeResponse } from '@otp.com/sdk-node'

// TODO: Update the object below with actual values
const example = {
  "otpId": null,
  "recipient": +14155552671,
  "recipientType": null,
  "channel": null,
  "verifiedAt": null,
} satisfies VerificationExchangeResponse

console.log(example)

// Convert the instance to a JSON string
const exampleJSON: string = JSON.stringify(example)
console.log(exampleJSON)

// Parse the JSON string back to an object
const exampleParsed = JSON.parse(exampleJSON) as VerificationExchangeResponse
console.log(exampleParsed)
```

[[Back to top]](#) [[Back to API list]](../README.md#api-endpoints) [[Back to Model list]](../README.md#models) [[Back to README]](../README.md)


