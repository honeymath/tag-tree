# tag-tree
Evidence-driven nested label system for verifiable and auditable data annotation, designed to support AI reasoning and minimize hallucinations via graded evidence levels.

## Overview

This system is a hierarchical label annotation framework that manages data labeling through a structured folder-based architecture. Each label is a folder containing descriptions, scripts, verification mechanisms, and tools for automatic data interpretation. The system ensures traceability, extensibility, and semantic integrity.

## Label Structure

Folder Hierarchy

Each label is represented as a folder.

Each folder contains a readme.md describing the semantics and usage of the label.

Labels can be nested to form a hierarchy (i.e., sublabels), supporting structured classification.

The labeling path for a file can be represented as a directory-like expression, e.g., task/urgent/reply_needed.

Files can be annotated by multiple labels.

Label Folder Contents

Each label folder includes:

readme.md: Human-readable description.

Content of details of labels (Label Metadata Files) stored as `{inode}.yaml`, the advantage of storing inode is the uniqueness of it among all files suitable for the situation where the file could be moved, or folder name changed; however it does not suitable if user want to quickly replace a content of a file by deleting and renaming. The system is designed for AI useage.

To enable bidirection links, there is a registrar registering labels attached for each inode.


Subfolders:

_questions/: Contains Python scripts defining mandatory questions for labeling under this tag. For example, if a label represents a task, questions may include “What is the deadline?” The answers of the quetion is stored at the datafield of the file the corresponding label.

_dataformat/: Contains a YAML structure mapping fields to Python scripts. This defines how the labeled data should be formatted into tabular or structured output.

_addition/: Contains scripts to generate additional instructions or derived content, such as extracting referenced email content, computed fields, or supplementary classification.

## Scripts for this project

- enlabel.py filemane.ext -l Label1/Label2/Label3   // adding a label to a file, and this triggers some questions and one have to answer. give user the quit chance so when not anwser the question is able to give up the label.
- viewlabel.py filename.ext  // Able to get the full report of the labeled file as text.
- filterbylabel.py label1, label2 ,... // find those files by the attached label.
- settings.ini   // some settings


## Label Metadata Files

For each file filename.ext, an accompanying metadata file filename.ext.label stores annotation details:

script:
  - module: validator
    function: extract_second_line
    parameters: {}
label:
  - task/urgent/reply_needed
  - summary

task: reply_generation

timestamp: 2025-04-22T12:34:56

Field Description:

script: A list of pre-defined scripts with parameters. These perform verification or content extraction. Scripts must come from a pre-approved set but allow customization through parameters.

label: One or more hierarchical labels assigned to this file.

task: The higher-level process or job responsible for assigning this label.

timestamp: The automatic creation time of this label annotation.

Scripts may include logic such as extracting lines, parsing headers, or validating content. These are used as evidence for human or machine reasoning.

## Execution Model for Scripts

Each labeled file may have an associated sequence of scripts.

These scripts are intended for validation or data extraction.

Parameterized execution is supported to reduce redundant code.

Only approved scripts are allowed to avoid unregulated behavior, while maintaining pluggable extensibility.

## System Properties

Bidirectional traceability between data and labels.

Semantic consistency via required _questions and format scripts.

Automatic enrichment through GPT-assisted _addition logic.

Suitable for dynamic, high-accuracy annotation tasks in domains such as NLP, OCR, and email processing.

## Future Extensions

_review/: Storing GPT or human review outcomes.

_relations/: Expressing dependency or semantic links between labels.

Label versioning: Track label evolution over time.

This modular and extensible design lays a strong foundation for a robust annotation ecosystem.
