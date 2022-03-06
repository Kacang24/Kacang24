import stripe
charge = stripe.Charge.retrieve(
  "ch_3KRyspHUhMG4O3Va1VRLiqVw",
  api_key="pk_live_51KR9pNHUhMG4O3VamYiVYmh7qnY7Nf2x5oaENOZuuP18t0FVoinvUzB04nRO6ahvwGDDlWmgkvA3WgWnblSRKNb6003D3B5r0V"
)
charge.save() # Uses the same API Key.
stripe.AccountLink.create(
  account = "acct_1KZx9XQd3njzEtbq
  refresh_url = "https://example.com/reauth",
  return_url = "https://example.com/return",
  type = "account_onboarding",
  collect = "eventually_due",
  // Handler for a "Connect Reader" button
function connectReaderHandler() {
  var config = {simulated: true};
  terminal.discoverReaders(config).then(function(discoverResult) {
    if (discoverResult.error) {
      console.log('Failed to discover: ', discoverResult.error);
    } else if (discoverResult.discoveredReaders.length === 0) {
      console.log('No available readers.');
    } else {
      // Just select the first reader here.
      var selectedReader = discoverResult.discoveredReaders[0];

      terminal.connectReader(selectedReader).then(function(connectResult) {
        if (connectResult.error) {
          console.log('Failed to connect: ', connectResult.error);
        } else {
          console.log('Connected to reader: ', connectResult.reader.label);
        }
      });
    }
  });
}
terminal.setSimulatorConfiguration(collectPaymentMethod:5217295357282189)
