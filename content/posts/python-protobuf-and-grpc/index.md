---
date: "2022-05-22T00:00:00Z"
title: Python, Protobuf, and GRPC
draft: true
tags:
- software development
- python
- microservices
---

For a long time I wanted to get into the world of Protocol Buffers and GRPC.I have a strong interest in ways to scale software development that doesn't suck, and have always found it almost impossible to achieve. One of the key aspects of being able to do that, as was learned from industry experience, is to write decoupled software, and in many cases, also break it to different services that communicate with each other over the network.

Now in order for those services to communicate with one another efficiently it is necessary to have a clear contract between them (i.e API schema). That is where Protobuf comes into the picture. Utilizing it's IDL makes describing the schema and the services that provide it very concise and clear.

Chance have it that I'm starting a project of splitting up our monolith architecture into domain services.

---

Looking for resources about using GRPC with Python, I couldn't find many good comprehensive guides (the Python school one is pretty good[]), but had to pick up pieces from many different locations, so here is my take on a good setup for Python development.

## Structure

There a few to go about structuring the protobuf files. I like the method of having a single repo that contains all the protobuf files.

A nice trick I found to have it play nice with Python is to use a top level directory for the proto services that will be a 

```
├── bv_proto
│   └── service_a
│       └── v1
│           └── service_a.proto
└── python
    ├── bv_proto
    │   ├── _version.py
    │   ├── service_a
    │       └── v1
    │           ├── service_a_pb2.py
    │           ├── service_a_pb2.pyi
    │           └── service_a_pb2_grpc.py
    └── pyproject.toml
```

## Using buf

  - Generate Python package without the import path issue
  - Forces good conventions with the linter
  - Use backwards compatibility checks to validate API stability
  - Replaces `protoc` usage - one less tool to use
  - Plugins support adding stuff like Python type definitions (`.pyi`) 
- Python packaging using `hatch`:
  - Simple pyproject configuration
  - Supports version bumps
- Github actions