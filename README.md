

    // Set your secret key. Remember to switch to your live secret key in production!
    // See your keys here: https://dashboard.stripe.com/apikeys
    Stripe.apiKey = "sk_test_51KR9pNHUhMG4O3VaCbZbJ93DqH5HeLvroq7I6I8Y2d1kvPkeCWMRI3gqrRMbkit5ONDkf5IiZ8ccosJsdV48YcWJ00amlmFtgA";
    post("/webhook", (request, response) -> {
      String payload = request.body();
      String sigHeader = request.headers("Stripe-Signature");

      // If you are testing your webhook locally with the Stripe CLI you
      // can find the endpoint's secret by running `stripe listen`
      // Otherwise, find your endpoint's secret in your webhook settings in the Developer Dashboard
      String endpointSecret = "whsec_...";

      Event event = null;

      // Verify webhook signature and extract the event.
      // See https://stripe.com/docs/webhooks/signatures for more information.
      try {
        event = Webhook.constructEvent(
          payload, sigHeader, endpointSecret
        );
      } catch (JsonSyntaxException e) {
      // Invalid payload.
        response.status(400);
        return "";
      } catch (SignatureVerificationException e) {
      // Invalid Signature.
        response.status(400);
        return "";
      }

      if ("payment_intent.succeeded".equals(event.getType())) {
        // Deserialize the nested object inside the event
        EventDataObjectDeserializer dataObjectDeserializer = event.getDataObjectDeserializer();
        PaymentIntent paymentIntent = null;
        if (dataObjectDeserializer.getObject().isPresent()) {
          PaymentIntent paymentIntent = (PaymentIntent) dataObjectDeserializer.getObject().get();
          String connectedAccountId = event.getAccount();
          handleSuccessfulPaymentIntent(connectedAccountId, paymentIntent);
        } else {
          // Deserialization failed, probably due to an API version mismatch.
          // Refer to the Javadoc documentation on `EventDataObjectDeserializer` for
          // instructions on how to handle this case, or return an error here.
        }
      }

      response.status(200);
      return "";
    });

