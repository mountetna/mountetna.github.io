---
layout: default
---
# Modeling

## Introduction
Data modeling captures the structure of the research project. A *model* corresponds to an entity in the experiment. Each entity has many properties, which are described by *attributes* of the model. For example, we may have a sample model with two attributes:

```
sample { identifier :name, parent :patient, file :picture }
```

The data attaching to a model is a set of records for each entity in the experiment. For example, we may have three sample records:

```
{ name: "sample1", patient: "patient1", picture: "metis://sample1.png" }
{ name: "sample2", patient: "patient1", picture: "metis://sample2.png" }
{ name: "sample3", patient: "patient2", picture: "metis://sample3.png" }
```

As the set of records for a model may often be represented in a tabular format, with records in rows and attributes in columns, a model may sometimes be referred to as a "table".

### Attributes
Broadly there are two classes of attributes:
- **data** attributes (string, integer, float, boolean, matrix, date_time, shifted_date_time, file, file_collection)
- **link** attributes (parent, link, child, collection, table). Links must be two-way (i.e., a patient has a `collection :sample` attribute, and a sample has a `parent :patient` attribute).

The "parent" attribute is required for each model except the project model, producing a model hierarchy.

### Data Model Hierarchy
Although models may have non-hierarchical relationships (e.g. belonging to multiple collections) or no relationships to other models, the library enforces the requirement that models must have parents, and thus form a tree with the project at the root. The main purpose of requiring a hierarchy is to allow a conceptually simpler representation of the dataset, with the project as the root entity collecting all other pieces of data.

## How to Data Model
Setting up a new set of data models describing a project can be a daunting task. Fortunately, we can make good use of pre-existing work and conventions to make this task easier. Here we will describe one approach you may take to arriving at a set of models you can use to begin linking data.

The work of modeling data in the library is done via the Map page of the Timur application. This page allows us to visualize the model hierarchy, and allows us to inspect the attributes on each model, as well as the details of each attribute. In addition the map page contains a number of modeling tools (if you are an admin on your project), allowing you to add, remove or reparent models, and to add, remove or edit attributes in single or copied in bulk from another model. We will employ these modeling tools to build our project map.

### Use a Template Project

An invaluable resource in updating your map is to look at an existing map for another project. The data library may contain one or more "template" projects (e.g. in the UCSF Data Library the "coprojects_template" project) suitable for copying from.

### Naming Models

When adding models to your project, although you have the latitude to name models as you see fit, it is best to adhere to convention and use model names taken from the template project.

### Creating a Backbone

At first your new project will contain only a single model, the "project" model. Initially we wish to add the principle entities of the project that have produced the data we intend to ingest and collate. Although experiments may in principle have considerable variety in their structure, in practice a common backbone does well to describe the majority of projects:

```
project > cohort > subject > timepoint > biospecimen > compartment
```

Projects may not require all of the models in this hierarchy, e.g., if there is a single cohort, or only one timepoint of collection, those models may be excluded from the backbone. Other projects may require models not present here, e.g. some projects might include a study "arm" above a cohort, or more detailed models describing sample aliquoting.

After determining which models in the backbone our project requires, we may add the models to our project using the "Add Model" button, first adding a model below the `project` model, then selecting the new model and adding a model below that, etc.

### Adding data models

Now that the main structure of the project is determined, we may add more models to hold data. Roughly, data models can be broken down into "clinical" and "assay" categories.

We may add data models as leaves to the backbone by using the "Add Model" button (e.g. to add an `rna_seq` model below a `biospecimen` model). Once the empty model has been added, we may fill it with attributes using the "Copy Attributes" button. Select the template project and model you wish to copy from, then select the attributes you want to copy.

#### Clinical data models

Clinical data usually attaches to the patient or timepoint model, and usually originates in a patient's electronic medical record (EMR). This kind of data often has issues relating to patient privacy; one should be cautious about including unconstrained `string` attributes or `date_time` fields, as these can contain compromising information. The use of the `shifted_date_time` attribute allows the library to safely anonymize entered dates while still maintaining a relative patient-centric timeline for dates.

Many clinical data models are relatively simple and consist of name/value attributes; names or values may often come from standard medical ontologies defining medications, treatments or diseases.

#### Assay models

Assays are usually leaf models at the very bottom of the data hierarchy, attaching to the biospecimen or compartment models. Assay models might involve large, high-throughput datasets generated by complex protocols and machines. In addition to containing the raw output from the machine (e.g. a fastq file from a sequencer), the format of these models is typically determined to match the output of a corresponding processing workflow which operates on the raw data to produce a more suitable analytic intermediate.

#### Pool models

Often individual assays may be grouped into batches for processing, e.g. a plate of RNA sequencing, a microarray of tissue cross-sections, or a pool of single cells. These pool entities may combine information from multiple subjects, and so do not attach into the "backbone" models of the project; instead they are parented directly to the `project` model and collect the assay model, while the corresponding assay contains a link to the pool model but remains parented to the backbone. E.g., for an `sc_seq_pool` of `sc_seq` assays, we might have: `sc_seq_pool { parent :project, collection :sc_seq }, sc_seq { parent :subject, link :sc_seq_pool }`

To create a pool or batch model, use "Add Model" to add the new model below the project model. Then use "Add Attribute" to add a collection attribute pointing to the releant assay, with a reciprocal 'link' attribute in the assay model. Finally, use "Copy Attributes" to copy the attributes for the pool model from the corresponding template project model.

### Adding additional attributes

Project models, even when copied from a template, may be added to. Each project has its idiosyncracies requiring novel data fields not present in the template, such as alias identifiers from external collaborators, project-specific sample or assay QC, etc. We may add attributes to a model at any time using the "Add Attribute" button.

### Creating a new data model

Science is endlessly inventive, and you will inevitably encounter a new data type unrepresented by a template model. Creating a new assay model can be challenging and involves understanding the format of the raw assay data, how it is processed, and what pieces of processed data are relevant for analysis. Subsequently you can define the list of attributes capturing the relevant data. 
