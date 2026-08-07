# OTPApi

All URIs are relative to *https://api.otp.com/api/v1*

| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| [**getOtpStatus**](OTPApi.md#getotpstatus) | **GET** /otp/{otp_id} | Get OTP status |
| [**resendOtp**](OTPApi.md#resendotp) | **POST** /otp/resend | Resend an OTP |
| [**sendOtp**](OTPApi.md#sendotp) | **POST** /otp/send | Send an OTP |
| [**verifyOtp**](OTPApi.md#verifyotp) | **POST** /otp/verify | Verify an OTP |



## getOtpStatus

> OtpStatusResponse getOtpStatus(otpId)

Get OTP status

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
| **200** | Current status. |  -  |
| **401** | Missing or invalid API key (also returned for a disabled app or suspended company). |  -  |
| **404** | OTP not found (also returned for another company\&#39;s OTP, to avoid probing). |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#api-endpoints) [[Back to Model list]](../README.md#models) [[Back to README]](../README.md)


## resendOtp

> OtpResponse resendOtp(resendRequest)

Resend an OTP

Resend a pending OTP, advancing to the next configured channel (e.g. SMS to WhatsApp).

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
| **200** | Resend accepted. |  -  |
| **401** | Missing or invalid API key (also returned for a disabled app or suspended company). |  -  |
| **404** | OTP not found (also returned for another company\&#39;s OTP, to avoid probing). |  -  |
| **409** | No enabled channel, channel not enabled, resend not allowed, or an idempotency-key conflict. |  -  |
| **429** | Resend cooldown not elapsed. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#api-endpoints) [[Back to Model list]](../README.md#models) [[Back to README]](../README.md)


## sendOtp

> OtpResponse sendOtp(sendRequest, idempotencyKey)

Send an OTP

Generate a one-time password and deliver it to the recipient. The channel is chosen by your app\&#39;s routing (default order + per-country overrides). Returns an &#x60;otp_id&#x60; to verify against. When routing picks WhatsApp the code is not sent yet: the response carries an &#x60;action_url&#x60; (a wa.me link) the user opens to receive the code over WhatsApp, and the OTP stays pending until they enter it. On every channel the user enters the code and you call &#x60;/otp/verify&#x60;. 

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
    // string | Replays the prior response for the same key; a reused key with a different body is a 409. (optional)
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
| **idempotencyKey** | `string` | Replays the prior response for the same key; a reused key with a different body is a 409. | [Optional] [Defaults to `undefined`] |

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
| **401** | Missing or invalid API key (also returned for a disabled app or suspended company). |  -  |
| **409** | No enabled channel, channel not enabled, resend not allowed, or an idempotency-key conflict. |  -  |
| **422** | Request body failed validation. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#api-endpoints) [[Back to Model list]](../README.md#models) [[Back to README]](../README.md)


## verifyOtp

> VerifyResponse verifyOtp(verifyRequest)

Verify an OTP

Verify the code the user entered. &#x60;matched: true&#x60; means the code was correct.

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
| **401** | Missing or invalid API key (also returned for a disabled app or suspended company). |  -  |
| **404** | OTP not found (also returned for another company\&#39;s OTP, to avoid probing). |  -  |
| **422** | Request body failed validation. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#api-endpoints) [[Back to Model list]](../README.md#models) [[Back to README]](../README.md)

