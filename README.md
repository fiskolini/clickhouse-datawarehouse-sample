# ClickHouse Installation Examples

This repository contains two examples of ClickHouse installations, each with its own directory and configuration files:

## 1. Simple Installation of ClickHouse Server with Grafana

In the `clickhouse-server` directory, you will find a simple installation of ClickHouse Server along with Grafana for monitoring. This installation is designed for a single ClickHouse instance. You can find the container's information in the `docker-compose.yml` file and the relevant configuration files in the `.docker` directory.

### Usage

To set up this installation, follow these steps:

1. Navigate to the `clickhouse-server` directory.
2. Review the configuration files in the `.docker` directory to customize the installation to your requirements.
3. Use the provided Docker Compose file or other instructions to start the ClickHouse Server and Grafana.

### Add _sample_ data
ClickHouse supports some [sample datasets](https://clickhouse.com/docs/en/getting-started/example-datasets) that can be used to populate data and to try the capabilities of ClickHouse.

#### How to set a sample dataset:
1. Make sure you have the clickhouse instance(s) up and running;
2. Download the necessary sample data by running `wget --no-check-certificate --continue https://transtats.bts.gov/PREZIP/On_Time_Reporting_Carrier_On_Time_Performance_1987_present_2022_1.zip`
   1. Their dataset is huge (around 41GB). If you still want to use the whole data, write a simple script that can download the files from 1987 to 2022 and the 12 different parts from each of them (`https://transtats.bts.gov/PREZIP/On_Time_Reporting_Carrier_On_Time_Performance_1987_present_{1987..2022}_{1..12}.zip`).
3. After having the file locally, run the following command to ingest data from them: `ls -1 *.zip | xargs -I{} -P $(nproc) bash -c "echo {}; unzip -cq {} '*.csv' | sed 's/\.00//g' | clickhouse-client --input_format_csv_empty_as_default 1 --query='INSERT INTO ontime FORMAT CSVWithNames'"`


## 2. High Availability Installation of ClickHouse

In the `clickhouse-ha-server` directory, you will find a more complex setup of ClickHouse with high availability. This configuration includes:

- Two ClickHouse instances (clickhouse-01 and clickhouse-02).
- Three dedicated ClickHouse Keepers.
- A CH Proxy load balancer.
- One shard with replication across clickhouse-01 and clickhouse-02.

The configuration files for this installation can be found in the `.docker` directory within the `clickhouse-ha-server` directory.

### Usage

To set up this high availability ClickHouse installation, follow these steps:

1. Navigate to the `clickhouse-ha-server` directory.
2. Review the configuration files in the `.docker` directory to customize the installation to your requirements.
3. Use the provided Docker Compose file or other instructions to start the high availability ClickHouse setup.

## 3. Additional Notes

- Both installations use Docker Compose to manage the containers and simplify deployment.
- The provided configurations are examples and may need to be adjusted for specific production environments.
- Refer to the official ClickHouse documentation for detailed configuration options and advanced usage scenarios.
- The presentation slides can be found [here](https://docs.google.com/presentation/d/17j9e7hAlzq8NS9aIm8xDURhI2WC1MNPqN0SvbeLJv5M/edit?usp=sharing) (Google Slides) and a copy of them can be found in the `presentation.pdf` file.