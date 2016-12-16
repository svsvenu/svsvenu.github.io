---
layout: post
title: Camel split and aggregate with Jboss data grid for persistence
---

## Introduction

OOTB camel provides a way to split a large files, process it and aggregate the processed chunks, however the process
is done in memory that comes with the obvious pitfalls of volatility and availability. If the split and aggregate processed
needs to be backed up with persistence, then we need to implement the [RecoverableAggregationRepository](
https://camel.apache.org/maven/camel-2.15.0/camel-core/apidocs/org/apache/camel/spi/RecoverableAggregationRepository.html).

![_config.yml]({{ site.baseurl }}/images/camel_split_aggregate.png)

## The splitter

The very first step in the process is the splitter which will split a file into chunks of our choice, to implement
a customized splitter, we need to have a class that implements the java.util.Iterator interface, camel will then
call the next, hasNext etc for each of the camel blocks that follow the splitter call. Since the call iterates, its an
excellent way to stream in large files, for instance we can stream in an xml file by node.

## The processing

The processing bean, acts on the body created in the splitter, it will most likely end up putting the processed results
back into the exchange,

## Aggregation

The aggregation process then kicks off at the tail end of the chunk processing, first camel calls the implementation
of AggregationStrategy ( implemented inline as a private static class ArrayListAggregationStrategy ) , after method execution is complete
camel checks to see if there is a key/value pair in our persistence store (JDG) by calling the get method on the class
implementing RecoverableAggregationRepository ( In our case this is RecoverableAggregationRepositoryImpl )
class. The next step in the process would be a call to the add method on the same class, Since our strategy is to keep
aggregating to the Arraylist,after every call, the arraylist represents the current and transient set of results accumulated.

## persistence

As the title of the blog states, persistence is achieved by implementing