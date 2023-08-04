---
title: Dagster
date: "2023-08-03T22:40:32.169Z"
description: A data orchestration tool that you should be aware of.
---

### Why do you need Dagster?
If you are looking for good UI to see beautiful data pipelines, machine learning pipelines and easy-to-configure kind of tool for data pipelining. This is one of the best you can try out.

### Software-defined assets

- an object in a persistent storage such as a table, file, or persisted machine learning model.
- it is a Dagster object that couples an asset to the function and upstream assets used to produce its contents.
- it enables a declarative approach to data management. Here the code is the source of truth on what data assets should exist and how to assets are computed.
- To refer the asset, there is handle called ```AssetKey```
- The contents of the software-defined assets will have a set of upstream asset keys.
- To compute the contents of these assets, there is a function called ```op``` and it it computes the content from the upstream dependencies.

As we store the results to persistent storage, after running its op, we have actually **Materialized** the asset.

You can **materialize** the assets from ```Dagit``` or by  invoking Python APIs.

If you're running Dagster on local or over cloud, it is by default stores the **materialized** assets to pickle files but you can change it's behavior which is fully changeable using ```I/O managers```.


### Relevant APIs #
```@asset``` is a decorator used to define assets.
```SourceAsset``` is a class that describes an asset but it doesn't define how to compute it. ```SourceAsset```s are used represent assets that other assets or jobs depend on, in settings where they cannot be materialized themselves.

### A basic software-defined asset #
The easiest way to create a software-defined asset is with the ```@asset``` decorator.
```python
from dagster import asset

@asset
def cities_by_business_dna():
    return ['Ahmedabad', 'Mumbai', 'Kolkata', 'Bangalore']
```

By default, the decorated function here is ```cities_by_business_dna```, is the asset key. The decorated function forms the asset's op: It's responsible for producing asset's contents. The asset is an independent asset here.



### Basic dependencies
Let's pick our previous example as it is but with a new city made it to the list of business dna ```Surat```

To define an asset dependency is to include an upstream asset name as argument to the decorated function.

The following example simply puts the contents of ```cities_by_business_dna``` into ```latest_cities_by_business_dna``` function and computes the contents.

```python
@asset
def cities_by_business_dna():
    return ['Ahmedabad', 'Mumbai', 'Kolkata', 'Bangalore']


@asset
def latest_cities_by_business_dna(cities_by_business_dna):
    return cities_by_business_dna + ['Surat']
```
That's all! it is one of the easiest way to define assets.


### Explicit dependencies in defining assets
Explicit dependencies are referred to as dependencies by matching argument namesto upstream asset names. Sounds complex right! Don't worry too much.

```python
from dagster import AssetIn, asset

@asset
def cities_by_business_dna():
    return ['Ahmedabad', 'Mumbai', 'Kolkata', 'Bangalore']


@asset(ins={"cities": AssetIn("cities_by_business_dna")})
def latest_cities_by_business_dna(cities):
    return cities + ['Surat']
```

In above code snippet, ```ins={"cities": AssetIn("cities_by_business_dna")}``` can be interpreted in a simple english terms that the contents of the asset with the key ```cities_by_business_dna``` will be provided to the function argument named ```cities```

To identify the asset explicitly, ```AssetIn``` takes asset keys in it.
Let's take a look at an example on this.

```python
from dagster import AssetIn, asset


# If the cities key has a single segment, you can specify it with a string:
@asset(ins={"cities": AssetIn(key="cities_by_business_dna")})
def latest_cities_by_business_dna(cities):
    return cities + ['Surat']


# If it has multiple segments, you can provide a list:
@asset(ins={"cities": AssetKey(key=["some_db_schema", "cities_by_business_dna"])})
def another_latest_cities_by_business_dna(cities):
    return cities + ['Surat', 'Pune']
```

Let's learn more with [**External asset dependencies**](004-external-asset-dependencies.md)
