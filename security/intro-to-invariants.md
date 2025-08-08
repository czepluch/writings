# Intro to Invariants

A spacecraft's airlock must never have both doors open simultaneously. The same ticket can't be used to enter the cinema twice. An ether transfer must deduct the balance of the sender and increase the balance of the specified recipient accordingly. These are invariants - fundamental rules that must always hold for systems to work correctly.

Smart contracts need similar rules. When hackers exploit contracts, they often break assumptions developers never explicitly tested. Invariants help you identify and test these assumptions before they become vulnerabilities.

## What Are Invariants?

Invariants are statements about your contract that must remain true regardless of how users interact with it. They capture the essential logic that keeps your contract secure and functional. Think of them as ensuring your business logic. Instead of mapping out all edge cases that could lead to an exploit, you can focus on the core rules that must always hold.

**Local invariants** apply within individual functions. For example, a multiplication function should never overflow, or a division function should never divide by zero.

**Global invariants** span your entire contract's behavior. These ensure that key relationships hold across all possible sequences of function calls.

Consider a simple escrow contract. Some invariants might be:

- Funds can only be released to the intended recipient or refunded to the sender
- The contract balance equals the sum of all active escrows
- Only parties to an escrow can interact with their specific escrow

Or a lending market:

- Total debt never exceeds total deposits
- Users can only borrow against their collateral
- Liquidations only happen when positions are underwater

Or a token contract:

- The sum of all user balances equals the totalSupply
- No individual balance exceeds the totalSupply
- Transfers never create or destroy tokens

## Why Should You Care?

**Catch logic bugs early**. Traditional unit tests check specific scenarios, but invariants validate the fundamental rules across all possible interactions. They catch edge cases you haven’t thought of.

**Document critical assumptions**. Invariants make your contract's security model explicit. Other developers can immediately understand what your contract promises to maintain.

**Guide security reviews**. Auditors can focus on verifying that your code maintains the stated invariants rather than guessing what the contract should do.

**Reduce exploit risk**. Most smart contract exploits violate invariants that developers assumed would hold but never explicitly verified.

The key insight: bugs happen when reality doesn't match your assumptions. Invariants force you to state your assumptions clearly and validate them systematically.

## Additional Benefits of Invariants

Beyond preventing bugs, the process of defining invariants offers several advantages that make your development process more effective:

### Deepen Your Protocol Understanding

When you force yourself to write down what "must always be true," you discover gaps in your own understanding. You might realize that certain operations you thought were simple actually have complex edge cases, or that relationships between different parts of your system are more intricate than you initially designed.

**Example**: While defining invariants for a DEX, you might discover that your price calculation doesn't handle the case where one token has zero supply, or that your fee mechanism could create arbitrage opportunities you hadn't considered.

### Simplify Your Architecture

The exercise of defining invariants often reveals that your protocol is more complex than necessary. When you struggle to express a simple rule in code, it's often a sign that your design can be simplified.

**Example**: If you find yourself writing complex invariants like "The sum of all user balances plus pending transfers minus failed transfers equals total supply," you might realize that your transfer mechanism is overly complicated. A simpler design might eliminate the need for pending/failed states entirely.

### Enable Automated Verification

Once you've clearly defined your invariants, you can leverage powerful automated tools to ensure they always hold:

- **Credible Layer Assertions** can automatically verify that your invariants are upheld for every single transaction to your protocol
- **Fuzzing tools** like Foundry's invariant testing can run millions of random scenarios.
- **Static analysis tools** can catch invariant violations at compile time
- **Formal verification** can mathematically prove that your invariants hold

Without clear invariants, these tools are much less effective because they rely on invariants to define what exactly to validate.

### Improve Team Communication

Invariants serve as a shared language for your team. When discussing new features, you can ask, "Does this maintain our existing invariants?" When reviewing code, you can check "Could this change break any invariants?" This creates a common framework for making decisions.

### Accelerate Onboarding

New team members can understand your protocol's core logic by reading the invariants rather than diving into implementation details. This is especially useful for new developers to understand the main guarantees and logic of the protocol quickly.

### Guide Refactoring Decisions

When you need to change your protocol, invariants act as guardrails. You can ask "Does this refactoring maintain all existing invariants?" and "Which invariants would this change violate?" This prevents you from accidentally breaking core guarantees while improving your code.

### Build Confidence in Deployments

Knowing that your invariants are well-tested and automated gives you confidence when deploying to mainnet, especially if you use a tool like the Phylax Credible Layer to make sure that your invariants are upheld for every single transaction to your protocol.

The time you spend defining invariants isn't just about preventing bugs - it's an investment in understanding, simplifying, and automating your protocol's security model.

## How to Define Your First Invariants

Start by asking three questions about your contract:

### 1. What must never happen?

These become your safety invariants:

- Users accessing funds they don't own
- Unauthorized addresses calling restricted functions
- A flash loan being repaid without paying the fee
- Significant deviation of asset prices within a single block

### 2. What relationships must always hold true?

These become your consistency invariants:

- Accounting relationships (total supply = sum of all balances)
- Collateral ratios (borrowed amount ≤ collateral value × max LTV)
- Pool relationships (reserve0 × reserve1 = k constant in AMM)
- Fee calculations (protocol fees + user fees = total fees collected)

### 3. What guarantees do you make to users?

These become your correctness invariants:

- Deposited funds are always withdrawable by the depositor, even if the contract is paused
- Auction winners receive their items
- Liquidations can only happen when the collateral ratio falls below the minimum threshold
- Voting results reflect actual votes cast

### Practical Exercise

Take your current contract and spend 10 minutes writing down:

- 3 things that should never happen
- 3 relationships between variables that must hold
- 3 promises your contract makes to users

You can use natural language. "Users can't withdraw more than they deposited" is a perfectly valid invariant.

**Pro tip**: AIs like Claude can help brainstorm invariants. Describe your contract and add it as context to the prompt, and ask the AI to define invariants. You'll often discover assumptions you hadn't considered. You might also get completely invalid suggestions, so make sure to stay skeptical and validate the invariants manually.

## From Invariants to Tests

Once you've identified your invariants, you can test them using:

[**Foundry's invariant testing**](https://getfoundry.sh/forge/advanced-testing/invariant-testing) - automatically calls your functions with random inputs and checks that invariants hold:

```solidity
function invariant_userBalanceNeverExceedsTotalDeposits() public view {
    assertLe(vault.deposits(user), vault.totalDeposits());
}
```

**Manual unit tests** - explicitly test invariants in specific scenarios:

```solidity
function testInvariant_AfterDeposit() public {
    vault.deposit(100);
    // Check that all invariants still hold
    assertLe(vault.userBalance(msg.sender), vault.totalDeposits());
}
```

## Enforcing Invariants in Production

While many invariants can be enforced with require statements through CEI and [FREI-PI](https://www.nascent.xyz/idea/youre-writing-require-statements-wrong) (you should read this post directly after this one), some things are not possible or too expensive to check on-chain.

The most promising tool that lets you define and enforce invariants on-chain for every transaction made to your protocol is the [Credible Layer](https://docs.phylax.systems/credible/credible-introduction) by Phylax Systems.

You write assertions in Solidity that express your invariants. These assertions have access to cheatcodes that are not available in regular Solidity. For example, you can fork the state of a transaction at any point in the call frame and check that your invariants hold.

This is useful for detecting intra-block manipulation. For example, if someone finds a way to change ownership of a contract, they could change it back to the initial owner after they've exploited the contract and it would look like nothing happened. With assertions things like this would be detected.

Example of a price manipulation detection assertion:

```solidity
function assertion_priceDeviation() external view {
    Vault assertionAdopter = Vault(ph.getAssertionAdopter());
    // Fork to the pre state of the transaction
    ph.forkTxPre();
    uint256 preOraclePrice = oracle.getPrice(token); // Get the price before the transaction

    // Using swap calls as an example, but any call that affects price should be checked
    PhEvm.CallInputs[] memory calls = ph.getCallInputs(address(assertionAdopter), assertionAdopter.swap.selector);
    for (uint256 i = 0; i < calls.length; i++) {
        // Fork to the post state of each call frame
        ph.forkCallPost(calls[i].id);

        // Get oracle price for a token
        uint256 postOraclePrice = oracle.getPrice(token);

        // Calculate deviation and require change is within range
        uint256 deviation = ((postOraclePrice - preOraclePrice) * 10000) / preOraclePrice;
        require(deviation <= MAX_DEVIATION_BPS,
            "ORACLE_PRICE_MANIPULATION: Token price changed by more than 5% in single transaction"
        );
    }
}
```

## Next Steps

**Start simple**: Pick one contract and identify 3-5 core invariants. Write them in your native language first.

**Make it part of your process**: Before writing code, ask "What invariants must this maintain?" After writing code, ask "Could this break any existing invariants?"

**Build incrementally**: Add invariant testing to your workflow gradually. Document invariants in a README in your smart contract repo, write invariant tests and assertions.

Invariants aren't just about preventing hacks - they're about understanding what your contract promises and ensuring it keeps those promises. Start thinking in terms of "what must always be true" and you'll build more robust smart contracts from day one.
