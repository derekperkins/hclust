# hclust

Package for peforming hierachical clustering in Go.

## Methods

Distance matrices can be calculated using the binary, Canberra, Euclidean, Jaccard,
Manhattan or maximum metrics. The linkage methods available are: average, centroid,
complete, McQuitty, median, single and Ward. The linkage method algorithms
used are as recommended in [Müllner](https://arxiv.org/abs/1109.2378). Briefly,
the single method was implemented using MST, the average, complete, McQuitty and
Ward methods are implemented using the nearest-neighbor chain algorithm and the
centroid and median methods were implemented using the generic algorithm. Leaf
optimization is performed using the improved optimization approach of
[Bar-Jospeh, et al.](https://www.ncbi.nlm.nih.gov/pubmed/11472989)

## Installation

`go get github.com/knightjdr/hclust`

## Usage

`import "github.com/knightjdr/hclust"`

### Distance

Setting the transpose argument to true will calculate distances between columns
as apposed to rows. Euclidean distances will be calculated if an invalid metric
is supplied.

`hclust.Distance(matrix [][]float64, metric string, transpose bool) [][]float64`

### Cluster

Cluster requires a symmetric distance matrix and a vector of names for the
rows/columns. It will return a dendrogram, a newick tree and the names vector
sorted based on the clustering order. Each row in the dendrogram is a node
containing the leafs/subnodes and the length of the branches to the leafs/subnodes.

```
type Hclust struct {
	Dendrogram []SubCluster
	Newick     string
	Order      []string
}

type SubCluster struct {
	Leafa   int
	Leafb   int
	Lengtha float64
	Lengthb float64
	Node    int
}

hclust.Cluster(matrix [][]float64, names []string, method string, optimize bool) (hclust Hclust, err error)
```

## Benchmarks

Benchmarking tests were performed using a single core on a 3.7 GHz Quad-Core
Intel Xeon E5 processor with 32 GB RAM.

Distance matrix benchmarks were measured using an input table with 4157 rows
and 199 columns, with the distances calculated between rows.

| Distance metric  | Execution time  |
| ---------------- | --------------- |
| Canberra         | 5.1s            |
| Euclidean        | 1.6s            |
| maximum          | 2.8s            |

Clustering benchmarks were measured using a symmetric distance matrix with dimensions
4157×4157.

| Linkage Method  | Execution time |
| --------------- | -------------- |
| complete        | 1.7s           |
| median          | 1.82s          |
| single          | 1.78s          |

## Tests

`go test`

© 2018 James Knight.
