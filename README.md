# symmetrical-waffle
SwissBank Empire - React Dashboard with Withdraw, Balance, and Transaction UI.
const express = require('express');
const cors = require('cors');
const app = express();
app.use(cors());
app.use(express.json());

let balance = 3500000;  // Simulated user balance
let transactions = [
  { id: 1, date: "2025-08-01", type: "Deposit", amount: 8000000, status: "Completed" },
  { id: 2, date: "2025-08-02", type: "Deposit", amount: 3000000, status: "Completed" },
  { id: 3, date: "2025-08-03", type: "Withdrawal", amount: 500000, status: "Pending" },
];

// GET balance
app.get('/balance', (req, res) => {
  res.json({ balance });
});

// GET transactions
app.get('/transactions', (req, res) => {
  res.json({ transactions });
});

// POST withdraw request
app.post('/withdraw', (req, res) => {
  const { amount } = req.body;
  if (!amount || amount <= 0) return res.status(400).json({ message: "Invalid amount" });
  if (amount > balance) return res.status(400).json({ message: "Insufficient balance" });

  const newTxn = {
    id: transactions.length + 1,
    date: new Date().toISOString().slice(0, 10),
    type: "Withdrawal",
    amount,
    status: "Pending"
  };
  transactions.unshift(newTxn);
  balance -= amount;
  res.json({ message: "Withdraw request successful", transaction: newTxn, balance });
});

const PORT = 5000;
app.listen(PORT, () => console.log(`Server running on port ${PORT}`));
