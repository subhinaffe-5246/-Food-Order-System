<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Online Food Ordering - Queue System</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background: #fff8f0;
      margin: 0;
      padding: 40px;
    }

    h1 {
      text-align: center;
      color: #ff6b00;
    }

    .container {
      max-width: 600px;
      margin: auto;
      background: #fff;
      border-radius: 10px;
      padding: 20px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.1);
    }

    select, input, button {
      width: 100%;
      padding: 12px;
      margin-top: 10px;
      border: 1px solid #ccc;
      border-radius: 6px;
      font-size: 16px;
    }

    button {
      background-color: #ff6b00;
      color: white;
      font-weight: bold;
      cursor: pointer;
    }

    button:hover {
      background-color: #e65c00;
    }

    .queue-preview {
      background: #fff3e6;
      border: 1px dashed #ffa94d;
      margin-top: 20px;
      padding: 15px;
      border-radius: 6px;
    }

    .order-queue {
      margin-top: 30px;
    }

    .order {
      background: #f9f9f9;
      border-left: 6px solid #ff6b00;
      margin-bottom: 10px;
      padding: 12px;
      border-radius: 6px;
      box-shadow: 0 1px 3px rgba(0,0,0,0.05);
    }

    .link {
      text-align: center;
      margin-top: 20px;
    }

    .link a {
      text-decoration: none;
      color: #0077cc;
    }
  </style>
</head>
<body>

  <h1>🍔 Online Food Order System</h1>

  <div class="container">
    <select id="foodItem">
      <option value="">-- Select Food Item --</option>
      <option value="Pizza">Pizza</option>
      <option value="Burger">Burger</option>
      <option value="Sushi">Sushi</option>
      <option value="Pasta">Pasta</option>
      <option value="Salad">Salad</option>
    </select>

    <input type="text" id="customerName" placeholder="Enter your name" />
    <button onclick="enqueueOrder()">Place Order</button>

    <div class="queue-preview" id="queuePreview">No orders in queue.</div>

    <div class="order-queue" id="orderQueue"></div>

    <div class="link">
      <a href="summary.html" target="_blank">View Order Summary Page</a>
    </div>
  </div>

  <script>
    const orderQueue = [];
    let isProcessing = false;

    function enqueueOrder() {
      const food = document.getElementById("foodItem").value;
      const name = document.getElementById("customerName").value.trim();

      if (!food || !name) {
        alert("Please select a food item and enter your name.");
        return;
      }

      orderQueue.push({ customer: name, food });
      document.getElementById("foodItem").value = "";
      document.getElementById("customerName").value = "";
      updateQueuePreview();
      processQueue();
    }

    function updateQueuePreview() {
      const preview = document.getElementById("queuePreview");
      if (orderQueue.length === 0) {
        preview.textContent = "No orders in queue.";
      } else {
        preview.innerHTML = "<strong>Orders in Queue:</strong><br>" +
          orderQueue.map((o, i) => `${i + 1}. ${o.customer} - ${o.food}`).join("<br>");
      }
    }

    function processQueue() {
      if (isProcessing || orderQueue.length === 0) return;

      isProcessing = true;
      const order = orderQueue.shift();
      updateQueuePreview();

      const display = document.getElementById("orderQueue");
      const orderEl = document.createElement("div");
      orderEl.className = "order";
      orderEl.textContent = `🍽️ Processing Order: ${order.customer} ordered ${order.food}`;

      display.appendChild(orderEl);

      setTimeout(() => {
        orderEl.remove();
        isProcessing = false;
        processQueue(); // continue with next
      }, 5000);
    }
  </script>
</body>
</html>
