---
linktitle: "M3 Quickstart"
weight: 1
---

# Creating a Single Node M3 Cluster with Docker

This guide shows how to use Docker to install and configure M3, create a single-node cluster, and read and write metrics to it.

{{% notice warning %}}
Deploying a single-node M3 cluster is a great way to experiment with M3 and get an idea of what it has to offer, but is not designed for production use. To run M3 in clustered mode with a separate M3Coordinator, [read the clustered mode guide](/docs/how_to/cluster_hard_way).
{{% /notice %}}

## Prerequisites

-   **Docker**: You don't need [Docker](https://www.docker.com/get-started) to run M3DB, but it is the simplest and quickest way.
    -   If you use Docker Desktop, we recommend the following minimum _Resources_ settings.
        -   _CPUs_: 2
        -   _Memory_: 8GB
        -   _Swap_: 1GB
        -   _Disk image size_: 16GB
-   **JQ**: This example uses [jq](https://stedolan.github.io/jq/) to format the output of API calls. It is not essential for using M3.
-   **curl**: This example uses curl for communicating with M3 endpoints. You can also use alternatives such as [Wget](https://www.gnu.org/software/wget/) and [HTTPie](https://httpie.org/).

## Start Docker Container

By default the official M3 Docker image configures a single M3 instance as one binary containing:

-   An M3DB storage instance for time series storage. It includes an embedded tag-based metrics index and an etcd server for storing the cluster topology and runtime configuration.
-   An M3Coordinator instance for writing and querying tagged metrics, as well as managing cluster topology and runtime configuration.

The Docker container exposes three ports:

-   `7201` to manage the cluster topology, you make most API calls to this endpoint.
-   `7203` for Prometheus to scrape the metrics produced by the M3DB and M3Coordinator instances.

{{< tabs name="start_container" >}}
{{% tab name="Command" %}}

{{% notice tip %}}
The command below creates [a persistent data directory](https://docs.docker.com/storage/volumes/) on the host operating system to maintain data persistence between container restarts. If you have already followed this tutorial, when you create a namespace in the next step, the namespace already exists. You can clear the data by deleting the contents of the _m3db_data_ folder, or deleting the namespace with [the DELETE endpoint](/docs/operational_guide/namespace_configuration/#deleting-a-namespace).
{{% /notice %}}

```shell
docker run -p 7201:7201 -p 7203:7203 --name m3db -v $(pwd)/m3db_data:/var/lib/m3db quay.io/m3db/m3dbnode:latest
```

{{% /tab %}}
{{% tab name="Output" %}}

<!-- TODO: Perfect image, pref with terminalizer -->

![Docker pull and run](/docker-install.gif)

{{% /tab %}}
{{< /tabs >}}

{{% notice info %}}
When running the command above on Docker for Mac, Docker for Windows, and some Linux distributions you may see errors about settings not being at recommended values. Unless you intend to run M3DB in production on macOS or Windows, you can ignore these warnings.
{{% /notice %}}

## Configuration

This example uses this [sample configuration file](https://github.com/m3db/m3/raw/master/src/dbnode/config/m3dbnode-local-etcd.yml) by default.

The file groups configuration into `coordinator` or `db` sections that represent the `M3Coordinator` and `M3DB` instances of single-node cluster.

{{% notice tip %}}
You can find more information on configuring M3DB in the [operational guides section](/operational_guide/).
{{% /notice %}}

{{% fileinclude file="quickstart/common-steps.md" %}}