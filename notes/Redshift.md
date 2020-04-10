# Redshift
Scaling up and down is easy, though not automatic.

![](Redshift/Screenshot%202020-04-10%20at%206.23.40%20AM.png)

- - - -
## Redshift Architecture
— `Cluster`
	— `Leader Node` 
	— `Compute Nodes`
		—`Node Slices` 

### Leader Node
	* manages communication between clients and compute nodes
	* prepares query execution plan
	* aggregates query results from compute nodes and sends back to the client
		
### Compute Node	
	* (1 - 128 nodes based on compute type)
	* Two types `dense storage` -> `HDD` -> `Large warehouses`
			   `dense compute` -> `SSD` -> `Faster Warehouse`
	
	* has its own dedicated CPU, memory and attached disk storage based on chosen node type

### Node Slice
	* Each Node slice has a portion of the memory and disk space from its compute node, and all the sub-processing of the partial query is done by the node slice for the compute node.
	
- - - -
## Redshift Spectrum
Allows to easily query data stored on S3 as any other table in Redshift.
The storage is still managed by S3, only the compute is utilised by spectrum. It supports all the common formats for compressed data and open-source formats (like CSV, Avro, Parquet etc.)

- - - -
## Durability and Scalability
* Replicates all data when its loaded into the cluster.
* Data automatically backed upto S3.
* 3 copies of the data are created. (Original -> replica -> S3)
* In the event of a drive failure, Redshift will run with degraded performance, until Redshift rebuilds that drive with a replica of the drive stored on the cluster.
* Supports both `horizontal -> increasing no. of nodes` and `vertical -> higher instance type` scaling.

- - - -
## Distribution Styles
`EVEN`
`KEY`
`ALL`

### EVEN Distribution

![](Redshift/Screenshot%202020-04-10%20at%207.55.12%20AM.png)


Leader node assigns data in a round-robin fashion amongst all worker nodes.

### KEY Distribution 

![](Redshift/Screenshot%202020-04-10%20at%207.57.27%20AM.png)

Data divided based on one column. This can come in handy when, queries are made based on data in a single column, having data physically stored together on a node slice, can speed up queries.

### ALL Distribution

![](Redshift/Screenshot%202020-04-10%20at%207.59.50%20AM.png)

Replicates data across all nodes.
Inserts, Updates, Deletes will be slower.
 Really only applicable for slow changing tables. 

- - - -
## Sort Keys
Very important and needs to be decided upfront, as it helps skip over large blocks of data while querying.

