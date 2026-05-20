# 🤝 Social Debt Router

An intelligent, client-side graph-routing web application designed to simplify group expenses and debts safely. Unlike generic debt splitters that prioritize arbitrary pairings, this application routes transfers strictly through an individual's explicit network of trusted friends.

🔗 **Live Demo:** `https://chrismorang.github.io/social-debt-router/`

---

## 🚀 Key Features

* **Preference-Constrained Graph Routing:** Leverages a Breadth-First Search (BFS) pathfinding engine to move money across trust chains. Untrusted pathways are completely restricted from executing direct transactions.
* **Auto-Aggregated Transaction Chains:** Fuses multi-step routes natively. If a user acts as a pipeline node multiple times across different settlement branches, the engine automatically compiles everything into a single lump-sum instruction.
* **Zero Overhead Portability:** Runs entirely inside the client browser as a single HTML5 file. No node dependencies, databases, backend stacks, or configuration servers required.
* **State Import & Export System:** Instantly download or upload an entire group layout matrix via a compact JSON configuration file to eliminate repetitive data re-entry.
* **Clean, Modern Layout:** Engineered using a mobile-responsive dashboard framework styled with Tailwind CSS.

---

## 📖 How to Use (User Guide)

This section explains how to set up your group, import data, and calculate the most optimized, friction-free payment plan.

### ⚡ Quick Start Process

1. **Add Your Group Members:**
   * Locate the **1. Add/Manage Group** panel.
   * Type a friend's name into the text field and click **Add**. Repeat for everyone involved (Maximum: 15 people).

2. **Enter Balances & Map Preferences:**
   * For each person card, enter their final balance. Use a **negative sign (`-`)** if they *owe* money (Debtors highlight in red). Use a **positive sign** if they are *owed* money (Creditors highlight in green).
   * Check the boxes next to the names of people they are comfortable transacting with. 
   * *Constraint:* Each person must select **at least 1** friend, up to a **maximum of 5**.

3. **Compute and Settle:**
   * Click the large green **🚀 Calculate Optimized Settlements** button.
   * Look at the **3. Optimized Transaction Plan** panel on the right. The system outputs a clean list showing exactly who needs to pay whom. All repetitive paths are automatically grouped together into single-line payouts.

### 💾 Saving and Loading Group Data
To save you from typing names and mapping checkboxes every single time you open the app, use the Config Toolbar at the top:
* **📤 Export Config (JSON):** Click this to instantly download a file named `debt_settler_config.json` containing your entire group structure.
* **📥 Import Config:** The next time you open your site, upload your saved `debt_settler_config.json` file to instantly restore your entire group list, balances, and checked preferences exactly where you left off.

---

## 📐 How the Algorithm Works

Standard algorithms (like Splitwise's transaction minimizer) focus purely on optimizing node degrees. In reality, routing money to someone you do not know well or don't get along with creates immense real-world friction. 

### The Logic Breakdown
1. **Network Building:** The engine interprets your inputs to construct a dedicated directional network graph $G = (V, E)$ containing bidirectional clearing capabilities where true preferences cross over.
2. **Path Selection:** When evaluating an individual with a negative balance, the engine isolates paths toward active positive credit holders over the fewest possible steps.
3. **Lump Sum Collapsing:** To avoid making middlemen manage multiple separate fractions of cash transfers, an internal lookup layer groups identical directional trajectories (`Sender -> Recipient`) right before rendering the UI dashboard cards.

### Data Configuration Schema (JSON)
The built-in configuration exporter outputs structured data mimicking the following blueprint:

```json
{
  "Chris": {
    "balance": 1196.36,
    "preferences": ["Dhiraj", "Inghi", "Kabang"]
  },
  "Deka": {
    "balance": -534.96,
    "preferences": ["Chris", "Dhiraj"]
  }
}
```
---

## ❓ Frequently Asked Questions

### 👥 Group Logistics & Preferences

#### Q: What happens if two people are "enemies" or don't know each other?
**A:** Simply **do not check** the box connecting them. The routing algorithm treats unchecked connections as physically broken paths. It will intelligently scan the network for a mutual connection to act as a temporary pass-through pipeline instead.

#### Q: Does a middleman lose or gain money during a pass-through transfer?
**A:** **No.** If Person A passes $100 to Person B, and Person B is immediately routed to pass $100 to Person C, Person B's net out-of-pocket change is exactly $0. Their personal pocket remains completely unaffected.

#### Q: Can money flow backwards through a checked preference?
**A:** Yes. If Person X lists Person Y as a preferred friend, the algorithm treats this as a relationship of mutual trust. Money can flow from X to Y (X paying off debt) or from Y to X (Y passing along a route to clear X's credit). 

---

### ⚠️ Troubleshooting & Warnings

#### Q: The app is throwing a warning that balances don't equal zero. Why?
**A:** This means the total sum of all your positive numbers doesn't match the total sum of your negative numbers in your master spreadsheet. Double-check your core spreadsheet math! For a group to fully square up, the sum of all debts and credits must equal `0`. 

#### Q: What happens if a debtor has selected friends, but all of those friends also owe money?
**A:** This creates an "isolated island" in the network graph where money has no clear path to reach a creditor. If the algorithm completely runs out of friendly paths to an available pool of cash, it utilizes a built-in **Fallback Safety Match**—automatically forcing a direct transaction to the highest available pool creditor to ensure the group's debts actually get settled.

#### Q: What happens if someone has a balance but selects 0 friends?
**A:** The application will trigger a safety alert blocking the calculation. Everyone must select **at least 1 friend** so the algorithm has a starting path to route the cash.

#### Q: How secure is the Import/Export Config feature?
**A:** It is completely secure. The application runs entirely client-side (locally in your browser). When you export or import the JSON file, your data never leaves your computer, and absolutely no data is uploaded to an external database or server.
