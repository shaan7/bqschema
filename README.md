# BQSchema [![wercker status](https://app.wercker.com/status/c3ce047415c3b4ba6ac9bc5ad26d1747/s "wercker status")](https://app.wercker.com/project/bykey/c3ce047415c3b4ba6ac9bc5ad26d1747) [![GoDoc](https://godoc.org/github.com/SeanDolphin/bqschema?status.png)](http://godoc.org/github.com/SeanDolphin/bqschema)

BQSchema is a package used to created Google Big Query schema directly from Go structs and import BigQuery QueryResponse into arrays of Go structs.

## Usage

You can use BQSchema to automatically load Google Big Query results into arrays of basic Go structs.

~~~ go
// main.go
package main

import (
	"google.golang.org/api/bigquery/v2"
	"github.com/SeanDolphin/bqschema"
)

type person struct{
	Name  string
	Email string
	Age   int
}

func main() {
  	// authorize the bigquery service
  	// create a query
	result, err := bq.Jobs.Query("projectID", query).Do()
	if err == nil {
		var people []person
		err := bqschema.ToStructs(result, &people)
		// do something with people
	}
}

~~~

You can also use BQSchema to create the schema fields when creating new Big Query tables from basic Go structs.

~~~ go
// main.go
package main

import (
	"google.golang.org/api/bigquery/v2"
	"github.com/SeanDolphin/bqschema"
)

type person struct{
	Name  string
	Email string
	Age   int
}

func main() {
  	// authorize the bigquery service
	 table, err := bq.Tables.Insert("projectID","datasetID", bigquery.Table{
		Schema:bqschema.MustToSchema(person{})
	}).Do()
}

~~~
