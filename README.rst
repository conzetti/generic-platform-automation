===========================
Generic Platform Automation
===========================

Overview
========

The purpose of this repo is to is to provide an example of how to implement Pivotal's platform-automation
(``platform-automation`` slug on Pivnet) using tools that are largely native to managing PCF. This includes abusing ``bosh
interpolate`` to render configurations on target containers, which reduces the complexity of using ``credhub interpolate``
within the container.

Pipelines
~~~~~~~~~

- ext-deps
- install-opsman
- product-pipeline

**note**:
   Aside from ``product-pipeline``, the other pipelines follow typical naming constructs (``pipeline.yml``, ``params.yml``).
   ``install-pas`` is only for reference, and is considered outdated. Be careful when putting certificates into Credhub
   that will be later pulled into a product config; due to the way the newlines are rendered, you will more than likely
   want to make the certificate a multi-line string, using ``\r\n`` at the end of each new line.

   ``product-pipeline`` leverages ``common.yml`` for reusable elements (e.g. Pivnet token, etc), and subsequently leverages
   a product/tile-specific configuration to provide the parameters. The ``config`` key is used to hold the tile
   configuration, which is transformed into JSON (in the form of an ENV var on a task container) and later back to YAML
   using ``bosh interpolate``.

.. code-block:: console

   ├── ext-deps
   │   ├── params.yml
   │   └── pipeline.yml
   ├── install-opsman
   │   ├── params.yml
   │   └── pipeline.yml
   ├── product-pipeline
   │   ├── azure
   │   │   ├── a9s-mongodb
   │   │   │   └── 2.1.0
   │   │   │       └── params.yml
   │   │   ├── azure-log-analytics
   │   │   │   └── 1.3.1
   │   │   │       └── params.yml
   │   │   ├── azure-open-service-broker
   │   │   │   └── 0.12.0
   │   │   │       └── params.yml
   │   │   ├── healthwatch
   │   │   │   └── 1.4.4
   │   │   │       └── params.yml
   │   │   ├── metrics
   │   │   │   └── 1.6.0
   │   │   │       └── params.yml
   │   │   ├── mysql
   │   │   │   └── 2.4.4
   │   │   │       └── params.yml
   │   │   ├── pas
   │   │   │   └── 2.2.13
   │   │   │       ├── full
   │   │   │       │   └── params.yml
   │   │   │       └── small
   │   │   │           └── params.yml
   │   │   ├── rabbitmq
   │   │   │   └── 1.15.4
   │   │   │       └── params.yml
   │   │   ├── redis
   │   │   │   └── 2.0.1
   │   │   │       └── params.yml
   │   │   └── scs
   │   │       └── 2.0.7
   │   │           └── params.yml
   │   ├── common.yml
   │   └── pipeline.yml
   └── README.rst


Running the ``product-pipeline``
==============================

1. Set the pipeline for the specific tile you're using (the example, below, uses PAS SRT).

.. code-block:: console

    fly -t us-east-playground set-pipeline -c product-pipeline/pipeline.yml -l product-pipeline/common.yml -l product-pipeline/azure/pas/2.1.13/small/params.yml -p install-pas-srt

2. Profit


Contribution
============

This is largely for demo purposes and adopting common patterns. If you want to make this repo better, feel free to open
a PR!
