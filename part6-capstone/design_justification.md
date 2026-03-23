## Storage Systems

To meet the four goals, a **hybrid data architecture (Lakehouse + specialized stores)** is most suitable.

For **predicting patient readmission risk**, a **Data Lake** is used to store raw historical data (EHR records, treatment notes, lab results). This allows flexible schema handling and supports feature engineering for ML models. Cleaned and transformed data is also stored in a **Data Warehouse**, which provides structured, query-optimized datasets for model training and evaluation.

For **natural language querying by doctors**, a **Vector Database** is essential. Patient records and clinical notes are converted into embeddings and stored here, enabling semantic search (e.g., identifying “cardiac events” even if phrased differently). This works alongside the Data Lake, which acts as the source of truth.

For **monthly reporting (bed occupancy, costs)**, a **Data Warehouse (OLAP system)** is used. It supports aggregations, joins, and time-series analysis efficiently, making it ideal for BI tools and dashboards.

For **real-time ICU vitals**, a **Time-Series Database** is used. It is optimized for high-ingestion rates and time-based queries (e.g., trends, anomaly detection). This ensures low-latency storage and retrieval for monitoring dashboards.

Together, this combination ensures each workload is handled by a system optimized for its access pattern and performance needs.

---

## OLTP vs OLAP Boundary

The **OLTP (Online Transaction Processing)** layer consists of operational systems such as EHR platforms, ICU monitoring devices, and billing systems. These systems are responsible for real-time data capture, updates, and transactions. They prioritize consistency, low latency, and reliability.

The boundary between OLTP and OLAP is defined at the **data ingestion layer (ETL/streaming pipelines)**. Once data is extracted from transactional systems, it enters the analytical domain.

The **OLAP (Online Analytical Processing)** layer begins with the Data Lake and Data Warehouse. Here, data is transformed, aggregated, and optimized for analysis rather than transactions. The ML models, reporting systems, and vector search capabilities all operate on this analytical layer.

This separation ensures that analytical workloads do not impact the performance of critical hospital operations.

---

## Trade-offs

A key trade-off in this design is **increased system complexity due to multiple specialized storage systems** (Data Lake, Warehouse, Vector DB, Time-Series DB). While each system is optimized for a specific use case, managing multiple technologies increases operational overhead, data duplication, and integration complexity.

For example, the same patient data may exist in multiple places (lake, warehouse, vector DB), leading to potential consistency issues and higher storage costs.

To mitigate this, a **Data Lakehouse approach** can be adopted, where the Data Lake and Data Warehouse are unified using technologies like Delta Lake or Apache Iceberg. This reduces duplication and simplifies data governance. Additionally, implementing a **centralized metadata/catalog system** ensures consistent schema management and data lineage tracking.

This trade-off is justified because it enables scalability, flexibility, and high performance across diverse workloads, which is critical in a healthcare setting.
