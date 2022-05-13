# Data Ingestion

**TFRecord** [**https://oreil.ly/2-MuJ**](https://oreil.ly/2-MuJ)****

**TFRecord** is a lightweight format optimized for _streaming_ large datasets. TFRecord files containing the data represented as `tf.Example` data structures**.** While in practice, most TensorFlow users store serialized example Protocol Buffers in TFRecord files, the TFRecord file format actually supports any binary data, as shown in the following:

```python
import tensorflow as tf

with tf.io.TFRecordWriter("test.tfrecord") as w:
    w.write(b"First record")
    w.write(b"Second record")

for record in tf.data.TFRecordDataset("test.tfrecord"):
    print(record)

tf.Tensor(b'First record', shape=(), dtype=string)
tf.Tensor(b'Second record', shape=(), dtype=string)
```

Storing your data as _TFRecord_ and `tf.Examples` provides a few benefits:

1. The data structure is system independent since it relies on Protocol Buffers, a cross-platform, cross-language library, to serialize data.
2. TFRecord is optimized for downloading or writing large amounts of data quickly.
3. `tf.Example`, the data structure representing every data row within TFRecord, is also the default data structure in the TensorFlow ecosystem and, therefore, is used in all TFX components.

{% hint style="info" %}
The process of ingesting, splitting, and converting the datasets is performed by the `ExampleGen` component. Datasets can be read from local and remote folders as well as requested from data services like Google Cloud BigQuery.
{% endhint %}

## ExampleGen

## Ingesting Local Data Files

It can ingest a few data structures, including _comma-separated value_ files (CSVs), precomputed TFRecord files, and serialization outputs from Apache Avro and Apache Parquet.

### Converting comma-separated data to tf.Example

```python
import os
from tfx.components import CsvExampleGen
from tfx.utils.dsl_utils import external_input

base_dir = os.getcwd()
data_dir = os.path.join(os.pardir, "data")
examples = external_input(os.path.join(base_dir, data_dir))
example_gen = CsvExampleGen(input=examples)

context.run(example_gen)
```

#### FOLDER STRUCTURE

It is expected that the input path of _ExampleGen_ only contains the data files.&#x20;

The component tries to consume all existing files within the path level. Any additional files (e.g., metadata files) can’t be consumed by the component and make the component step fail.&#x20;

The component is also not stepping through existing subdirectories unless it is configured as an input pattern.

### Importing existing TFRecord Files

```python
import os
from tfx.components import ImportExampleGen
from tfx.utils.dsl_utils import external_input

base_dir = os.getcwd()
data_dir = os.path.join(os.pardir, "tfrecord_data")
examples = external_input(os.path.join(base_dir, data_dir))
example_gen = ImportExampleGen(input=examples)

context.run(example_gen)
```

### Converting Parquet-serialized data to tf.Example

TFX includes executor classes for loading different file types, including Parquet serialized data.

The following example shows how you can override the `executor_class` to change the loading behavior. Instead of using the `CsvExampleGen` or `ImportExampleGen` components, we will use a generic file loader component `FileBasedExampleGen`, which allows an override of the `executor_class`

```python
from tfx.components import FileBasedExampleGen
from tfx.components.example_gen.custom_executors import parquet_executor
from tfx.utils.dsl_utils import external_input

examples = external_input(parquet_dir_path)
example_gen = FileBasedExampleGen(
    input=examples,
    executor_class=parquet_executor.Executor) 
```

{% hint style="success" %}
If we would like to load new file types into our pipeline, we can override the `executor_class` instead of writing a completely new component.
{% endhint %}

### Converting Avro-serialized data to tf.Example

```python
from tfx.components import FileBasedExampleGen
from tfx.components.example_gen.custom_executors import avro_executor
from tfx.utils.dsl_utils import external_input

examples = external_input(avro_dir_path)
example_gen = FileBasedExampleGen(
    input=examples,
    executor_class=avro_executor.Executor)
```

### Converting your custom data to TFRecord data structures

[https://oreil.ly/bmlp-git-convert\_data\_to\_tfrecordspy](https://oreil.ly/bmlp-git-convert\_data\_to\_tfrecordspy) in _chapters/data\_ingestion_.

```python
import csv
import tensorflow as tf

original_data_file = os.path.join(
    os.pardir, os.pardir, "data",
    "consumer-complaints.csv")
tfrecord_filename = "consumer-complaints.tfrecord"
tf_record_writer = tf.io.TFRecordWriter(tfrecord_filename) 1

with open(original_data_file) as csv_file:
    reader = csv.DictReader(csv_file, delimiter=",", quotechar='"')
    for row in reader:
        example = tf.train.Example(features=tf.train.Features(feature={ 2
            "product": _bytes_feature(row["product"]),
            "sub_product": _bytes_feature(row["sub_product"]),
            "issue": _bytes_feature(row["issue"]),
            "sub_issue": _bytes_feature(row["sub_issue"]),
            "state": _bytes_feature(row["state"]),
            "zip_code": _int64_feature(int(float(row["zip_code"]))),
            "company": _bytes_feature(row["company"]),
            "company_response": _bytes_feature(row["company_response"]),
            "consumer_complaint_narrative": \
                _bytes_feature(row["consumer_complaint_narrative"]),
            "timely_response": _bytes_feature(row["timely_response"]),
            "consumer_disputed": _bytes_feature(row["consumer_disputed"]),
        }))
        tf_record_writer.write(example.SerializeToString()) 3
    tf_record_writer.close()
```

## Ingesting Remote Data Files

The ExampleGen component can read files from remote cloud storage buckets like Google Cloud Storage or AWS Simple Storage Service (S3)

```python
examples = external_input("gs://example_compliance_data/")
example_gen = CsvExampleGen(input=examples)
```

{% hint style="warning" %}
Access to private cloud storage buckets requires setting up the cloud provider credentials. The setup is provider specific. AWS is authenticating users through a user-specific access key and access secret. To access private AWS S3 buckets, you need to create a user access key and secret. In contrast, the Google Cloud Platform (GCP) authenticates users through service accounts. To access private GCP Storage buckets, you need to create a service account file with the permission to access the storage bucket.
{% endhint %}

## Ingesting Data Directly from Databases

TFX provides two components to ingest datasets directly from databases.

* `BigQueryExampleGen` component to query data from BigQuery tables
* `PrestoExampleGen` component to query data from Presto databases

### From Google Cloud BigQuery

```python
from tfx.components import BigQueryExampleGen

query = """
    SELECT * FROM `<project_id>.<database>.<table_name>`
"""
example_gen = BigQueryExampleGen(query=query)
```

In TFX versions greater than 0.22.0,

```python
from tfx.extensions.google_cloud_big_query.example_gen \
    import component as big_query_example_gen_component
big_query_example_gen_component.BigQueryExampleGen(query=query)
```

### From Presto databases

```python
from proto import presto_config_pb2
from presto_component.component import PrestoExampleGen

query = """
    SELECT * FROM `<project_id>.<database>.<table_name>`
"""
presto_config = presto_config_pb2.PrestoConnConfig(
    host='localhost',
    port=8080)
example_gen = PrestoExampleGen(presto_config, query=query)
```

Since TFX version 0.22, the PrestoExampleGen requires a separate installation process. After installing the protoc compiler

```bash
$ git clone git@github.com:tensorflow/tfx.git && cd tfx/
$ git checkout v0.22.0
$ cd tfx/examples/custom_components/presto_example_gen
$ pip install -e .
```

## Data Preparation

### Splitting Datasets

#### Splitting one dataset into subsets

three-way split: training, evaluation, and test sets with a ratio of 6:2:2. The ratio settings are defined through the hash\_buckets:

```python
from tfx.components import CsvExampleGen
from tfx.proto import example_gen_pb2
from tfx.utils.dsl_utils import external_input

base_dir = os.getcwd()
data_dir = os.path.join(os.pardir, "data")
output = example_gen_pb2.Output(
    split_config=example_gen_pb2.SplitConfig(splits=[
        example_gen_pb2.SplitConfig.Split(name='train', hash_buckets=6),
        example_gen_pb2.SplitConfig.Split(name='eval', hash_buckets=2),
        example_gen_pb2.SplitConfig.Split(name='test', hash_buckets=2)
    ]))

examples = external_input(os.path.join(base_dir, data_dir))
example_gen = CsvExampleGen(input=examples, output_config=output)

context.run(example_gen)
```

After the execution of the example\_gen object, we can inspect the generated artifacts by printing the list of the artifacts:

```python
for artifact in example_gen.outputs['examples'].get():
    print(artifact)

Artifact(type_name: ExamplesPath,
    uri: /path/to/CsvExampleGen/examples/1/train/, split: train, id: 2)
Artifact(type_name: ExamplesPath,
    uri: /path/to/CsvExampleGen/examples/1/eval/, split: eval, id: 3)
Artifact(type_name: ExamplesPath,
    uri: /path/to/CsvExampleGen/examples/1/test/, split: test, id: 4)
```

{% hint style="warning" %}
If we don’t specify any output configuration, the ExampleGen component splits the dataset into a training and evaluation split with a ratio of 2:1 by default.
{% endhint %}

### Preserving existing splits

Original directory structure:

```
└── data
    ├── train
    │   └─ 20k-consumer-complaints-training.csv
    ├── eval
    │   └─ 4k-consumer-complaints-eval.csv
    └── test
        └─ 2k-consumer-complaints-test.csv
```

We can preserve the existing input split by defining this input configuration:

```python
import os

from tfx.components import CsvExampleGen
from tfx.proto import example_gen_pb2
from tfx.utils.dsl_utils import external_input

base_dir = os.getcwd()
data_dir = os.path.join(os.pardir, "data")

input = example_gen_pb2.Input(splits=[
    example_gen_pb2.Input.Split(name='train', pattern='train/*'),
    example_gen_pb2.Input.Split(name='eval', pattern='eval/*'),
    example_gen_pb2.Input.Split(name='test', pattern='test/*')
])

examples = external_input(os.path.join(base_dir, data_dir))
example_gen = CsvExampleGen(input=examples, input_config=input)
```

### Spanning Datasets

Think of a span as a snapshot of data. Every hour, day, or week, a batch _extract, transform, load_ (ETL) process could make such a data snapshot and create a new span. For this scenario, the `ExampleGen` component allows us to use _spans_.

A span can replicate the existing data records. As shown in the following, _export-1_ contains the data from the previous _export-0_ as well as newly created records that were added since the _export-0_ export:

```
└── data
    ├── export-0
    │   └─ 20k-consumer-complaints.csv
    ├── export-1
    │   └─ 24k-consumer-complaints.csv
    └── export-2
        └─ 26k-consumer-complaints.csv
```

We can now specify the patterns of the spans. The input configuration accepts a `{SPAN}` placeholder, which represents the number (0, 1, 2, …) shown in our folder structure. With the input configuration, the `ExampleGen` component now picks up the “latest” span. In our example, this would be the data available under folder _export-2_:

```python
from tfx.components import CsvExampleGen
from tfx.proto import example_gen_pb2
from tfx.utils.dsl_utils import external_input

base_dir = os.getcwd()
data_dir = os.path.join(os.pardir, "data")

input = example_gen_pb2.Input(splits=[
    example_gen_pb2.Input.Split(pattern='export-{SPAN}/*')
])

examples = external_input(os.path.join(base_dir, data_dir))
example_gen = CsvExampleGen(input=examples, input_config=input)
context.run(example_gen)
```

Of course, the input definitions can also define subdirectories if the data is already split:

```
input = example_gen_pb2.Input(splits=[
    example_gen_pb2.Input.Split(name='train',
                                pattern='export-{SPAN}/train/*'),
    example_gen_pb2.Input.Split(name='eval',
                                pattern='export-{SPAN}/eval/*')
])
```

## Versioning Datasets

This feature is currently not supported by the TFX `ExampleGen` component. If you would like to version your datasets, you can use third-party data versioning tools and version the data before the datasets are ingested into the pipeline. Unfortunately, none of the available tools will write the metadata information to the TFX ML MetadataStore directly. TFX supports storing the file name and path of the ingested data in the ML MetadataStore.

### Data versioning tools

1. [Data Version Control (DVC)](https://dvc.org) : an open source version control system for machine learning projects. It lets you commit hashes of your datasets instead of the entire dataset itself. Therefore, the state of the dataset is tracked (e.g., via git), but the repository isn’t cluttered with the entire dataset.
2. [Pachyderm](https://www.pachyderm.com) : an open source machine learning platform running on Kubernetes. It originated with the concept of versioning for data (“Git for data”) but has now expanded into an entire data platform, including pipeline orchestration based on data versions.

## Ingestion Strategies

### Structured Data

Structured data is often stored in a **database** or on a disk in **file format**, supporting tabular data. If the data exists in a database, we can either export it to **CSVs** or consume the data directly with the **`PrestoExampleGen` ** or the **`BigQueryExampleGen` ** components (if the services are available).

Data available on a disk stored in file formats supporting tabular data should be converted to CSVs and ingested into the pipeline with the `CsvExampleGen` component. Should the amount of data **grow beyond a few hundred megabytes**, you should consider converting the data into **TFRecord** files or store the data with **Apache Parquet**.\


### Text Data for Natural Language Problems

Text corpora can snowball to a considerable **size**. To ingest such datasets efficiently, we recommend converting the datasets to **TFRecord** or **Apache Parquet** representations.  this allows an efficient and incremental loading of the corpus documents. The ingestion of the corpora from a database is also possible; however, we recommend considering network traffic costs and bottlenecks.

### Image Data for Computer Vision Problems

convert image datasets from the image files to **TFRecord** files, but **not to decode** the images. The compressed images can be stored in tf.Example records as **byte strings**:

```python
import tensorflow as tf

base_path = "/path/to/images"
filenames = os.listdir(base_path)

def generate_label_from_path(image_path):
    ...
    return label

def _bytes_feature(value):
    return tf.train.Feature(bytes_list=tf.train.BytesList(value=[value]))

def _int64_feature(value):
    return tf.train.Feature(int64_list=tf.train.Int64List(value=[value]))

tfrecord_filename = 'data/image_dataset.tfrecord'

with tf.io.TFRecordWriter(tfrecord_filename) as writer:
    for img_path in filenames:
        image_path = os.path.join(base_path, img_path)
        try:
            raw_file = tf.io.read_file(image_path)
        except FileNotFoundError:
            print("File {} could not be found".format(image_path))
            continue
        example = tf.train.Example(features=tf.train.Features(feature={
            'image_raw': _bytes_feature(raw_file.numpy()),
            'label': _int64_feature(generate_label_from_path(image_path))
        }))
        writer.write(example.SerializeToString())
```

## Resources

&#x20;Reading files from AWS S3 requires Apache Beam 2.19 or higher, which is supported since TFX version 0.22.

See the documentation for more details on [managing AWS Access Keys](https://oreil.ly/Dow7L).

See the documentation for more details on [how to create and manage service accounts](https://oreil.ly/6y8WX).

Visit the proto-lens GitHub for [details on the `protoc` installation](https://oreil.ly/h6FtO).



``

###
