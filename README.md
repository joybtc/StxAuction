# STX Auction Smart Contract

## Overview
The **STX Auction Smart Contract** is a Clarity-based smart contract that enables users to create and participate in auctions on the Stacks blockchain. The contract allows an auction owner to initialize an auction, accept bids, and conclude or cancel the auction under certain conditions.

## Features
- **Initialize Auction:** The auction owner can start an auction with a specified duration, item name, and reserve price.
- **Submit Bids:** Participants can place bids, which must be higher than the current top bid by a minimum increment.
- **Conclude Auction:** The auction owner can end the auction, transferring funds to the seller if the reserve price is met.
- **Cancel Auction:** The auction owner can cancel the auction if no bids have been placed.
- **Read-Only Queries:** Users can query the auction's status, top bid, top bidder, and other auction details.

## Error Codes
The contract defines several error codes to handle different failure cases:

| Error Code | Description |
|------------|-------------|
| `ERR-NOT-AUTHORIZED (u100)` | Sender is not authorized to perform the action. |
| `ERR-AUCTION-ALREADY-STARTED (u101)` | The auction has already been initialized. |
| `ERR-AUCTION-NOT-STARTED (u102)` | The auction has not started yet. |
| `ERR-AUCTION-ENDED (u103)` | The auction has already ended. |
| `ERR-BID-TOO-LOW (u104)` | The submitted bid is too low. |
| `ERR-TRANSFER-FAILED (u105)` | The STX transfer failed. |
| `ERR-AUCTION-NOT-ENDED (u106)` | The auction has not ended yet. |
| `ERR-NO-BIDS (u107)` | No bids have been placed on the auction. |
| `ERR-INVALID-DURATION (u108)` | The specified auction duration is invalid. |
| `ERR-RESERVE-NOT-MET (u109)` | The reserve price was not met at the auction's conclusion. |
| `ERR-AUCTION-IN-PROGRESS (u110)` | The auction cannot be canceled because bidding has started. |
| `ERR-INVALID-NAME-LENGTH (u111)` | The auction item name is invalid (must be 1-50 characters). |
| `ERR-INVALID-RESERVE-PRICE (u112)` | The reserve price must be greater than zero. |

## Data Variables
The contract stores the following data variables:

| Variable | Type | Description |
|-----------|------|-------------|
| `top-bid` | `uint` | The current highest bid. |
| `top-bidder` | `optional principal` | The address of the highest bidder. |
| `auction-start-block` | `uint` | The block at which the auction started. |
| `auction-end-block` | `uint` | The block at which the auction will end. |
| `owner` | `principal` | The auction owner. |
| `item-name` | `string-ascii 50` | The name of the auctioned item. |
| `reserve-price` | `uint` | The minimum price required for a successful auction. |

## Functions
### 1. Initialize Auction
```clarity
(define-public (initialize-auction (duration uint) (name (string-ascii 50)) (min-price uint))
```
- Initializes a new auction with a specified duration, item name, and minimum price.
- Ensures the sender is the auction owner and that the auction hasn't started already.
- Ensures valid duration and name constraints.

### 2. Submit Bid
```clarity
(define-public (submit-bid (offer uint))
```
- Allows users to submit bids.
- Ensures the auction is active and the bid meets the minimum requirements.
- Transfers STX to the contract and refunds the previous highest bidder.

### 3. Conclude Auction
```clarity
(define-public (conclude-auction)
```
- Ends the auction, transferring the highest bid amount to the auction owner if the reserve price is met.

### 4. Cancel Auction
```clarity
(define-public (cancel-auction)
```
- Cancels the auction if no bids have been placed.
- Only the owner can perform this action.

### 5. Read-Only Queries
```clarity
(define-read-only (query-top-bid)
(define-read-only (query-top-bidder)
(define-read-only (query-auction-end)
(define-read-only (query-item-name)
(define-read-only (query-reserve-price)
(define-read-only (query-auction-status)
```
- These functions allow users to retrieve auction details without modifying blockchain state.

## Constants
- **BID-STEP**: `u1000000` (Minimum bid increment is 1 STX)

## Usage
1. **Deploy the contract** on the Stacks blockchain.
2. **Initialize the auction** by calling `initialize-auction` with the required parameters.
3. **Submit bids** during the auction period.
4. **Conclude the auction** when it ends to transfer funds to the owner.
5. **Cancel the auction** if no bids have been placed.

## License
This smart contract is open-source and available under the MIT license.

