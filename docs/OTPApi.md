# OTPApi

All URIs are relative to *https://api.otp.com*

| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| [**getOtpStatus**](OTPApi.md#getotpstatus) | **GET** /api/v1/otp/{otp_id} | Fetch the current status of an OTP. |
| [**resendOtp**](OTPApi.md#resendotp) | **POST** /api/v1/otp/resend | Resend a pending OTP, escalating the channel if configured. |
| [**sendOtp**](OTPApi.md#sendotp) | **POST** /api/v1/otp/send | Start an OTP: routes a channel and dispatches the code. |
| [**verifyOtp**](OTPApi.md#verifyotp) | **POST** /api/v1/otp/verify | Verify a code against a pending OTP. |



## getOtpStatus

> OtpStatusResponse getOtpStatus(otpId)

Fetch the current status of an OTP.

### Example

```ts
import {
  Configuration,
  OTPApi,
} from '@otp.com/sdk-node';
import type { GetOtpStatusRequest } from '@otp.com/sdk-node';

async function example() {
  console.log("🚀 Testing @otp.com/sdk-node SDK...");
  const config = new Configuration({ 
    // Configure HTTP bearer authorization: bearerAuth
    accessToken: "YOUR BEARER TOKEN",
  });
  const api = new OTPApi(config);

  const body = {
    // string
    otpId: 38400000-8cf0-11bd-b23e-10b96e4ef00d,
  } satisfies GetOtpStatusRequest;

  try {
    const data = await api.getOtpStatus(body);
    console.log(data);
  } catch (error) {
    console.error(error);
  }
}

// Run the test
example().catch(console.error);
```

### Parameters


| Name | Type | Description  | Notes |
|------------- | ------------- | ------------- | -------------|
| **otpId** | `string` |  | [Defaults to `undefined`] |

### Return type

[**OtpStatusResponse**](OtpStatusResponse.md)

### Authorization

[bearerAuth](../README.md#bearerAuth)

### HTTP request headers

- **Content-Type**: Not defined
- **Accept**: `application/json`


### HTTP response details
| Status code | Description | Response headers |
|-------------|-------------|------------------|
| **200** | Current status of the OTP. |  -  |
| **400** | otp_id is not a valid UUID. |  -  |
| **401** | Missing or invalid API key (also returned for a disabled app or a suspended company). |  -  |
| **404** | OTP not found (the same 404 is returned for an OTP belonging to another company). |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#api-endpoints) [[Back to Model list]](../README.md#models) [[Back to README]](../README.md)


## resendOtp

> OtpResponse resendOtp(resendRequest)

Resend a pending OTP, escalating the channel if configured.

### Example

```ts
import {
  Configuration,
  OTPApi,
} from '@otp.com/sdk-node';
import type { ResendOtpRequest } from '@otp.com/sdk-node';

async function example() {
  console.log("🚀 Testing @otp.com/sdk-node SDK...");
  const config = new Configuration({ 
    // Configure HTTP bearer authorization: bearerAuth
    accessToken: "YOUR BEARER TOKEN",
  });
  const api = new OTPApi(config);

  const body = {
    // ResendRequest
    resendRequest: ...,
  } satisfies ResendOtpRequest;

  try {
    const data = await api.resendOtp(body);
    console.log(data);
  } catch (error) {
    console.error(error);
  }
}

// Run the test
example().catch(console.error);
```

### Parameters


| Name | Type | Description  | Notes |
|------------- | ------------- | ------------- | -------------|
| **resendRequest** | [ResendRequest](ResendRequest.md) |  | |

### Return type

[**OtpResponse**](OtpResponse.md)

### Authorization

[bearerAuth](../README.md#bearerAuth)

### HTTP request headers

- **Content-Type**: `application/json`
- **Accept**: `application/json`


### HTTP response details
| Status code | Description | Response headers |
|-------------|-------------|------------------|
| **200** | Resend accepted; the OTP may now be on a different channel. |  -  |
| **401** | Missing or invalid API key (also returned for a disabled app or a suspended company). |  -  |
| **404** | OTP not found (the same 404 is returned for an OTP belonging to another company). |  -  |
| **409** | The OTP cannot be resent (resolved, expired, or out of attempts), or the requested channel is not enabled. |  -  |
| **422** | Request body failed validation. |  -  |
| **429** | Resend cooldown has not elapsed; see the Retry-After header. |  -  |
| **503** | Routing picked WhatsApp but our inbound number is not configured. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#api-endpoints) [[Back to Model list]](../README.md#models) [[Back to README]](../README.md)


## sendOtp

> OtpResponse sendOtp(sendRequest, idempotencyKey)

Start an OTP: routes a channel and dispatches the code.

Routing picks the channel from the app config. When it selects WhatsApp the code is not sent yet: the response returns action_url (a wa.me link) the user opens to receive the code over WhatsApp, and the OTP stays pending until they enter it. On all other channels the code is delivered directly and action_url is null. Either way the user enters the code and you call POST /otp/verify.

### Example

```ts
import {
  Configuration,
  OTPApi,
} from '@otp.com/sdk-node';
import type { SendOtpRequest } from '@otp.com/sdk-node';

async function example() {
  console.log("🚀 Testing @otp.com/sdk-node SDK...");
  const config = new Configuration({ 
    // Configure HTTP bearer authorization: bearerAuth
    accessToken: "YOUR BEARER TOKEN",
  });
  const api = new OTPApi(config);

  const body = {
    // SendRequest
    sendRequest: ...,
    // string | Replay the prior response for a repeated request; a reused key with a different body is a 409. (optional)
    idempotencyKey: idempotencyKey_example,
  } satisfies SendOtpRequest;

  try {
    const data = await api.sendOtp(body);
    console.log(data);
  } catch (error) {
    console.error(error);
  }
}

// Run the test
example().catch(console.error);
```

### Parameters


| Name | Type | Description  | Notes |
|------------- | ------------- | ------------- | -------------|
| **sendRequest** | [SendRequest](SendRequest.md) |  | |
| **idempotencyKey** | `string` | Replay the prior response for a repeated request; a reused key with a different body is a 409. | [Optional] [Defaults to `undefined`] |

### Return type

[**OtpResponse**](OtpResponse.md)

### Authorization

[bearerAuth](../README.md#bearerAuth)

### HTTP request headers

- **Content-Type**: `application/json`
- **Accept**: `application/json`


### HTTP response details
| Status code | Description | Response headers |
|-------------|-------------|------------------|
| **201** | OTP created and delivery started. |  -  |
| **401** | Missing or invalid API key (also returned for a disabled app or a suspended company). |  -  |
| **409** | No channel can reach this recipient, or the idempotency key was reused with a different body. |  -  |
| **422** | Request body failed validation. |  -  |
| **503** | Routing picked WhatsApp but our inbound number is not configured. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#api-endpoints) [[Back to Model list]](../README.md#models) [[Back to README]](../README.md)


## verifyOtp

> VerifyResponse verifyOtp(verifyRequest)

Verify a code against a pending OTP.

### Example

```ts
import {
  Configuration,
  OTPApi,
} from '@otp.com/sdk-node';
import type { VerifyOtpRequest } from '@otp.com/sdk-node';

async function example() {
  console.log("🚀 Testing @otp.com/sdk-node SDK...");
  const config = new Configuration({ 
    // Configure HTTP bearer authorization: bearerAuth
    accessToken: "YOUR BEARER TOKEN",
  });
  const api = new OTPApi(config);

  const body = {
    // VerifyRequest
    verifyRequest: ...,
  } satisfies VerifyOtpRequest;

  try {
    const data = await api.verifyOtp(body);
    console.log(data);
  } catch (error) {
    console.error(error);
  }
}

// Run the test
example().catch(console.error);
```

### Parameters


| Name | Type | Description  | Notes |
|------------- | ------------- | ------------- | -------------|
| **verifyRequest** | [VerifyRequest](VerifyRequest.md) |  | |

### Return type

[**VerifyResponse**](VerifyResponse.md)

### Authorization

[bearerAuth](../README.md#bearerAuth)

### HTTP request headers

- **Content-Type**: `application/json`
- **Accept**: `application/json`


### HTTP response details
| Status code | Description | Response headers |
|-------------|-------------|------------------|
| **200** | Verification result. |  -  |
| **401** | Missing or invalid API key (also returned for a disabled app or a suspended company). |  -  |
| **404** | OTP not found (the same 404 is returned for an OTP belonging to another company). |  -  |
| **422** | Request body failed validation. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#api-endpoints) [[Back to Model list]](../README.md#models) [[Back to README]](../README.md)

