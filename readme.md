# Group Purchase Smart Contract

This repository contains a **Group Purchase smart contract** written in NexScript. The contract facilitates a decentralized group purchasing mechanism where the price of a product decreases as more participants join, with a minimum price limit. The contract manages participant payments, refunds, and final payments to the vendor.

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Functions](#functions)
- [How It Works](#how-it-works)
- [Usage](#usage)
  - [Participating in a Group Purchase](#participating-in-a-group-purchase)
  - [Finalizing the Purchase](#finalizing-the-purchase)
  - [Requesting a Refund](#requesting-a-refund)
- [License](#license)

## Overview

The **Group Purchase Smart Contract** allows multiple participants to purchase a product at a potentially reduced price, depending on the number of participants. The contract sets a minimum number of participants required to finalize the purchase and a price per participant that decreases with each new buyer, but it cannot fall below a predefined minimum price.

Once the required number of participants is met, the contract allows the vendor to finalize the purchase, transferring the collected funds to the vendor. If participants overpay due to a price reduction, they can request a refund for the excess amount.

## Features

- **Dynamic Pricing**: The price decreases with each new participant until a minimum price is reached.
- **Group Participation**: The purchase requires a minimum number of participants to be successful.
- **Finalization**: The vendor can finalize the purchase once the minimum number of participants is reached.
- **Refund Mechanism**: Participants can request a refund if they paid more than the final price.
- **Transaction Security**: The contract ensures that funds are transferred correctly to the vendor and that participants are refunded any overpayment.

## Functions

### Constructor (`GroupPurchase`)
Initializes the contract with the following parameters:
- `vendorPk`: Public key of the vendor.
- `minPrice`: Minimum price per participant.
- `priceDecreasePerSale`: Amount by which the price decreases with each new participant.
- `minimumParticipants`: Minimum number of participants required to finalize the purchase.
- `totalParticipants`: Tracks the current number of participants.
- `purchaseCompleted`: Boolean flag to track whether the purchase has been completed.

### `participate(pubkey participantPk, sig participantSig)`
- Allows participants to join the group purchase by submitting their public key and signature.
- Verifies the signature and checks if the payment is sufficient for the current price.
- Updates the participant count and, if necessary, refunds any excess payment.

### `finalizePurchase(sig vendorSig)`
- Finalizes the group purchase, ensuring that the minimum number of participants has been reached.
- Transfers the total funds collected to the vendor.
- Marks the purchase as complete.

### `refund(pubkey participantPk, sig participantSig)`
- Allows participants to request a refund if they overpaid relative to the final price.
- Verifies the participant's payment and signature.
- Refunds the difference between the amount paid and the final price.

## How It Works

1. **Dynamic Pricing**  
   The product starts at a base price. As more participants join, the price decreases according to a set decrement value (`priceDecreasePerSale`). The price, however, cannot go below a specified `minPrice`.

2. **Participation**  
   Participants join by calling the `participate()` function, sending the required funds for the current price. If a participant pays more than the current price, the excess funds are refunded immediately.

3. **Finalizing the Purchase**  
   Once the minimum number of participants is met, the vendor can finalize the purchase by calling `finalizePurchase()`, which transfers the total collected funds to the vendor and marks the purchase as complete. No further participation is allowed after this.

4. **Refund**  
   If a participant paid more than the final price, they can request a refund for the excess amount by calling the `refund()` function.

## Usage

### 1. Participating in a Group Purchase

To participate in a group purchase, a participant must call the `participate` function with their public key and signature.

```nexscript
participate(
    participantPk = 0xabcdef01...,   // Participant's public key
    participantSig = sig_A           // Signature of the participant
)
```
The function will calculate the current price, verify the payment, and update the total participant count. If the participant overpays, the excess will be refunded automatically.

### 2. Finalizing the Purchase
Once the required number of participants has been reached, the vendor can call the finalizePurchase function to finalize the group purchase.

```nexscript
finalizePurchase(
    vendorSig = sig_vendor          // Vendor's signature
)
```

### 3. Requesting a Refund
If a participant paid more than the final price, they can request a refund by calling the refund function with their public key and signature.

```nexscript
refund(
    participantPk = 0xabcdef01...,   // Participant's public key
    participantSig = sig_A           // Signature of the participant
)
```
This will return the excess payment based on the final price and the amount the participant initially paid.

## License
This project is licensed under the MIT License.
