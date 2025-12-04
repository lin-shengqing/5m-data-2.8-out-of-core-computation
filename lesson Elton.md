# Lesson

## Brief

### Preparation

Create the conda environment based on the `ooc-environment.yml` file in the [environment folder](https://github.com/su-ntu-ctp/5m-data-2.1-intro-big-data-eng/tree/main/environments).

### High Level Overiew
Here’s the clean, beginner-friendly explanation — and you’ll understand exactly whether they’re the same thing or not.

---

# ✅ What is “Out-of-Core DataFrame”?

**An out-of-core dataframe is a dataframe that can work with datasets *larger than your computer’s RAM*, by loading only small chunks of data at a time.**

Normal Pandas loads the entire dataset into memory (RAM).
If RAM is 16GB and your CSV is 30GB → Pandas will crash.

Out-of-core dataframes **avoid this** by:

* reading data chunk-by-chunk
* processing only what fits into memory
* offloading heavy operations to disk or distributed systems

Common out-of-core dataframe technologies:

| Library                     | What it does                                           |
| --------------------------- | ------------------------------------------------------ |
| **Dask DataFrame**          | Parallel processing, chunked DataFrame                 |
| **Vaex**                    | Lazy out-of-core DataFrame optimized for huge datasets |
| **Polars (streaming mode)** | Efficient columnar processing, lazy execution          |
| **Modin**                   | Distributed pandas (backend: Ray or Dask)              |
| **PySpark DataFrame**       | Distributed across a cluster, but same concept         |

**In short: Out-of-core = works with “too big for memory” datasets.**

---

# ✅ What is “Out-of-Core Data Processing”?

This is the **broader concept**, not tied to dataframes only.

**Out-of-core processing** = any technique that processes data too large to fit into memory by using disk, streaming, or distributed chunks.

This applies to:

* DataFrames
* Machine Learning
* ETL pipelines
* NLP (processing huge text corpora)
* Image/video data
* Databases

Examples of out-of-core processing:

* Reading CSVs in chunks (`pd.read_csv(..., chunksize=50000)`)
* Training ML models on mini-batches (SGD, partial_fit in scikit-learn)
* Stream processing frameworks (Kafka, Flink)
* Using Dask/Spark to process TB-level data

So:

👉 **Out-of-core = “I process data without loading everything at once.”**

---

# 🎯 Do they come hand-in-hand? Or separate topics?

They **overlap**, but:

### ✔ Out-of-core DataFrame

is a **specific tool / implementation** of

### ✔ Out-of-core Data Processing (general strategy).

Think of it like this:

* **Out-of-core processing = the concept**
* **Out-of-core dataframe = one tool that implements this concept**

**So yes, they are related — but they are not the same.**

---

# 🔥 Simple Analogy

* Your RAM is like a dining table.
* A giant 30GB dataset is like a huge buffet.
* Pandas tries to bring the entire buffet onto your table → doesn’t fit → crash.
* Out-of-core processing brings one plate at a time.
* Out-of-core DataFrame is a restaurant helper (Dask, Vaex, Polars streaming) that does this automatically for you.

--------

### Lesson Overview

In this lesson, we will be learning about _out of core_ dataframe and data processing. We will use `Polars`, an alternative dataframe libraries to `Pandas` for more performant and scalable data processing. We will also use `DuckDB` to query data that is too large to fit in memory.

_Out of core_ computation is a technique that allows us to process data that is **too large to fit in memory**. This is a common problem in data science and data engineering. Although Pandas is the de-facto dataframe library that is used in data science and data engineering, it is limited by the amount of memory (RAM) available on the machine as it is an _in-memory_ dataframe library. It is also less performant, as it processes data in a _single thread_, which is a bottleneck for modern computers that have multiple cores.

---
Pandas runs mostly in single-thread mode because of Python’s design (GIL).
This means only 1 CPU core is used, even if you have many.
For small/medium datasets → fine.
For big datasets → slow and inefficient.
Modern alternatives (Dask, Polars, Spark) overcome this by parallelizing across multiple cores or clusters.


🆚 How do modern DataFrame systems fix this?
✔ Dask DataFrame
Breaks your data into many partitions and processes them in parallel across multiple threads/cores.

✔ Polars
Built in Rust — no GIL, multi-threaded by default. Often 10×–100× faster than Pandas.

✔ Modin
Acts like “Pandas but parallel”, using Ray or Dask backend.

✔ PySpark / Databricks
Distributed across many machines.
---

Polars is an alternative dataframe library that is more performant and scalable than Pandas. It's designed to be fast, using **Arrow** arrays as its foundation, and it can leverage _multi-threading_ for various operations. This makes it particularly useful for large datasets. Polars can process data in chunks. Instead of loading the entire dataset into memory, it reads and processes data in manageable chunks. This means that even if the dataset is larger than available memory, Polars can still handle it by sequentially processing each chunk.

However, being a newer library, Polars might not have all the features that Pandas provides, but it is rapidly improving.

---

## Part 1 - Out-of-core Computation and Data Frames

We will be using the `out_of_core.ipynb` notebook throughout this lesson.

> Open the notebook in VSCode by double clicking on the file. Then select `ooc` conda environment for the kernel.
>
> Follow on with the lesson in the notebook.
