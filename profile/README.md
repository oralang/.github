# 🌐 Ora — A System Language for Smart Contracts

**Ora** is a new language for the EVM, engineered from first principles.

- 🧠 Built for **precision, predictability, and control**
- ⚡ Driven by the core belief: **comptime > runtime**
- 🛡️ Designed to prevent entire classes of bugs through **semantic rigor**
- ✨ Inspired by system languages like Zig and Rust — not by inheritance trees

---

## 🔧 Why Ora?

Modern smart contract languages try to **abstract too much** — hiding costs, mutability, and storage behavior behind object-oriented conventions.

Ora takes the opposite approach:

> We believe **developers should see what’s happening**, reason about it, and **control it at the lowest level necessary**.

Ora is **not** object-oriented. It doesn't come from a traditional academic compiler pipeline. It was born from real engineering pain in production smart contracts — multi-call reentrancy, accidental state corruption, gas inefficiency, and fragile formal tooling.

---

## 🧭 What Makes Ora Different?

### ✅ Comptime > Runtime

Ora tries to **fail as early as possible** — during compilation.

- Mutation guards (`@lock`, `@unlock`) verified statically
- Path-based access analysis
- Region tracking for memory/storage/tstore
- Formal assertions (`requires`, `ensures`, `invariant`) with static validation
- Type inference & validation at compile time, not in the EVM

### 🧠 Precise Semantics

No hidden memory regions. No "magical" fallback. Every variable in Ora has:

| Region   | Mutability | Lifetime          |
|----------|------------|-------------------|
| `stack`  | mutable    | local             |
| `memory` | mutable    | per-transaction   |
| `storage`| mutable    | persistent        |
| `tstore` | mutable    | transient (tx only)|

This gives you **deterministic understanding of state**, and makes it easier to reason about security boundaries.

---

## 🧪 Example: Safe Transfer with Mutation Lock

```ora
pub fn transfer(to: address, amount: u256) {
    @lock(balances[to]);

    balances[msg.sender] -= amount;
    balances[to] += amount;

    log Transfer(msg.sender, to, amount);
}
