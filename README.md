# EvoBench - Proof of Concept

In the following, a description for reproducing the proof of concept experiments from *EvoBench: Benchmarking Schema Evolution in NoSQL* is given.
The required Docker containers are available on [DockerHub](https://hub.docker.com) and [Zenodo](https://zenodo.org).


## Running the Experiments

The only requirement to run the experiments is to have [Docker](https://www.docker.com/) installed.
The needed configuration files are already available in the Docker container of the benchmark.
With the following command a short test run can be executed:

```bash
docker run -t --rm \
--network host \
-v "/var/run/docker.sock:/var/run/docker.sock" \
-v "$(pwd)/results:/opt/evobench/results" \
dbishagen/evobench:20210617.1-alpha \
-c ./config/experimental_setup_3-test.json
```

For the use of own and/or customized configuration files they can be included as follows: </br> `-v "$(pwd)/config:/opt/evobench/config:ro"`

The elements below on the list are the JSON configuration files that are related to the several experiments. They can be passed to the benchmark container as a *command* as in the *short test* example:

- Effects of SES-Side, DB-Side and Different Entity Sizes:  `experimental_setup_1.json`
- Comparison Between MongoDB and Cassandra:  `experimental_setup_2.json`
- Effects of a Revised Version of the Schema Evolution System: `experimental_setup_2.json`
- Differences Between Stepwise and Composite & Comparision of a Real and a Syntetic Data Set: `experimental_setup_4_and_5.json`
  
**NOTE:** The initial *Java heap size* and the maximum *Java heap size* of the *Schema Evolution System* (SES) is set to 8G and 32G respectively in the JSON configuration files.


## Building the Data Sets with the Data Generator

The datasets used in the experiments can be recreated using our data generator (based on the [json-data-generator](https://github.com/vincentrussell/json-data-generator/tree/json-data-generator-1.12) library). 
The Docker container includes folders with schema files for the following datasets:

- unibench-sf0_1-small
- unibench-sf0_1-big
- mediawiki-gen

The following command allows to generate the `unibench-sf0_1-small` data set:

```bash
docker run -t --rm \
-v "$(pwd)/datasets:/opt/datagenerator/datasets" \
dbishagen/evobench-data-generator:20210617.1-alpha \
-s ./schema/unibench-sf0_1-small -o ./datasets/unibench-sf0_1-small
```

To start the data generator with the *user* and *group* of the host system the following docker configuration can be used: </br> `-u $(id -u ${USER}):$(id -g ${USER})`
