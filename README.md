# Balatro Duel

A **2-player online, turn-based, Balatro-inspired** competitive card game built in **Unity**. Players score hands to clear **three blinds** and spend surplus chips in a **shared contested shop**. The game uses a **server-authoritative** networking model (capstone version runs as **host-as-server**).

---

## Features
- **Online 1v1**, turn-based (watchable, no simultaneous actions)
- **3 blinds total:** Small → Big → Boss
- **3 Hands + 3 Discards per blind**
- **Shared shop** after Small and Big only
- **Wallet economy** (surplus chips carry over and are spent in the shop)
- **Server-authoritative** rules, turn order, RNG, scoring, and shop state

---

## Rules Summary (MVP)

### Blinds
- The match consists of:
  1. **Small Blind**
  2. **Big Blind**
  3. **Boss Blind** (no shop after this)

### Play Phase (per blind)
Each player starts the blind with:
- `ScoreChips = 0`
- `HandsRemaining = 3`
- `DiscardsRemaining = 3`

Turns alternate. On your turn:
- **Play Hand** → score a poker hand, consume **1 Hand**, end turn  
- **Discard** → discard **1–5 cards**, draw replacements, consume **1 Discard**, end turn  
  - *(No discard + play in the same turn for MVP.)*

Other rules:
- When a player reaches the blind target, they become **Finished** (read-only) and can watch the opponent finish.
- If a player reaches `HandsRemaining = 0` and is still below the target, they **fail and lose the match immediately**.
- When both players finish, the blind ends and **surplus chips** are added to their **Wallet**.

### Win Condition
- If both players clear the **Boss Blind**, the winner is the player with the **higher Wallet**.
- Tie-breakers: **higher Boss surplus**, then **draw**.

---

## Shop Phase (after Small & Big)
A shared, turn-based market inspired by Star Realms:
- **5 market slots**
- First buyer in the first shop is **random**
- First buyer **alternates** each shop
- **Immediate refill** after each purchase
- Players buy **one action per turn**: `Buy` or `Pass`
- Shop ends when both players **pass consecutively**
- **No rubber-banding** (no “behind player buys first” rule)

---

## Networking
- **Server-authoritative:** clients send intents (play/discard/buy/pass), server validates and resolves.
- Capstone target: **Host-as-server** (one player hosts the server + client; the other joins as client).

---

## Tech Stack
- **Unity 2022.3+**
- **Unity Netcode for GameObjects (NGO)**

---

## How to Run
1. Open the project in **Unity**.
2. Enter Play Mode or build the project.
3. One player selects **Host**.
4. The other player selects **Join** and enters the host IP/address.
