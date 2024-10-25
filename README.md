
---

# Retool-Access Project

> **Streamlining data management for the Fulfillment team**

---

### **Diagram Overview**

Below is the **Retool-Access Diagram**, showcasing how data flows between Retool and the main database, with a focus on managing active vs. dormant data efficiently.

```
                             +---------------------------+
                             |                           |
                             |       Retool Database     |
                             |        (Active Data)      |
                             |       (5 GB Limit)        |
                             |                           |
                             +-----------+---------------+
                                         |
             +---------------------------+------------------------------------------+
             |                           |                                          |
+------------v-------------+   +---------v---------+                       +---------v--------+
|  Companies               |   |      Contacts     |                       |     Projects      |
|  (Active Companies)      |   |  (Linked to       |                       |   (Active)        |
+--------------------------+   |   Companies)      |                       +-------------------+
|                           |   +-------------------+                               |
|                           |                                                +------v--------+
+---------------------------+                                                |  Expert       |
                                                                             |  Requests     |
                                                                             |  (Open)       |
                                                                             +---------------+
                                                                                     |
                                                                                     |
                                 +------------------------+                 +---------v--------+
                                 |  Expert Applications   |                 |   Consultations  |
                                 |   (Linked to Requests) |                 |   (Recent Only)  |
                                 +------------------------+                 +------------------+
                                                                                     |
                                                                                     |
                                                                           +---------v--------+
                                                                           |      Tickets     |
                                                                           |    (Linked to    |
                                                                           |  Expert Requests)|
                                                                           +------------------+


                           +-------------------------------------------+
                           |                                           |
                           |            Main Database                  |
                           |      (AWS or other storage)               |
                           |       (All Historical Data)               |
                           +-------------------------------------------+

  <--------------------------On-demand Queries & Data Sync --------------------------->

   1. **On-demand Data Retrieval**: Accesses main database to pull recent or specific records 
      (e.g., for Companies, Contacts, Projects, Expert Requests, Consultations) into the Retool database.
   2. **Periodic Dormant Data Sync**: Identifies and moves dormant data back to the main database, 
      removing it from Retool to manage storage efficiently.
```

---

## 📋 **Project Overview**

**Retool-Access** enables the Fulfillment team to manage expert requests, consultations, and profiles seamlessly within Retool. Key goals include:

- **Enhanced Data Management**: Allows the team to add custom properties like pipeline stages without impacting the main API.
- **Efficient Storage**: Retool hosts only active records, while the main database archives dormant data, optimizing Retool’s 5GB limit.
- **On-demand Retrieval**: Supports data reactivation as needed for on-the-fly requests from dormant archives.

---

## 🏗️ **Architecture**

The **Retool-Access Diagram** illustrates the application’s data architecture, which relies on two main layers:

1. **Retool Database (5GB limit)**:
   - Stores only **active data** relevant to current projects, consultations, and expert requests.
   - Automates the syncing process to manage active and dormant records effectively.

2. **Main Database (AWS or Other)**:
   - Serves as the primary storage, hosting all historical data and archiving dormant records that exceed the Retool activity threshold.

For further architectural details, view the [Retool-Access Diagram Documentation](Docs/Retool-Access-Diagram.md).

---

## 🔄 **Data Flow**

1. **On-Demand Data Retrieval**:
   - The Fulfillment team retrieves records as needed from the main database, temporarily storing them in the Retool database.
   - Only recent and active data (approx. 5% of records) is fetched to keep storage usage within limits.

2. **Dormant Data Sync**:
   - A regular sync process (e.g., monthly) identifies and archives dormant data back to the main database.
   - This ensures Retool contains only active or recently accessed records, clearing space for new requests.

---

## 📊 **Key Tables and Relationships**

- **Companies**: Associated with both **Contacts** (company representatives) and **Projects**.
- **Contacts**: Individuals representing a company, often involved in multiple projects.
- **Projects**: Main operational unit, hosting multiple **Expert Requests** for consultations.
- **Expert Requests**: Central point linking **Expert Applications**, **Consultations**, and **Tickets**.
- **Expert Applications**: Track submissions from experts for each request.
- **Consultations**: Represent active sessions linked to expert requests.
- **Tickets**: Connected to expert requests, supporting issues or inquiries.

Each table plays a critical role in the Fulfillment team’s workflow. For more in-depth information, refer to [Table Documentation](Docs/Table-Structure.md).

---

## 🛠️ **Setup and Usage**

1. **Setting Up Retool Database**:
   - Connect the Retool PostgreSQL database and set up data sync to pull active data on demand.
   - Configure automation to archive dormant data back to the main database periodically.

2. **Using Retool Apps**:
   - Use Retool’s UI to manage data fields such as **pipeline stages**, **consultation tracking**, and **expert application statuses**.
   - Employ on-demand queries to retrieve specific archived records as needed, avoiding storage overflows in Retool.

---

## 🤝 **Contributing**

We welcome contributions! To get started:

1. **Fork the Repository**: Make a personal copy.
2. **Create a New Branch**: For each feature or bug fix.
3. **Submit a Pull Request**: Provide a description and tag relevant team members for review.

---

## 📄 **Additional Resources**

- **[Data Sync Workflow](Docs/Data-Sync-Workflow.md)**: Detailed steps on syncing active and dormant data.
- **[Troubleshooting Guide](Docs/Troubleshooting-Guide.md)**: Common issues and solutions.

---
