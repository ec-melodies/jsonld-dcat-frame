# JSON-LD Frames for DCAT catalogs and datasets

The two JSON-LD frames in this repository are meant for easily working with arbitrary DCAT catalog or dataset resources published as JSON-LD (or converted to it).

The typical workflow is to apply one of the two frames to a given JSON-LD resource, followed by compaction with the context of the resulting framed document. Those are standard operations supported by most JSON-LD processors.

## Features

- short aliases for all common fields
- proper hierarchical structure
- URLs are never automatically turned into relative ones
- "distributions" is always an array (empty if no distributions exist)
- supports dataset relationships via "isPartOf" and "parts" which are always just URI references (never embedded)
