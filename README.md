# PES-VCS: A Mini Version Control System

## 👤 Student Details

* Name: Arya
* Course: B.Tech CSE
* Lab: Operating Systems
* SRN: PES2UG24CS629

---

# 📌 Overview

This project implements a simplified version control system inspired by Git. The system, called **PES-VCS**, supports core version control operations such as:

* Repository initialization
* File staging
* Snapshot creation using trees
* Commit creation with history tracking
* Content-addressable storage using SHA-256

The implementation demonstrates how modern VCS systems internally manage data efficiently.

---

# ⚙️ System Design

The system is divided into four main components:

---

## 🔹 1. Object Store (`object.c`)

* Stores all data (blobs, trees, commits)
* Uses **SHA-256 hashing**
* Implements **content-addressable storage**
* Ensures:

  * Deduplication
  * Integrity verification

📌 Format:

```
<type> <size>\0<data>
```

---

## 🔹 2. Index (Staging Area) (`index.c`)

* Tracks files staged for commit
* Stores:

  * File path
  * Hash
  * Mode
  * Timestamp
  * Size

📌 Acts like:

```
git add
```

---

## 🔹 3. Tree Structure (`tree.c`)

* Represents a snapshot of the directory
* Converts index → tree object
* Stores hierarchical structure (files/directories)

📌 Each entry:

```
<mode> <name> <hash>
```

---

## 🔹 4. Commit System (`commit.c`)

* Creates commits using:

  * Tree reference
  * Parent commit
  * Author + timestamp
  * Message

* Maintains history using **linked structure**

📌 Commit format:

```
tree <hash>
parent <hash>
author <name>
message <msg>
```

---

# 🔄 Workflow

The complete workflow:

```
pes init → pes add → pes commit → pes log
```

1. `init` → creates `.pes` structure
2. `add` → stores file as blob + updates index
3. `commit` → creates tree + commit object
4. `log` → traverses commit history

---

# 📸 Screenshots

## 🔹 Phase 1

* `./test_objects` → All tests passed
* Object storage verified

## 🔹 Phase 2

* `./test_tree` → Tree creation verified
* `xxd` output shows raw tree structure

## 🔹 Phase 3

* `pes add` + `pes status` working
* `.pes/index` shows metadata

## 🔹 Phase 4

* `pes log` shows full commit history
* References (`HEAD`, `refs/heads/main`) working
* Object store growth verified

## 🔹 Final Integration

* `make test-integration` passed successfully

---

# 🧠 Key Concepts Learned

* Content-addressable storage
* Hash-based integrity
* Tree-based filesystem snapshots
* Commit history as linked list
* Separation of working directory and index

---

# 📊 Analysis Questions

---

## 🔹 Q5: Branching

### Q5.1 – How are branches implemented?

Branches are implemented as pointers to commits. Each branch stores the hash of the latest commit. For example:

```
refs/heads/main → <commit_hash>
```

---

### Q5.2 – How does branching work internally?

When a new branch is created:

* A new reference file is created
* It points to an existing commit
* Both branches share history until divergence

---

### Q5.3 – How does merging work conceptually?

Merging combines histories by:

* Finding common ancestor
* Combining changes
* Creating a new commit with multiple parents

---

## 🔹 Q6: Garbage Collection

### Q6.1 – What are unreachable objects?

Unreachable objects are:

* Objects not referenced by any commit
* Created but no longer needed

---

### Q6.2 – How does garbage collection work?

Garbage collection:

* Traverses reachable objects from HEAD
* Deletes all unreferenced objects
* Frees storage space

---

# ✅ Conclusion

This project successfully implements a mini version control system demonstrating:

* Efficient data storage using hashing
* Version tracking through commits
* File system snapshot representation using trees

It provides a clear understanding of how Git internally works.

---

