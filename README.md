// ZeeKwat Game Shop - React App Template with Updated Diamond Packages

import React, { useState } from "react";

const diamondPackages = [
  { id: 1, amount: 86, price: 5500 },
  { id: 2, amount: 172, price: 11000 },
  { id: 3, amount: 257, price: 16500 },
  { id: 4, amount: 343, price: 22000 },
  { id: 5, amount: 429, price: 27500 },
  { id: 6, amount: 514, price: 33000 },
  { id: 7, amount: 600, price: 38500 },
  { id: 8, amount: 706, price: 44000 },
  { id: 9, amount: 878, price: 55000 },
  { id: 10, amount: 1049, price: 66000 },
  { id: 11, amount: 1135, price: 71500 },
  { id: 12, amount: 1412, price: 88000 },
  { id: 13, amount: 1669, price: 104500 },
  { id: 14, amount: 2195, price: 132000 },
  { id: 15, amount: 2901, price: 176000 },
  { id: 16, amount: 3688, price: 220000 },
  { id: 17, amount: 5532, price: 330000 },
  { id: 18, amount: 9288, price: 550000 },
  { id: 19, amount: "Weekly Pass (1 week)", price: 6700 },
];

const TELEGRAM_BOT_TOKEN = "7697523982:AAGQGqbt7RenN8h9wpUqxn36Nu7guK7SWM4"; 
const TELEGRAM_CHAT_ID = "1837570754";     

function App() {
  const [selected, setSelected] = useState(null);
  const [uid, setUid] = useState("");
  const [serverId, setServerId] = useState("");
  const [ordered, setOrdered] = useState(false);
  const [orderData, setOrderData] = useState(null);

  const handleOrder = async () => {
    if (!uid  !serverId  !selected) return alert("Fill all fields");

    const newOrder = {
      uid,
      serverId,
      package: selected,
      date: new Date().toLocaleString(),
    };

    localStorage.setItem("order", JSON.stringify(newOrder));
    setOrderData(newOrder);
    setOrdered(true);

    const message = 🛒 New Order Received!\n\n👤 UID: ${uid}\n🔢 Server ID: ${serverId}\n💎 Diamond: ${selected.amount}\n💰 Price: ${selected.price} Ks\n🕒 Time: ${newOrder.date};

    await fetch(https://api.telegram.org/bot${TELEGRAM_BOT_TOKEN}/sendMessage, {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({
        chat_id: TELEGRAM_CHAT_ID,
        text: message,
        parse_mode: "Markdown",
      }),
    });
  };

  return (
    <div className="min-h-screen bg-gray-900 text-white p-4">
      <h1 className="text-3xl font-bold text-center mb-6">ZeeKwat Game Shop</h1>
      <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4 mb-6">
        {diamondPackages.map((pkg) => (
          <div key={pkg.id} className={p-4 border rounded ${selected?.id === pkg.id ? "border-green-400" : "border-gray-700"}} onClick={() => setSelected(pkg)}>
            <p className="text-xl font-semibold text-center">{pkg.amount} 💎</p>
            <p className="text-lg text-center">{pkg.price.toLocaleString()} Ks</p>
          </div>
        ))}
      </div>

      <div className="grid grid-cols-1 sm:grid-cols-2 gap-4 mb-6">
        <input type="text" className="p-2 bg-gray-800 rounded" placeholder="MLBB UID" value={uid} onChange={(e) => setUid(e.target.value)} />
        <input type="text" className="p-2 bg-gray-800 rounded" placeholder="Server ID" value={serverId} onChange={(e) => setServerId(e.target.value)} />
      </div>

      <button onClick={handleOrder} className="w-full bg-green-600 hover:bg-green-700 text-white py-2 rounded">Confirm Order</button>

      {ordered && orderData && (
        <div className="mt-6 bg-green-800 p-4 rounded">
          <p className="font-semibold text-center mb-2">✅ Order Submitted Successfully!</p>
          <p><strong>UID:</strong> {orderData.uid}</p>
          <p><strong>Server:</strong> {orderData.serverId}</p>
          <p><strong>Diamond:</strong> {orderData.package.amount}</p>
          <p><strong>Price:</strong> {orderData.package.price.toLocaleString()} Ks</p>
          <p><strong>Date:</strong> {orderData.date}</p>
        </div>
      )}
    </div>
  );
}

export default App;
