### **1. Cassandra Issue: Query Returning Deleted Data**
#### **Likely Reason**
- Cassandra uses **eventual consistency** due to its distributed nature. Deleted records (tombstones) may still appear in queries if some nodes haven’t fully synchronized.
- If **read repair** hasn’t been triggered, or if the consistency level used for queries is too low, stale data may be returned.
- The deleted data might persist due to **compaction** delays or **hinted handoff** issues.

#### **How to Avoid It**
- **Increase Consistency Level:** Use `QUORUM` or `ALL` instead of `ONE` when querying.
- **Run Repairs Regularly:** `nodetool repair` helps sync data across nodes.
- **Check Compaction Strategy:** Ensure that tombstones are removed efficiently.
- **Manually Flush Data:** Running `nodetool flush` can help.

---

### **2. MongoDB: Sharding `sanfrancisco.company_name` Based on `_id`**
Since `replicaset_1` is underperforming, we will set up **sharding** using `replicaset_2`. Below are the steps:

#### **Step 1: Enable Sharding for the Database**
```sh
sh.enableSharding("sanfrancisco")
```

#### **Step 2: Choose `_id` as the Shard Key**
```sh
sh.shardCollection("sanfrancisco.company_name", { "_id": "hashed" })
```
Using a **hashed** `_id` ensures even distribution.

#### **Step 3: Add the New Shard (`replicaset_2`)**
```sh
sh.addShard("replicaset_2/mongo-node2:27017")
```
Replace `mongo-node2:27017` with the correct hostname and port.

#### **Step 4: Verify Sharding**
```sh
sh.status()
```

