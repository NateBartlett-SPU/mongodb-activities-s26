# MongoDB with `mongosh` — Full Intro Lab

## 🎯 Goal
In this lab, you will:
1. Create a Node + MongoDB development container
2. Start MongoDB using Docker Compose
3. Connect to MongoDB using mongosh
4. Insert product data
5. Practice find, update, and delete operations
6. Complete exercises using JSON-style queries

---

# 🧱 Part 1 — Project Setup
```bash
mkdir mongodb-mongosh-lab
cd mongodb-mongosh-lab
mkdir .devcontainer
```

---

# 🐳 Part 2 — Dev Container Files

## devcontainer.json
```json
{
  "name": "Node MongoDB Dev Container",
  "dockerComposeFile": "docker-compose.yml",
  "service": "app",
  "workspaceFolder": "/workspace",
  "forwardPorts": [3000, 27017],
  "postCreateCommand": "node --version && npm --version && mongosh --version"
}
```

## Dockerfile
```Dockerfile
FROM node:20-bookworm
RUN apt-get update && apt-get install -y curl gnupg
RUN curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc \
    | gpg --dearmor -o /usr/share/keyrings/mongodb-server-7.0.gpg
RUN echo "deb [ signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/debian bookworm/mongodb-org/7.0 main" \
    > /etc/apt/sources.list.d/mongodb-org-7.0.list
RUN apt-get update && apt-get install -y mongodb-mongosh
WORKDIR /workspace
```

## docker-compose.yml
```yaml
services:
  app:
    build: .
    volumes:
      - ..:/workspace
    command: sleep infinity
    depends_on:
      - db

  db:
    image: mongo:7
    ports:
      - "27017:27017"
```

---

# ▶️ Part 3 — Start
```bash
mongosh "mongodb://db:27017"
```

---

# 🗄️ Part 4 — Database
```js
use storeDB
```

---

# 📦 Part 5 — Insert Data
```js
db.products.insertMany([
  { name: "Laptop", price: 899.99, quantity: 10, warehouse: "A" },
  { name: "Mouse", price: 19.99, quantity: 150, warehouse: "B" },
  { name: "Keyboard", price: 49.99, quantity: 85, warehouse: "A" },
  { name: "Monitor", price: 199.99, quantity: 40, warehouse: "C" },
  { name: "Printer", price: 129.99, quantity: 25, warehouse: "D" },
  { name: "Desk Lamp", price: 34.99, quantity: 60, warehouse: "B" },
  { name: "USB Cable", price: 9.99, quantity: 300, warehouse: "A" },
  { name: "External HDD", price: 79.99, quantity: 45, warehouse: "C" },
  { name: "Webcam", price: 59.99, quantity: 70, warehouse: "D" },
  { name: "Headphones", price: 89.99, quantity: 55, warehouse: "B" },
  { name: "Smartphone", price: 699.99, quantity: 35, warehouse: "A" },
  { name: "Tablet", price: 329.99, quantity: 20, warehouse: "C" },
  { name: "Charger", price: 24.99, quantity: 120, warehouse: "D" },
  { name: "Backpack", price: 49.99, quantity: 65, warehouse: "B" },
  { name: "Router", price: 109.99, quantity: 30, warehouse: "A" },
  { name: "Switch", price: 89.99, quantity: 28, warehouse: "C" },
  { name: "Speakers", price: 149.99, quantity: 22, warehouse: "D" },
  { name: "Microphone", price: 79.99, quantity: 33, warehouse: "B" },
  { name: "Camera", price: 499.99, quantity: 15, warehouse: "A" },
  { name: "Tripod", price: 39.99, quantity: 50, warehouse: "C" },
  { name: "SSD", price: 119.99, quantity: 75, warehouse: "D" },
  { name: "RAM", price: 89.99, quantity: 95, warehouse: "A" },
  { name: "GPU", price: 399.99, quantity: 12, warehouse: "B" },
  { name: "CPU", price: 299.99, quantity: 18, warehouse: "C" },
  { name: "Motherboard", price: 159.99, quantity: 27, warehouse: "D" },
  { name: "Power Supply", price: 99.99, quantity: 32, warehouse: "A" },
  { name: "Case", price: 79.99, quantity: 40, warehouse: "B" },
  { name: "Cooling Fan", price: 14.99, quantity: 200, warehouse: "C" },
  { name: "Thermal Paste", price: 7.99, quantity: 180, warehouse: "D" },
  { name: "HDMI Cable", price: 12.99, quantity: 220, warehouse: "A" }
]);
```

---

# 🔍 Part 6 — Queries
```js
db.products.find()
db.products.find({ warehouse: "A" })
db.products.find({ price: { $gt: 100 } })
db.products.find({ quantity: { $lt: 20 } })
```

---

# ✏️ Part 7 — Updates
```js
db.products.updateOne({ name: "Mouse" }, { $set: { price: 17.99 } })
db.products.updateMany({}, { $set: { status: "active" } })
db.products.updateOne({ name: "USB Cable" }, { $inc: { quantity: 50 } })
```

---

# ❌ Part 8 — Deletes
```js
db.products.deleteOne({ name: "Thermal Paste" })
db.products.deleteMany({ quantity: { $lt: 15 } })
```

---

# 🧪 Part 9 — Exercises

## Basic
- Find all products
- Find products in warehouse C
- Find products price < 50
- Find products quantity >= 100

## Intermediate
- Warehouse A AND price > 100
- Warehouse B AND price < 100
- Quantity < 50 AND price > 50

## Updates
- Change Keyboard price to 44.99
- Add field category="Accessories" to Mouse
- Increase HDMI Cable quantity by 30

## Deletes
- Delete Printer
- Delete all products in warehouse D
- Delete products with price < 10

---

# 🚀 Challenges

- Find products where warehouse IN ["A","B"]
- Find products where price between 50 and 200
- Add reorderNeeded=true where quantity < 25
- Set reorderNeeded=false where quantity >= 25

