===========================
Generic Platform Automation
===========================

Overview
========

The purpose of this repo is to is to provide an example of how to implement the platform-automatioon (`p-automator`)
using tools that are largely native to managing PCF. This includes abusing `bosh interpolate` to render configurations
on target containers, which reduces the complexity of using `credhub interpolate` within the container.

Pipelines
~~~~~~~~~

- ext-deps
- install-opsman
- install-pas (deprecated)
- product-pipeline

.. note:: Aside from `product-pipeline`, the other pipelines follow typical naming
    constructs (`pipeline.yml`, `params.yml`). `install-pas` is only for reference,
    and is considered outdated.

`product-pipeline` leverages `common.yml` for reusable elements (e.g. Pivnet
token, etc), and subsequently leverages a product/tile-specific configuration
to provide the parameters. The `config` key is used to hold the tile
configuration, which is transformed into JSON (in the form of an ENV var on a
task container) and later back to YAML using `bosh interpolate`.

.. code-block:: console

		├── ext-deps
		│   ├── params.yml
		│   └── pipeline.yml
		├── install-opsman
		│   ├── params.yml
		│   └── pipeline.yml
		├── install-pas
		│   ├── params.yml
		│   └── pipeline.yml
		└── product-pipeline
				├── azure
				│   ├── azure_log_analytics_params.yml
				│   ├── azure_open_service_broker_pcf_params.yml
				│   ├── healthwatch_params.yml
				│   ├── metrics_params.yml
				│   ├── mongodb_params.yml
				│   ├── mysql_params.yml
				│   ├── pas_srt.yml
				│   ├── rabbitmq_params.yml
				│   ├── redis_params.yml
				│   └── scs_params.yml
				├── common.yml
				└── pipeline.yml

Running the `product-pipeline`
==============================

1. Set the pipeline for the specific tile you're using.

.. code-block:: console

    fly -t us-east-playground set-pipeline -c product-pipeline/pipeline.yml -l product-pipeline/common.yml -l product-pipeline/pas_srt.yml -p install-pas-srt

2. Profit


Contribution
============

This is largely for demo purposes and adopting common patterns. If you want to
make this repo better, feel free to open a PR!
