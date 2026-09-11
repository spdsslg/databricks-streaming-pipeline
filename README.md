## Details
The pipeline is built using Medallion architecture. Raw data is created with `faker` library and simulates two sources, which are `drivers` and `rides`. Generated data is saved as json files in a Volume. This landing point is append only. 

The bronze layer of the architecture transforms data from raw storage and saves it into two delta tables with the corresponding names `drivers_bronze` and `rides_bronze`. It is also append only.

Silver layer normalises and filters data from the bronze layer. As data in `drivers` source can change (for example a driver's rating changes), duplicates can occur in bronze layer and raw data storage. In silver layer they are deduplicated and correclty upserted to the corresponding silver tables. As data from this layer is then used in Gold layer and updates are possible, CDF is enabled. Z-Ordering is later enabled(in Queries and Optimisations file). 

Gold layer represents a preaggregated delta table with such columns as `avg_distance`, `avg_cost`, `total_distance`. Liquid clustering is enabled.
