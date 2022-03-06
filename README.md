
Stripe.api_key = 'pk_live_51KR9pNHUhMG4O3VamYiVYmh7qnY7Nf2x5oaENOZuuP18t0FVoinvUzB04nRO6ahvwGDDlWmgkvA3WgWnblSRKNb6003D3B5r0V'
Stripe::Account.create(type: 'express')
Stripe::AccountLink.create(
  account: 'acct_1KZx9XQd3njzEtbq
',
  refresh_url: 'https://github.com/Kacang24/conncet.refresh',
  return_url: 'https://github.com/Kacang24/conncet.return',
  type: 'account_onboarding',
)
<html>
  <head>
    <title>Checkout</title>
  </head>
  <body>
    <form action="/create-checkout-session" method="POST">
      <button type="submit">Checkout</button>
    </form>
  </body>
</html>
