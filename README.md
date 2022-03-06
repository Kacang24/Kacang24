import json
import os
import stripe

from flask import Flask, jsonify, request

# This is your Stripe CLI webhook secret for testing your endpoint locally.
endpoint_secret = 'whsec_b3d7059f76397c2401e340fc3382b9bd76cbc5263458e8214083acd195176beb'

app = Flask(__name__)

@app.route('/webhook', methods=['POST'])
def webhook():
    event = None
    payload = request.data
    sig_header = request.headers['STRIPE_SIGNATURE']

    try:
        event = stripe.Webhook.construct_event(
            payload, sig_header, endpoint_secret
        )
    except ValueError as e:
        # Invalid payload
        raise e
    except stripe.error.SignatureVerificationError as e:
        # Invalid signature
        raise e

    # Handle the event
    print('Unhandled event type {}'.format(event['type']))

    return jsonify(success=True)
