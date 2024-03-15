---
layout: default
---
# Naming

## Introduction

Data must be correctly identified if it is useful. Names may serve two purposes: they must uniquely identify a data entity, and they may provide human-legible information about the entity.

The library encourages the use of human-legible, hierarchical data identifiers. Human legibility reduces the chance of data integrity issues, while hierarchical identifiers allow reconstruction of the parent, grandparent, etc. identifiers of a particular data item, and therefore allow us to associate data to the correct position in the data hierarchy using only the identifier string, avoiding the need for additional metadata to associate data.

Names are managed in the data library via the application Gnomon, which allows us to define a grammar describing the identifiers for a project. The grammar may be used to construct a new identifier, to validate existing identifiers, and (if the grammar is hierarchical) decompose the identifier to reconstruct parent identifiers.

## Use a Template

As usual, a grammar may be defined in a template project, and may already contain rules and tokens that are useful to your project. A good starting point is to copy the template grammar into your project and begin from there. In particular you may find many of the tokens relating to specific assays, biospecimens, etc., are already present in the template grammar.

## Editing the Grammar

The grammar may be modified through a UI that allows you to click to add or remove rules and tokens; or it may be edited as a block of JSON by clicking the `<>` symbol.

JSON editing mode can be an easy way to copy an entire grammar from one project to another. Simply goto the Edit Rules page for the first project, click `<>` to see the grammar JSON, then select all in the JSON editor and copy the text. Goto the second project, click `<>` to edit as JSON, and paste the copied JSON into the editor. Enter a revision comment and hit 'save' to save your changes.

The grammar has a version history, so any changes are logged and old versions of the grammar may be restored or copied as needed.

## Create a Grammar

To create a grammar for your project, click "Edit Rules" on your Gnomon project page. Ultimately, we wish to create rules describing the identifiers for each identified model in our project.

## Defining Rules

A "rule" is a sequence of tokens and other rules, optionally terminated by a counter.

Here is an example rule for a patient identifier:

`PROJ SEP COH .n`

The rule consists of three tokens, PROJ, SEP, and COH, and the special symbol .n denoting a counter (which may only appear at the end of a rule). A token is a set of one or more strings that may be substituted into the rule. In this example, we might define the tokens as: `PROJ={EXAMPLE} SEP={-} COH={A,B}`. A counter can be any integer value.

Having defined a patient rule, we may now write patient identifiers, e.g.: `EXAMPLE-A1, EXAMPLE-B22`, and determine whether an identifier matches the given rule. We may also glean information from the identifier, i.e., given the identifier `EXAMPLE-B22` we know that the value of the COH token (i.e., the patient's cohort) is B.

We may also define rules hierarchically, if the child rule contains all of the tokens for a parent rule. Usually, we will refer to the parent rule in the rule definition. E.g., the rule for a sample might be written `.patient SEP BSP .n` with a new token `BSP={PL,SR}`. Substituting in the patient rule, this rule would evaluate to `PROJ SEP COH .n SEP BSP .n`, matching, e.g., `EXAMPLE-B2-PL1, EXAMPLE-A3-SR3`

This gives us a general format for a hierarchical identifier that looks like this:

`.parent_rule SEP TOK .n`

When defining rules it is best to proceed from the top of the data hierarchy and proceed down, adding tokens as required to define the next rule.

## Defining Tokens

The choice of what tokens to put into the identifier may vary by project, and is usually determined according to the experimental protocol. Keep in mind that there will be many data attributes associated with the identifier, and it is not necessary to encode all of this information into the identifier; the tokens should include those pieces of information most relevant to processing the sample. Ultimately the main requirement of the identifier (that it be unique) comes from the counter.

To define a token we need to define a symbol - this is a short sequence of letters that will be used in the rule string and does not appear in the final identifier. We also need a human-legible label describing this symbol (e.g. BSP is the symbol for the "biospecimen" token in the above example). Finally, we need a set of token values and their descriptions (e.g., in the BSP example, `PL=Plasma SR=Serum`). The description will allow us to interpret the identifier string in a human-legible way.

Sometimes a token can only take on a single value (e.g. `PROJ={EXAMPLE}` above); we still must enter these the same way and refer to them in the rule string using their symbol (e.g., `PROJ` in this case).

## Synonyms

An odd concept in the Gnomon grammar is a "synonym", which allows us to match two different tokens to each other. This allows us to use two different tokens to encode the same information in two different rules, and still allow the rules to decompose hierarchically.

The most common usage of this property is to set a cohort rule. Cohorts are often top-level models, and therefore may be given pretty, human-legible names that are not suitable to put in an identifier. E.g., in the above example we may wish to call our cohorts "Case" and "Control", although in our COH token we denote them simply 'A' and B'. We may achieve this using a second token `COHORT={Case,Control}` used in the cohort rule (simply `COHORT`), and a synonym between `COH` and `COHORT`, where the *description* of each value in both tokens must line up (e.g. `Case=Case Control=Control` and `A=Case B=Control`). Gnomon will then be able to determine that patient `EXAMPLE-B2` belongs to cohort `Control`

## Adding Names

Adding a project grammar to Gnomon allows us to validate identifiers before linking data. However, in addition to validating according to the *pattern* of an identifier, we may also use Gnomon to register the creation of named entities. In particular, using Gnomon to track created names can aid in the maintenance of counters, which must be correctly incremented every time a new name is issued. If names are created and registered in Gnomon, then Gnomon can determine what is the next correct number in sequence for a given counter.

Names can be created in Gnomon either through the "Create an Identifier" interface (mostly useful for testing rules), or through the more powerful "Create Many Identifiers" interface.
