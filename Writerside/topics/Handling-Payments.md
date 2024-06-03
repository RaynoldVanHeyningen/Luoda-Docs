# Handling Payments

For users to be able to use Luoda, they need credits. The way to get credits is via subscription.
We handle subscriptions through stripe, we offer 2 plans:
Basic Plan
Premium Plan

Basic gives the user 1000 credits per month, and premium gives the user 2500 credits per month.

In this tutorial, you will learn how to:
* Build a pricing page
* Build a checkout button
* Build a success page
* Build a customer portal button
* Build a canceled page

## Before you start

List the prerequisites that are required or recommended.

Make sure that:
- First prerequisite
- Second prerequisite

## Add a pricing page

Add a page to your site to display your product and offer your customers an option to subscribe to it. When they click the checkout button, they’re redirected to a Stripe-hosted Checkout page. The order is finalized when your customer redirects to the Checkout page—they can’t modify it after that point.

You can also create a pricing table to displace your pricing information through the Dashboard. Copy the generated code and embed it in your site. When your customer clicks a pricing option, they’re redirected to the checkout page.

<code-block lang="html">
<![CDATA[
<!DOCTYPE html>
<html>
   <head>
      <title>Subscribe to a cool new product</title>
      <link rel="stylesheet" href="style.css" />
      <script src="https://js.stripe.com/v3/"></script>
   </head>
   <body>
   <section>
      <div class="product">
         <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="14px" height="16px" viewBox="0 0 14 16" version="1.1">
            <defs/>
            <g id="Flow" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd">
               <g id="0-Default" transform="translate(-121.000000, -40.000000)" fill="#E184DF">
                  <path d="M127,50 L126,50 C123.238576,50 121,47.7614237 121,45 C121,42.2385763 123.238576,40 126,40 L135,40 L135,56 L133,56 L133,42 L129,42 L129,56 L127,56 L127,50 Z M127,48 L127,42 L126,42 C124.343146,42 123,43.3431458 123,45 C123,46.6568542 124.343146,48 126,48 L127,48 Z" id="Pilcrow"/>
               </g>
            </g>
         </svg>
         <div class="description">
            <h3>Starter plan</h3>
            <h5>$20.00 / month</h5>
         </div>
      </div>
   </section>
   </body>
</html>
]]>
</code-block>

## Add a checkout

Add a button to your order preview page. Clicking this button redirects your customer to the Stripe-hosted Checkout page. The lookup_key was added when you created the product and price in the first step. When the form is submitted it is used to retrieve the price_id on the server.
<code-block lang="html" >
<![CDATA[
<!DOCTYPE html>
<html>
   <head>
      <title>Subscribe to a cool new product</title>
      <link rel="stylesheet" href="style.css" />
      <script src="https://js.stripe.com/v3/"></script>
   </head>
   <body>
   <section>
      <div class="product">
         <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="14px" height="16px" viewBox="0 0 14 16" version="1.1">
            <defs/>
            <g id="Flow" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd">
               <g id="0-Default" transform="translate(-121.000000, -40.000000)" fill="#E184DF">
                  <path d="M127,50 L126,50 C123.238576,50 121,47.7614237 121,45 C121,42.2385763 123.238576,40 126,40 L135,40 L135,56 L133,56 L133,42 L129,42 L129,56 L127,56 L127,50 Z M127,48 L127,42 L126,42 C124.343146,42 123,43.3431458 123,45 C123,46.6568542 124.343146,48 126,48 L127,48 Z" id="Pilcrow"/>
               </g>
            </g>
         </svg>
         <div class="description">
            <h3>Starter plan</h3>
            <h5>$20.00 / month</h5>
         </div>
      </div>
      <form action="/create-checkout-session" method="POST">
        <!-- Add a hidden field with the lookup_key of your Price -->
        <input type="hidden" name="lookup_key" value="{{PRICE_LOOKUP_KEY}}" />
        <button id="checkout-and-portal-button" type="submit">Checkout</button>
      </form>
   </section>
   </body>
</html>
]]>
</code-block>

## Add a success page

Create a success page for the URL you provided as the Checkout Session success_url to display order confirmation messaging or order details to your customer. Stripe redirects to this page after the customer successfully completes the checkout.
<code-block lang="html" >
<![CDATA[
<!DOCTYPE html>
<html>
<head>
  <title>Thanks for your order!</title>
  <link rel="stylesheet" href="style.css">
  <script src="client.js" defer></script>
</head>
<body>
  <section>
    <div class="product Box-root">
      <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="14px" height="16px" viewBox="0 0 14 16" version="1.1">
          <defs/>
          <g id="Flow" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd">
              <g id="0-Default" transform="translate(-121.000000, -40.000000)" fill="#E184DF">
                  <path d="M127,50 L126,50 C123.238576,50 121,47.7614237 121,45 C121,42.2385763 123.238576,40 126,40 L135,40 L135,56 L133,56 L133,42 L129,42 L129,56 L127,56 L127,50 Z M127,48 L127,42 L126,42 C124.343146,42 123,43.3431458 123,45 C123,46.6568542 124.343146,48 126,48 L127,48 Z" id="Pilcrow"/>
              </g>
          </g>
      </svg>
      <div class="description Box-root">
        <h3>Subscription to Starter plan successful!</h3>
      </div>
    </div>
  </section>
</body>
</html>
]]>
</code-block>

## Add a customer portal button

Add a button to redirect to the customer portal to allow customers to manage their subscription. Clicking this button redirects your customer to the Stripe-hosted customer portal page.
<code-block lang="html" >
<![CDATA[
<!DOCTYPE html>
<html>
<head>
  <title>Thanks for your order!</title>
  <link rel="stylesheet" href="style.css">
  <script src="client.js" defer></script>
</head>
<body>
  <section>
    <div class="product Box-root">
      <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="14px" height="16px" viewBox="0 0 14 16" version="1.1">
          <defs/>
          <g id="Flow" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd">
              <g id="0-Default" transform="translate(-121.000000, -40.000000)" fill="#E184DF">
                  <path d="M127,50 L126,50 C123.238576,50 121,47.7614237 121,45 C121,42.2385763 123.238576,40 126,40 L135,40 L135,56 L133,56 L133,42 L129,42 L129,56 L127,56 L127,50 Z M127,48 L127,42 L126,42 C124.343146,42 123,43.3431458 123,45 C123,46.6568542 124.343146,48 126,48 L127,48 Z" id="Pilcrow"/>
              </g>
          </g>
      </svg>
      <div class="description Box-root">
        <h3>Subscription to Starter plan successful!</h3>
      </div>
    </div>
    <form action="/create-portal-session" method="POST">
      <input type="hidden" id="session-id" name="session_id" value="" />
      <button id="checkout-and-portal-button" type="submit">Manage your billing information</button>
    </form>
  </section>
</body>
</html>
]]>
</code-block>

## Add a canceled page

Add another page for the cancel_url. Stripe redirects to this page when the customer clicks the back button in Checkout.
<code-block lang="html" >
<![CDATA[
<!DOCTYPE html>
<html>
<head>
  <title>Checkout canceled</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <section>
    <p>Picked the wrong subscription? Shop around then come back to pay!</p>
  </section>
</body>
</html>
]]>
</code-block>

## client.js

In the success page we are using some javascript code to get the stripe session id:

```Javascript
// In production, this should check CSRF, and not pass the session ID.
// The customer ID for the portal should be pulled from the
// authenticated user on the server.
document.addEventListener('DOMContentLoaded', async () => {
  let searchParams = new URLSearchParams(window.location.search);
  if (searchParams.has('session_id')) {
    const session_id = searchParams.get('session_id');
    document.getElementById('session-id').setAttribute('value', session_id);
  }
});
```