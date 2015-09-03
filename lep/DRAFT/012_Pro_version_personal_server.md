#LEP 012 - Pro version (personal server)

Date:   September 1, 2015

Status: Draft

Author: uaalto

## Abstract

A proposal for a Lantern Pro version consisting of a paid personal server, automatically launched after payment has been processed. These personal servers should be shareable at will by the paying user, and a mechanism for simplifying this is also proposed.

## Rationale

This LEP stems from the need to accomodate more users within reasonable costs, providing one of the multiple solutions to the problem: diverting users to alternate user-supported servers. The servers could be shared by more than one user, as long as the user agrees. This sharing should be easy in order to facilitate the mechanism and encourage diverse growth approaches for Lantern.


## Proposal

### 1. Payment UI

The process of requesting a personal server should be initiated from the Lantern UI. This wireframe proposes a simple way of including it in current Lantern's UI.

**TODO**


### 2. Send request to payment gateway

Since June 2014, Stripe can accept Alipay payments, which opens the door to offering this service in China. Using Alipay with Strip is straightforward. Its UI is as follows:


** IMage of Stripe/Alipay **

This UI widget will be responsible of sending a request to Stripe/Alipay, identified by a single-use token. This is the sort of code that the UI will require (just HTML):

```
<form action="" method="POST">
  <script
    src="https://checkout.stripe.com/checkout.js" class="stripe-button"
    data-key="pk_test_6pRNASCoBOKtIshFeQd4XMUh"
    data-amount="2000"
    data-name="Demo Site"
    data-description="2 widgets ($20.00)"
    data-image="/128x128.png"
    data-locale="auto">
  </script>
</form>
```

However, this code will not produce the charge from the Lantern client UI, but rather sends a request to validate the data in Stripe, which will in turn send the validated data to a server.

This request will carry also of some data to identify the Lantern instance (lantern id and IP). For this, we can use Stripe request [metadata](https://stripe.com/docs/api#metadata). Ideally, this will also provide geolocation information so the *Launcher server* doesn't have to issue a new geolocation request.


### 3. Launcher server receives confirmation

Once all validation has be done at Stripe's side, it will send a token to Lantern's Launcher server so it can proceed with charging.

At this moment, charging takes place and we can proceed with launching a server. In order to improve user's experience, it is probably best to launch it as soon as the payment has been successfully done.

Stripe's charging API is *synchronous*, so if it succeeds we can act immediately, whithout requiring any callback to check status. As soon as the `charge` call returns, we are good to go.

**Optional**
Should card details be saved? We don't have users at the moment, and since we are working with tokens, it is probably best to keep it simple without users.



### 4. Launching a server

From the Launcher server via:

* An HTTP endpoint in **cloudmaster**.
* Directly launching a server via cloud provider API. Take into account geolocation.


### 5. Automatically configuring the client

1. The user receives IP and token (securely).
1. Overriding config file








## Other features for Pro

When you are Pro, the *get Pro* disappears and new options emerge:

1. Share token

2. Regenerate token

