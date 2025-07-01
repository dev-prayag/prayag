
# Prayag DB

**Prayag** is a blazing fast, minimal in-memory key-value store for Node.js.
It combines the speed of a cache with safe disk snapshots, TTL expiry, log compaction, batch operations, and zero bloat.

Perfect for Discord bots, configs, simple storage, and any lightweight app that needs persistence.

---

## ⚡️ Features

- 🧩 Simple API: `set`, `get`, `delete`, `has`
- 🕒 TTL: auto-expire keys
- 💾 Persistence: compressed snapshot + changelog
- 🔄 Log compaction: log is cleared after each snapshot
- 🚀 Low latency reads/writes (in-memory)
- 🗃️ Batch operations: `setMany`, `getMany`
- 🔍 Filter and inspect keys: `filter`, `details`
- 🧹 Automatic cleanup for expired keys
- 🗂️ Works out-of-the-box with Node.js (no database server needed!)

---

## 📦 Installation

```bash
npm install prayag
```

---

## 📂 How it works

- All keys and values are stored **in memory**.
- The state is **snapshotted to disk** at an interval you choose.
- Every write is also logged to a **changelog** for crash recovery.
- Log is **compacted** automatically after each snapshot to keep disk space minimal.

---

## 🚀 Usage Example

```js
import Prayag from 'prayag';

const db = new Prayag({
  snapshotInterval: 30000,
  cleanupInterval: 60000
});

db.set('hello', 'world');
console.log(db.get('hello'));

db.delete('hello');
console.log(db.has('hello'));

db.set('temp', 'bye', 5000);
setTimeout(() => {
  console.log(db.get('temp'));
}, 6000);

db.setMany([
  ['user:1', { name: 'Alice' }],
  ['user:2', { name: 'Bob' }]
]);

console.log(db.getMany(['user:1', 'user:2']));
console.log(db.filter((k, v) => v.name === 'Bob'));
console.log(db.details());

db.close();
```

---

## 📁 Data Folder

Prayag automatically creates `./data`:
- `snapshot.json.gz`
- `changelog.log`

The log is cleared after each snapshot.

---

## ✅ Example: Guild Config

```js
db.set(`guild:123456`, {
  guildId: '123456',
  buttons: true,
  reconnect: {
    status: false,
    textChannel: null,
    voiceChannel: null
  }
});

console.log(db.get(`guild:123456`));
```

---

## 🧹 Cache Management

Prayag automatically cleans up expired keys with a background timer.

---

## 📝 License

MIT
