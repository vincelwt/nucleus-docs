---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
	- shell

toc_footers:
	- <a href='https://nucleus.sh/signup'>Sign Up for a Developer Key</a>
	- <a href='https://github.com/vincelwt/electron-nucleus'>Node module documentation</a>

search: true
---

# Introduction

Welcome to the Nucleus API!
You can use this API to work with licenses and policies.

Something is missing? Would like to see something more? 
-> hello@nucleus.sh


# Authentication

> To authorize, use this code:


```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
	-H "Authorization: your_access_token"
```


> Make sure to replace `your_access_token` with your unique access token available in your [account dashboard](https://nucleus.sh/account).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: your_access_token`

You may also send it in the body of a POST request as the parameter `token`.

<aside class="notice">
You must replace <code>your_access_token</code> with your account's access token.
</aside>

# Policies

A policy defines how a license can be used. For example, it allows you to configure its expiration delay, how many machines the license should work, etc.. and can be used inside your app to separate licenses types.



## Get All Policies

```shell
curl "https://nucleus.sh/app/:appId/policies"
	-H "Authorization: your_access_token"
```

> The above command returns JSON structured like this (as an array):

```json
[
	{
		"id": "basic",
		"duration": "365",
		"maxMachines": "5",
		"version": "0.8.1"
	},
	{
		"id": "lifetime",
		"duration": "0",
		"maxMachines": "0",
		"version": "all"
	}
]
```

This endpoint retrieves all licenses.

### HTTP Request

`GET https://nucleus.sh/app/:appId/policies`




## Create a policy

### HTTP Request

`POST https://nucleus.sh/app/:appId/policy`

### Query Parameters

Parameter | Optional | Description
--------- | ------- | -----------
id | required | The id of the new policy
maxMachines | optional | Maximum number of machines the license will be allowed on (0 for no limits)
versionValidity | optional | On which version should the license be working ('all' )
durationValidity | optional | For how many days the licenses will be valid (0 for always)




## Delete a policy

### HTTP Request

`DELETE https://nucleus.sh/app/:appId/policy/:policyId`



# Licenses


## Get All Licenses

```shell
curl "https://nucleus.sh/app/:appId/licenses"
	-H "Authorization: your_access_token"
```

> The above command returns JSON structured like this:

```json
[
	{
		"id": "license_id",
		"key": "eozUYGifqgoiZefjHDiz",
		"status": "active",
		"userEmail": "user@example.com",
		"policy": "basic",
		"created": "2018-02-13",
		"expire": "0"
	}
]
```

This endpoint retrieves all licenses.

### HTTP Request

`GET https://nucleus.sh/app/:appId/licenses`

<aside class="notice">
You must replace <code>:appId</code> with your application id, available on your <a target='_blank' href='https://nucleus.sh/account'>dashboard</a>.
</aside>




## Create a license

This is the main endpoint. It should be used on your server to create licenses for your users after they paid.

### HTTP Request

`POST https://nucleus.sh/app/:appId/licenses/`

### Query Parameters

Parameter | Optional | Description
--------- | ------- | -----------
policy | required | The policy you want to apply to the license
userEmail | optional | Email of the user who bought the license




## Get a license (and know if it's valid)

```shell
curl "https://nucleus.sh/app/:appId/license/:license"
```

> The above command returns JSON structured like this:

```json
{
	"id": "license_id",
	"key": "eozUYGifqgoiZefjHDiz",
	"status": "active",
	"userEmail": "user@example.com",
	"policy": "basic",
	"created": "2018-02-13",
	"expire": "0"
}
```

This is integrated directly in Nucleus's node module.
This endpoint doesn't need authentification.


### HTTP Request

`GET https://nucleus.sh/app/:appId/license/:license`


<aside class="notice">
`:license` can either be directly the license or its id.
</aside>


## Update a Specific License

For now you can only modify it's `status` property
This can be used to remotely disable a license.

### HTTP Request

`PUT https://nucleus.sh/app/:appId/license/:licenseId`

### Query Parameters

Parameter | Optional | Description
--------- | ------- | -----------
status | optional | Set to `disabled` to disable the license or `active` to reenable it



## Delete a Specific License

Completely delete a license. It won't work anymore and won't show up in your dashboard.
You should prefeer to disable it in case you want to re-use it later.

### HTTP Request

`DELETE https://nucleus.sh/app/:appId/license/:licenseId`