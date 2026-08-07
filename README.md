# otp.com Node.js SDK

TypeScript client for the [otp.com](https://otp.com) OTP API: send a one-time password, verify the
code the user entered, resend it on another channel.

Runs on Node 18+ and any runtime with a global `fetch` (Bun, Deno, Cloudflare Workers, browsers).
Ships with full type definitions and no runtime dependencies.

- **API contract:** [otp-com/sdk](https://github.com/otp-com/sdk) (`openapi.yaml`)
- **Method and model reference:** [`docs/`](./docs)
- **Other languages:** [PHP](https://github.com/otp-com/sdk-php) ·
  [Go](https://github.com/otp-com/sdk-go) · [Python](https://github.com/otp-com/sdk-python) ·
  [MCP server](https://github.com/otp-com/mcp)

## Install

```sh
npm install @otp.com/sdk-node
```

## Quickstart

Get an API key from the otp.com panel under **API Keys**. `otp_live_…` sends for real, `otp_test_…`
runs in sandbox. Keep it server-side; it is a bearer credential.

```ts
import { Configuration, OTPApi } from '@otp.com/sdk-node'

const otp = new OTPApi(new Configuration({ accessToken: process.env.OTP_API_KEY! }))

// 1. Send. You pass the recipient; your account routing picks the channel.
const sent = await otp.sendOtp({
  sendRequest: { recipient: '+14155552671', locale: 'en' },
})

sent.otpId            // keep this: you verify against it
sent.channel          // 'sms' | 'whatsapp' | 'email' | 'telegram'
sent.maskedRecipient  // '+14****71', safe to show the user
sent.actionUrl        // WhatsApp only, see below

// 2. Verify whatever the user typed in.
const result = await otp.verifyOtp({
  verifyRequest: { otpId: sent.otpId, code: '123456' },
})

if (result.matched) {
  // The code was correct; result.status is 'approved'.
}
```

The code itself is never returned by the API. `recipient` is a phone number in E.164 or an email
address; which one is valid depends on the channels enabled for your app.

### Retries that must not double-send

Pass an idempotency key and a repeat of the same call replays the first response instead of sending
a second code. Reusing a key with a different body is a `409`.

```ts
await otp.sendOtp({
  sendRequest: { recipient: '+14155552671' },
  idempotencyKey: `signup:${userId}`,
})
```

## WhatsApp: the code comes back to the user

Verification is identical on every channel, but WhatsApp delivery has one extra step. When routing
picks WhatsApp, the code has **not** been sent yet and the response carries an `actionUrl`:

```ts
const sent = await otp.sendOtp({ sendRequest: { recipient } })

if (sent.actionUrl) {
  // Open it for the user. They send us the prefilled message from their own WhatsApp,
  // we reply with the code, and the OTP stays 'pending' until they enter it.
  redirect(sent.actionUrl)
}
```

Then call `verifyOtp` exactly as on SMS. `actionUrl` is `null` on every other channel. Don't poll
for a WhatsApp OTP to approve itself: nothing leaves `pending` without a `verifyOtp` call. If the
user has no WhatsApp, resend on a channel they do have.

## Resending

```ts
// Advance to the next channel in your routing order.
await otp.resendOtp({ resendRequest: { otpId: sent.otpId } })

// Or move it onto a specific channel, e.g. the user has no WhatsApp.
await otp.resendOtp({ resendRequest: { otpId: sent.otpId, channel: 'sms' } })
```

A resend before the cooldown elapses is a `429`; a channel that isn't enabled for your app or the
recipient is a `409`.

## Checking status

```ts
const { status } = await otp.getOtpStatus({ otpId: sent.otpId })
// 'pending' | 'approved' | 'failed' | 'expired'
```

Useful for reconciliation and support tooling. It is not a substitute for `verifyOtp`, which is what
actually approves an OTP.

## Errors

Any non-2xx response throws a `ResponseError` carrying the raw `Response`. The body is always
`{ error: { type, message, details? } }`, where `type` is a stable machine-readable class.

```ts
import { ResponseError } from '@otp.com/sdk-node'

try {
  await otp.sendOtp({ sendRequest: { recipient } })
} catch (err) {
  if (err instanceof ResponseError) {
    const { error } = await err.response.json()
    console.error(err.response.status, error.type, error.message)
  }
  throw err
}
```

| Status | When |
| --- | --- |
| `401` | Missing or invalid API key, disabled app, or suspended company |
| `404` | Unknown `otp_id` (also returned for another company's OTP, to avoid probing) |
| `409` | No enabled channel, channel not enabled, resend not allowed, or idempotency-key conflict |
| `422` | Request body failed validation |
| `429` | Resend cooldown has not elapsed |

Network-level failures throw a `FetchError` instead.

## Configuration

```ts
new Configuration({
  accessToken: process.env.OTP_API_KEY!,        // required
  basePath: 'https://api.otp.com/api/v1',       // default
  headers: { 'X-Request-Id': requestId },       // sent on every request
  fetchApi: myFetch,                            // custom fetch implementation
  middleware: [{ pre: async (ctx) => { /* … */ } }],
})
```

`accessToken` also accepts a function or a promise, which is handy when keys are rotated by a
secret manager.

## API reference

| Method | Endpoint | Returns |
| --- | --- | --- |
| [`sendOtp`](./docs/OTPApi.md#sendotp) | `POST /otp/send` | [`OtpResponse`](./docs/OtpResponse.md) |
| [`verifyOtp`](./docs/OTPApi.md#verifyotp) | `POST /otp/verify` | [`VerifyResponse`](./docs/VerifyResponse.md) |
| [`resendOtp`](./docs/OTPApi.md#resendotp) | `POST /otp/resend` | [`OtpResponse`](./docs/OtpResponse.md) |
| [`getOtpStatus`](./docs/OTPApi.md#getotpstatus) | `GET /otp/{otp_id}` | [`OtpStatusResponse`](./docs/OtpStatusResponse.md) |

## Regenerating

Everything in this repo except this README is generated from
[`openapi.yaml`](https://github.com/otp-com/sdk) by
[OpenAPI Generator](https://openapi-generator.tech). Fix the contract, not `src/`; a pull request
against generated files will be overwritten by the next regeneration.

- **In CI:** run the **Regenerate from spec** workflow, or let `otp-com/sdk` dispatch it.
- **Locally:** `./update-sdk.sh sdk-node` from a checkout of `otp-com/sdk`.

`README.md` is listed in `.openapi-generator-ignore` so it survives regeneration. When the contract
changes, update it by hand.

## License

MIT
