# Christmas Raffle (Weihnachts-Wichteln)

A static Secret Santa web page hosted on GitHub Pages for our family gift exchange. Each family member enters their first name to discover who they will be getting a present for this Christmas.

---

## 👥 Family Members

The raffle pool consists of the following 5 members:
- **Enrico**
- **Nadine**
- **Sarah**
- **Ricci**
- **Diana**

---

## 📜 Reshuffling Rules

When reshuffling or drawing a new gift assignment for a new year, the following rules must be strictly adhered to:

1. **No Self-Assignments (Derangement)**:
   - No one may draw themselves ($A \neq B$).
2. **Exact Bijective Coverage (1-to-1 Mapping)**:
   - Every participant gives exactly one present.
   - Every participant receives exactly one present.
   - The set of givers and the set of receivers must both match the full family member list.
3. **No Partner / Couple Exclusions**:
   - There are no special partner or couple exclusions; any person can draw any other person in the pool.
4. **Any Valid Derangement Allowed**:
   - Mutual 2-person swaps (e.g. $A \to B$ and $B \to A$) as well as larger cycles (e.g. 3-cycles, 5-cycles) are completely acceptable.
5. **Year-over-Year History**:
   - Individual repeat pairings from previous years are permitted.
   - **Exception**: The *entire set of assignments* must not be 100% identical to the immediately preceding year (i.e. if every person ends up with the exact same recipient as last year, redraw).
6. **Strict Confidentiality**:
   - **Never reveal who is assigned to whom** in plaintext anywhere:
     - No plaintext pairings in source code or code comments.
     - No plaintext pairings in git commit messages or PR descriptions.
     - No plaintext pairings in terminal stdout/logs.
     - No plaintext pairings in AI agent chat responses to the user.
   - In code, givers must only exist as the MD5 hash of their lowercase first name.

---

## 🛠 Code Architecture

In [`index.html`](index.html), the JavaScript dictionary `giftAssignments` maps the **lowercase MD5 hash of the giver's first name** to the **capitalized plain name of the recipient**:

```javascript
const giftAssignments = {
    '<md5(lowercase_giver_name)>': 'RecipientName',
    ...
};
```

### Precomputed Hashes Reference

| First Name | Lowercase String | MD5 Hash |
| :--- | :--- | :--- |
| Enrico | `enrico` | `5b1b4dee9103f759fdb57197a78780a6` |
| Nadine | `nadine` | `8f5c853566391602f1a56b305e1d9cd5` |
| Sarah | `sarah` | `9e9d7a08e048e9d604b79460b54969c3` |
| Ricci | `ricci` | `083d2182dc8fdf0834b112256802ae2a` |
| Diana | `diana` | `3a23bb515e06d0e944ff916e79a7775c` |

---

## 🤖 Guide for AI Agents: How to Reshuffle

When a user asks to reshuffle or draw new Christmas raffle assignments, perform the following steps:

1. **Read Existing Assignments**:
   - Check `index.html` to see the current `giftAssignments` object.
2. **Generate a Random Valid Draw**:
   - Generate a random permutation of the 5 recipients where:
     - No one is assigned to themselves.
     - The draw is not 100% identical to the current `giftAssignments`.
3. **Format and Update `index.html`**:
   - Update the `giftAssignments` object in `index.html`.
   - Sort the dictionary keys alphabetically by MD5 hash so the order in the file does not reveal the draw order.
4. **Validate**:
   - Confirm there are exactly 5 entries.
   - Confirm all 5 MD5 hashes match the members' lowercase names.
   - Confirm all 5 recipients are unique and cover the 5 members.
   - Confirm no one is assigned to themselves.
5. **Commit the Changes**:
   - Stage and commit `index.html` with a clean, discreet commit message such as:
     `Update Christmas raffle assignments for the new year`
6. **Respond to the User**:
   - Confirm that the draw has been completed, updated, and validated.
   - **Do NOT disclose who is assigned to whom** under any circumstances.
