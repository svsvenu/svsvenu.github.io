---
layout: post
title: Camel split and aggregate with Jboss data grid for persistence
---

## Introduction

OOTB camel provides a way to split a large files, process it and aggregate the processed chunks, however the process
is done in memory that comes with the obvious pitfalls of volatility and availability. If the split and aggregate processed
needs to be backed up with persistence, then we need to implement the [RecoverableAggregationRepository](
https://camel.apache.org/maven/camel-2.15.0/camel-core/apidocs/org/apache/camel/spi/RecoverableAggregationRepository.html)