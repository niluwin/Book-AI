# Introduction

## Introduction

## TFX

Google faced the same problem internally and decided to develop a platform to simplify the pipeline definitions and to minimize the amount of task boilerplate code to write. The open source version of Google’s internal ML pipeline framework is TFX.

TFX provides a variety of pipeline components that cover a good number of use cases. Few oft the tools are:

* Data ingestion with `ExampleGen`
* Data validation with `StatisticsGen`, `SchemaGen`, and the `ExampleValidator`
* Data preprocessing with `Transform`
* Model training with `Trainer`
* Checking for previously trained models with `ResolverNode`
* Model analysis and validation with `Evaluator`
* Model deployments with `Pusher`

![TFX as part of ML pipelines](<../../.gitbook/assets/image (154).png>)

![TFX components and libraries](<../../.gitbook/assets/image (116).png>)

```shell
pip install tfx
```

```python
import tensorflow_data_validation as tfdv
import tensorflow_transform as tft
import tensorflow_transform.beam as tft_beam
...

from tfx.components import ExampleValidator
from tfx.components import Evaluator
from tfx.components import Transform
...
```

![Component overview](<../../.gitbook/assets/image (105).png>)

![Storing metadata with MLMD](<../../.gitbook/assets/image (4).png>)

You can start an interactive pipeline by importing the required packages:

```python
import tensorflow as tf
from tfx.orchestration.experimental.interactive.interactive_context import \
    InteractiveContext

context = InteractiveContext()

from tfx.components import StatisticsGen
statistics_gen = StatisticsGen(examples=example_gen.outputs['examples'])

context.run(statistics_gen)

context.show(statistics_gen.outputs['statistics'])

for artifact in statistics_gen.outputs['statistics'].get():
    print(artifact.uri)
```

### Alternatives

![](<../../.gitbook/assets/image (132).png>)

## Apache Beam

Beam is an open source tool for defining and executing data-processing jobs. it is essential if you wish to write custom components.

Pipelines also require well-managed distributed processing, which is why TFX leverages Apache Beam. This means that you can run the same data pipeline on Apache Beam’s DirectRunner, Apache Spark, Apache Flink, or Google Cloud Dataflow without a single change in the pipeline description.

Apache Beam can be used to describe batch processes, streaming operations, and data pipelines.

It has two uses in TFX pipelines:

* it runs under the hood of many TFX components to carry out processing steps like data validation or data preprocessing.
* it can be used as a pipeline orchestrator

Apache Beam’s abstraction is based on two concepts:

* collections: describe operations where data is being read or written from or to a given file or stream.
* transformations: describe ways to manipulate the data.

```shell
pip install apache-beam
```

For GCP:

```
pip install 'apache-beam[gcp]'
```

For AWS

```
pip install 'apache-beam[boto]'
```

### Basic collection example

The following example shows how to read a text file and return all lines:

```python
import apache_beam as beam

with beam.Pipeline() as p: 
    lines = p | beam.io.ReadFromText(input_file) 
```

```python
with beam.Pipeline() as p:
    ...
    output | beam.io.WriteToText(output_file)
```

### Basic transformation example

```python
counts = (
    lines
    | 'Split' >> beam.FlatMap(lambda x: re.findall(r'[A-Za-z\']+', x))
    | 'PairWithOne' >> beam.Map(lambda x: (x, 1))
    | 'GroupAndSum' >> beam.CombinePerKey(sum))
```

```
def format_result(word_count):
    """Convert tuples (token, count) into a string"""
    (word, count) = word_count
    return "{}: {}".format(word, count)

output = counts | 'Format' >> beam.Map(format_result)
```

{% code title="basic_pipeline.py" %}
```python
import re

import apache_beam as beam
from apache_beam.io import ReadFromText
from apache_beam.io import WriteToText
from apache_beam.options.pipeline_options import PipelineOptions
from apache_beam.options.pipeline_options import SetupOptions

input_file = "gs://dataflow-samples/shakespeare/kinglear.txt" 
output_file = "/tmp/output.txt"

# Define pipeline options object.
pipeline_options = PipelineOptions()

with beam.Pipeline(options=pipeline_options) as p: 
    # Read the text file or file pattern into a PCollection.
    lines = p | ReadFromText(input_file) 

    # Count the occurrences of each word.
    counts = ( 
        lines
        | 'Split' >> beam.FlatMap(lambda x: re.findall(r'[A-Za-z\']+', x))
        | 'PairWithOne' >> beam.Map(lambda x: (x, 1))
        | 'GroupAndSum' >> beam.CombinePerKey(sum))

    # Format the counts into a PCollection of strings.
    def format_result(word_count):
        (word, count) = word_count
        return "{}: {}".format(word, count)

    output = counts | 'Format' >> beam.Map(format_result)

    # Write the output using a "Write" transform that has side effects.
    output | WriteToText(output_file)
```
{% endcode %}

The results of the transformations can be found in the designated text file:

```bash
$ head /tmp/output.txt*
KING: 243
LEAR: 236
DRAMATIS: 1
PERSONAE: 1
king: 65
...
```

If you want to execute this pipeline on different Apache Beam runners like Apache Spark or Apache Flink, you will need to set the pipeline configurations through the `pipeline_options` object

## Resources

In supervised classification problems with multiple classes as outputs, it’s often necessary to convert from a category to a vector such as (0,1,0), which is a one-hot vector, or from a list of categories to a vector such as (1,1,0), which is a multi-hot vector.

Google started an internal project called Sibyl in 2007 to manage an internal machine learning production pipeline. However, in 2015, the topic gained wider attention when D. Sculley et al. published their learnings of machine learning pipelines, [“Hidden Technical Debt in Machine Learning Systems”](https://oreil.ly/qVlYb).

D. Sculley et al., “Hidden Technical Debt in Machine Learning Systems,” _Google, Inc._ (2015).
