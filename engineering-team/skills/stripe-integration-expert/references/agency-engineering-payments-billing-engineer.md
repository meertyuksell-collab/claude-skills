# Payments & Billing Engineer — extended playbook

> Merged from [The Agency](https://github.com/msitarzewski/agency-agents) agent `engineering/engineering-payments-billing-engineer.md` (MIT License, © AgentLand Contributors).
> Only sections that add something this skill did not already cover are kept; persona, generic metrics and duplicate material were removed.

## 🚨 Critical Rules You Must Follow


1. **Never touch raw card data.** Card numbers go from the customer's browser to the processor via hosted fields or SDK tokenization. If a PAN can reach your server, the design is wrong — that is the difference between SAQ A and a full PCI DSS audit.
2. **Every mutation carries an idempotency key.** Charges, refunds, and subscription changes must be safely retryable. Derive the key from the business operation (order ID + attempt), not from a random UUID per HTTP call.
3. **Webhooks are the source of truth, not the redirect.** Fulfill on `payment_intent.succeeded` (or the PSP equivalent), never on the customer returning to your success page. Customers close tabs; webhooks don't.
4. **Verify signatures and persist recoverable work.** Reject unsigned or stale webhook payloads; durably store verified events before acknowledgment. Deduplicate acceptance by event ID, mark completion only after successful processing, and make side effects safe to replay after a worker crash.
5. **Store money as integers in minor units.** Amounts are `4999` cents with an ISO 4217 currency code — never floats, and never a bare number without its currency. Beware zero-decimal currencies like JPY.
6. **Model every state, especially the unhappy ones.** `requires_action` (3DS), `processing`, partial refunds, disputes, and failed dunning retries are normal operating states, not edge cases to log-and-ignore.
7. **Reconcile before you celebrate.** A green test suite proves the code path; only a payout-to-ledger reconciliation proves the money. Automate it daily and alert on any drift.
8. **Test the failure catalog.** Every PSP publishes test cards for declines, insufficient funds, 3DS challenges, and disputes. A payment integration tested only with the success card is untested.

### Idempotent Payment Creation (TypeScript + Stripe)


```typescript
// The idempotency key is derived from the business operation, so a client
// retry, a server retry, and a double-click all resolve to the same charge.
import Stripe from 'stripe';

const stripe = new Stripe(process.env.STRIPE_SECRET_KEY!, { apiVersion: '2024-06-20' });

export async function createPaymentForOrder(order: Order): Promise<Stripe.PaymentIntent> {
  return stripe.paymentIntents.create(
    {
      amount: order.totalMinorUnits,          // integer cents — never floats
      currency: order.currency,               // ISO 4217, lowercase
      customer: order.stripeCustomerId,
      metadata: { order_id: order.id },       // always link PSP objects back to your domain
      automatic_payment_methods: { enabled: true },
    },
    { idempotencyKey: `order-${order.id}-attempt-${order.paymentAttempt}` }
  );
}
```

### Daily Reconciliation Query


```sql
-- Every processor payout must equal the sum of our ledger entries for that payout.
-- Any nonzero drift is an incident, not a curiosity.
SELECT
  p.payout_id,
  p.arrival_date,
  p.amount_minor                             AS processor_amount,
  COALESCE(SUM(l.amount_minor), 0)           AS ledger_amount,
  p.amount_minor - COALESCE(SUM(l.amount_minor), 0) AS drift
FROM processor_payouts p
LEFT JOIN ledger_entries l ON l.payout_id = p.payout_id
GROUP BY p.payout_id, p.arrival_date, p.amount_minor
HAVING p.amount_minor <> COALESCE(SUM(l.amount_minor), 0)
ORDER BY p.arrival_date DESC;
```

### PCI Scope Cheat Sheet


| Integration style | PCI validation | Rule of thumb |
|-------------------|---------------|----------------|
| Hosted checkout page (Stripe Checkout, PayPal redirect) | SAQ A | Card data never touches your pages — smallest scope, default choice |
| Embedded iframe fields (Stripe Elements, Adyen Drop-in) | SAQ A | Your page hosts the iframe; the PSP hosts the inputs |
| Your form posts card data via PSP JS (legacy direct-post) | SAQ A-EP | Your page can be attacked — avoid for new builds |
| Card data touches your servers | SAQ D / full audit | Almost never justified — redesign |

