---
layout: default
---

## Introduction to Mount Etna

Mount Etna is a set of applications that together provide tools for storing,
organizing, viewing and analyzing research data sets.

### How to organize data

Research science involves producing complex, highly structured datasets. The meticulous organization of this data is crucial to the scientific enterprise; in particular, the production of an error-free dataset can determine the success of an experiment, easily disrupted by the compromise of *data integrity*. The problem of how to produce this organization is therefore a critical research function.

In addition to the basic organization needs of the experiment, science has an important social component - knowledge is produced through a collective consensus, which requires the experimental data and method to be available to the community of researchers for validation and follow-up research.

Producing this organization and facilitating this sharing is the fundamental goal of the data library. In aid of this we wish to produce a set of tools allowing easy management of all aspects of the data life cycle.

But what is encapsulated in this nebulous word "data"? In the modern biomedical research setting (the main focus of the data library project), experimental data usually consists of measurements made on living tissue taken from study subjects, which may be human patients, cell lines, or model organisms. The measurements may be simple, as in the case of a clinical lab summarized by a single metric, or complex, as in the case of a sequencing experiment collecting data about millions of individual molecules.

In all cases, our goal is the same: to organize data so that experimental data is correctly associated with its point of origin in the experimental protocol. Our basic approach to this organization in the data library is to describe the dataset as a collection of experimental entities to which particular items of data attach. For example, a study may contain a set of subjects, each of whom produce a set of associated samples, upon which a set of experimental assays may be performed. Each of these entities (the subject, the sample, the assay) may have particular items of data associated with it, e.g., the subject may have associated demographic data like age, sex, etc., while the assay may have associated quality control values. When describing an entity, we wish to associate with it all of the data describing attributes of that entity, as well as noting its relationships to other data entities.

The fundamental unit of this organization in the data library is the **project**, which describes a single experiment or research study conducted by a team of researchers.

### The basic functions of the data library
- **Modeling a project**

  Organizing the dataset involves discovering the entities present in the experiment, the relationships between these entities, and the data attributes associated with each entity. This task is called *modeling*, and requires us to produce a *model* of each entity describing its *attributes* and *links* between entities.

- **Creating project identifiers**

  Correct association of data requires the use of identifiers; data that cannot be identified is useless, or perhaps even dangerous, to the experimental enterprise. Correctly associating data into a complex data hierarchy may require more than one piece of information (e.g., a sample has a name, and the patient that sample is associated with has a name; in order to correctly associate sample data with a patient we must know both the sample name and the patient name). To reduce the number of data items required for correct association and thereby prevent data integrity errors, the data library favors the use of *hierarchical identifiers* that allow the data to be correctly associated using only a single identifier string.

- **Control access to projects**

  Research science remains a competitive enterprise, and the initial phase of research is usually done by a small team working in private to assemble the dataset. Later, the same dataset will become a public resource available to the larger research community. The library provides role-based access to research projects across these various phases of the research life cycle.

- **Ingesting data**

  Data arrives in the library from a variety of external sources, usually deposited in the form of files. Ingestion is the primary step, producing a raw mass of files in the library that can subsequently be organized.

- **Linking data into models**

  Data ingested into the library can then be scanned, and individual data items read from ingested files can be associated into well-defined records in our data hierarchy of models.

- **Querying data**

  With the data associated into models, we may perform queries on well-formed data, making use of our relational data models to form precise and relevant subsets of the data for experimental inquiries.

- **Calculations and visualizations**

  Data queried out of the library is merely the grist for subsequent analysis; the library provides tools for performing calculations, creating visualizations, and sharing results with project members.

- **Exporting data**
  In addition to query-based subsetting of the data, the library provides mechanisms for bulk download of an entire project dataset.
