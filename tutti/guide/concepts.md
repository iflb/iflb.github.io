# Concepts

## Tutti Resources

### Project

**Project** is a working space that contains all configurations for a microtask, including a set of web page UIs, conditional rules, parameters, etc.

Inherently, a project is just a single directory located at `tutti/projects/<project_name>` which contains `scheme.py` (scheme file) and `templates/` (template files).
In other words, making a directory (with a proper structure) in `tutti/projects` means to create a project, and deleting the directory means to delete the project.
All data associated to a certain project is independent from that to another project.

### Template

**Template** is a UI component as a single web page that appears in microtasks of a project.
In a project directory, each template is defined as a sub-directory containing `Main.vue`, located at `tutti/projects/<project_name>/templates/<template_name>`.

#### Preset

Template **presets** are provided to requesters for easy creation of templates.
New templates can be created from presets in Tutti.works console or via Tutti.works client API.

### Scheme

Project **scheme** refers to project configuration, which is consisted of **config** and **flow**.
Scheme can be updated by editing `scheme.py`.
(For further programming details, see [references for Project Scheme](guide/ref_scheme).)

#### Config

**Config** is a set of parameters applied to a project, defined in `config_params()` method in `scheme.py`.

#### Flow

**Flow** defines template transition rules during worker's session in a microtask, written in `define_flow()` method in `scheme.py`.
In addition to defining displayed orders of templates, each template or group of templates ("batch node") can set condition statements such as "IF" and "WHILE" so that the flow can flexibly change based on states.

### Nanotask

**Nanotasks** are pre-defined assignment instances linked to a template, each of which has dynamic data fields (props) as well as parameters such as the number of collected assignments.
To learn nanotask data structure that can be loaded into templates, see [Understanding nanotask JSON](tutorial/nanotask?id=understanding-nanotask-json).

#### Nanotask Props

#### Nanotask Group

### Sessions

#### Work Session

#### Node Session

### Response

## Amazon MTurk Resources

For Amazon MTurk's native operations, data structures, etc., see [Amazon MTurk API Reference](https://docs.aws.amazon.com/AWSMechTurk/latest/AWSMturkAPI/).

### Tutti HIT Batch


