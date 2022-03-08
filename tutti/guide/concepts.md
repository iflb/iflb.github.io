# Concepts

In this page, all concepts that are important for understanding Tutti.works are explained.
Below includes descriptions and provided operations of each concept.
Note that information described in this page is not inclusive, and some of the concepts have other pages that describe further details.

## Tutti Resources

### Project

**Project** is a working space that contains all configurations for a microtask, including a set of web page UIs, conditional rules, parameters, etc.

Inherently, a project is just a single directory located at `tutti/projects/<project_name>` which contains `scheme.py` (scheme file) and `templates/` (template files).
In other words, making a directory (with a proper structure) in `tutti/projects` means to create a project, and deleting the directory means to delete the project.
All data associated to a certain project is independent from that to another project.

<span class="alias">projects directory = <code>tutti/projects/</code></span>
<table class="concepts-table">
    <tr>
        <th>Operation</th>
        <th>in Tutti.works Console</th>
        <th>via SSH</th>
        <th>via Client API</th>
    </tr>
    <tr>
        <td>list</td>
        <td>
            <ul>
                <li>Click select menu in the app bar</li>
                <li>Check in Dashboard page</li>
            </ul>
        </td>
        <td>
            See directories in <span class="alias">projects directory</span>
        </td>
        <td>
            <code>listProjects</code>
        </td>
    </tr>
    <tr>
        <td>create</td>
        <td>
            Click "+" icon in the app bar
        </td>
        <td>
            Create directory in <span class="alias">projects directory</span>
        </td>
        <td>
            <code>createProject</code>
        </td>
    </tr>
    <tr>
        <td>delete</td>
        <td>
            -
        </td>
        <td>
            Delete directory in <span class="alias">projects directory</span>
        </td>
        <td>
            <code>deleteProject</code>
        </td>
    </tr>
</table>

### Template

**Template** is a UI component as a single web page that appears in microtasks of a project.
In a project directory, each template is defined as a sub-directory containing `Main.vue`, located at `tutti/projects/<project_name>/templates/<template_name>`.

<span class="alias">templates directory = <code>tutti/projects/&lt;project\_name&gt;/templates/</code></span>
<table class="concepts-table">
    <tr>
        <th>Operation</th>
        <th>in Tutti.works Console</th>
        <th>via SSH</th>
        <th>via Client API</th>
    </tr>
    <tr>
        <td>list</td>
        <td>
            <ul>
                <li>Click select menu in the tool bar of Template page</li>
                <li>In dashboard page, hover on a number for "# Templates"</li>
            </ul>
        </td>
        <td>
            See directories in <span class="alias">templates directory</span>
        </td>
        <td>
            <code>listTemplates</code>
        </td>
    </tr>
    <tr>
        <td>create</td>
        <td>
            Click "Create Template" button in the tool bar of Template Page
        </td>
        <td>
            Create directory in <span class="alias">templates directory</span>
        </td>
        <td>
            <code>createTemplate</code>
        </td>
    </tr>
    <tr>
        <td>delete</td>
        <td>
            -
        </td>
        <td>
            Delete directory in <span class="alias">templates directory</span>
        </td>
        <td>
            <code>deleteTemplate</code>
        </td>
    </tr>
</table>

#### Preset

Template **presets** are provided to requesters for easy creation of templates.
New templates can be created from presets in Tutti.works console or via Tutti.works client API.

<table class="concepts-table">
    <tr>
        <th>Operation</th>
        <th>in Tutti.works Console</th>
        <th>via SSH</th>
        <th>via Client API</th>
    </tr>
    <tr>
        <td>list</td>
        <td>Click select menu for presets in create template dialog</td>
        <td>See directories in <code>tutti/.defaultproject/templates/.presets/</code><br>or <code>tutti/projects/&lt;project_name&gt;/templates/.presets/</code><br>(*Not recommended)</td>
        <td><code>listTemplatePresets</code></td>
    </tr>
</table>

### Scheme

Project **scheme** refers to project configuration, which is consisted of **config** and **flow**.
Scheme can be updated by editing `scheme.py`.

For further programming details, see [references for Project Scheme](guide/ref_scheme).

### Nanotask

**Nanotasks** are pre-defined assignment instances linked to a template, each of which has dynamic data fields (props) as well as parameters such as the number of collected assignments.
To learn nanotask data structure that can be loaded into templates, see [Understanding nanotask JSON](tutorial/nanotask?id=understanding-nanotask-json).

In microtasks, nanotasks are automatically selected and assigned to workers based on either of two types of nanotask assignment ordering rules.
By default, nanotasks are registered in a "global" priority queue in which nanotasks are always sorted based on rules specified in the project scheme as well as number of collected responses and priority value set for each nanotask.
Nanotasks are then assigned in FIFO.
As another way, next loaded nanotasks can also be manually specified with nanotask IDs in scheme's flow. See [references for Project Scheme](guide/ref_scheme) for details.

<table class="concepts-table">
    <tr>
        <th>Operation</th>
        <th>in Tutti.works Console</th>
        <th>via Client API</th>
    </tr>
    <tr>
        <td>list</td>
        <td>Click "Nanotasks" button for a template node of displayed flow in Scheme page</td>
        <td><code>listNanotasks</code></td>
    </tr>
    <tr>
        <td>create</td>
        <td>Click three-dots button for a template node of displayed flow in Scheme page,<br>and upload JSON file or create individually</td>
        <td><code>createNanotasks</code></td>
    </tr>
    <tr>
        <td>delete</td>
        <td>Select nanotasks and click delete button from nanotask list view for a template</td>
        <td><code>deleteNanotasks</code></td>
    </tr>
</table>

#### Nanotask Props

**Nanotask props** object, which is a member of a nanotask, is a key-value data structure that contains arbitrary information that should be dynamically loaded into the nanotask it belongs to.
For a simple example, props of a nanotask can have "audio\_url" as key, and a URL string for an audio file as its value.
In the template source code, you can then receive the props object in JavaScript code block and pass it as a source URL of an `<audio>` tag.

For further details, see [references for Template](guide/ref_template).


#### Nanotask Group

**Nanotask group** is a batch of grouped nanotasks to be assigned during a session of a microtask.
Currently, this is a feature which only works for Tutti.market.

<table class="concepts-table">
    <tr>
        <th>Operation</th>
        <th>in Tutti.works Console</th>
        <th>via Client API</th>
    </tr>
    <tr>
        <td>list</td>
        <td>Click "Nanotasks" button for a template node of displayed flow in Scheme page</td>
        <td><code>listNanotaskGroups</code></td>
    </tr>
    <tr>
        <td>create</td>
        <td>Select nanotasks and click "Create nanotask group" button from nanotask list view for a template</td>
        <td><code>createNanotaskGroup</code></td>
    </tr>
    <tr>
        <td>delete</td>
        <td>Select nanotasks and click delete button from nanotask list view for a template</td>
        <td><code>deleteNanotaskGroup</code></td>
    </tr>
</table>

### Sessions

There are two types of workers' **sessions** managed in Tutti.works.
Sessions are basically a working history object recorded while a user (worker) works in a microtask, in different granularities for each session type.

#### Work Session

**Work session** is created every time when a worker starts a microtask, and manages common information (*ex.,* worker ID, project name) for the working history until the microtask finishes the flow.

<table class="concepts-table">
    <tr>
        <th>Operation</th>
        <th>in Tutti.works Console</th>
    </tr>
    <tr>
        <td>list IDs</td>
        <td>Show "Session ID" column in responses table for a template in Responses page</td>
    </tr>
</table>

#### Node Session

**Node session** is created every time when a worker enters any `FlowNode` (*i.e.,* `BatchNode` and `TemplateNode`) defined in scheme, manages history information (*ex.,* node name, prev/next node) relevant to the node.
Node sessions can be used for keeping track of and analyzing each worker's path during a whole microtask.

<table class="concepts-table">
    <tr>
        <th>Operation</th>
        <th>via Client API</th>
    </tr>
    <tr>
        <td>list</td>
        <td><code>listNodeSessionsForWorkSession</code></td>
    </tr>
</table>

### Response

**Response** is a record for worker's submission results on a template, with an arbitrary data structure including answers the worker gave.

<table class="concepts-table">
    <tr>
        <th>Operation</th>
        <th>in Tutti.works Console</th>
        <th>via Client API</th>
    </tr>
    <tr>
        <td>list</td>
        <td>Select a template name in Responses page<br>(list for Template)</td>
        <td><code>listResponsesFor***</code></td>
    </tr>
    <tr>
        <td>watch</td>
        <td>-</td>
        <td><code>watchResponsesFor***</code></td>
    </tr>
</table>

## Amazon MTurk Resources

For Amazon MTurk's native operations, data structures, etc., see [Amazon MTurk API Reference](https://docs.aws.amazon.com/AWSMechTurk/latest/AWSMturkAPI/).

### Tutti HIT Batch

**Tutti HIT Batch** is a data structure that extends/includes Amazon MTurk's and HIT Type (HIT parameter template) and HIT Group (batch of HITs of the same HIT Type).
Tutti HIT Batch also refers to a group of HITs but with the following additional features:

- An arbitrary name field
- A *hidden* [Qualification Type](https://docs.aws.amazon.com/AWSMechTurk/latest/AWSMturkAPI/ApiReference_QualificationTypeDataStructureArticle.html) automatically added to HITs created for the batch (this is not intended to filter out any workers; to be described below)

These two features are mainly for efficient HIT retrieval;
as of March 2022, Amazon MTurk does not allow requesters to get/list HIT data with any other queries than HIT IDs and associated Qualfication Types.
Considering overheads for these operations via API calls, Tutti.works tries to reduce the number of API calls by grouping HITs by associating them with a batch name and the same Qualification Type with `DoesNotExist` comparator, thereby not affecting workers' eligibility to work on them at all.
This Qualification Type is not even visible to Tutti.works users by default, which you do not need to take care of.

<span class="alias"><b>HITs page</b> = Navigate to Worker Platforms-&gt;Amazon MTurk-&gt;HITs in Tutti.works Console</span>
<table class="concepts-table">
    <tr>
        <th>Operation</th>
        <th>in Tutti.works Console</th>
        <th>via Client API</th>
    </tr>
    <tr>
        <td>list</td>
        <td>Check in <span class="alias">HITs page</span></td>
        <td><code>listTuttiHITBatches</code><br><code>listTuttiHITBatchesWithHITs</code></td>
    </tr>
    <tr>
        <td>create</td>
        <td>In <span class="alias">HITs page</span>, click "Create HITs" button and then choose "Create new HIT Type"</td>
        <td><code>CreateTuttiHITBatch</code></td>
    </tr>
    <tr>
        <td>add HITs</td>
        <td>In <span class="alias">HITs page</span>, click "Create HITs" button and then choose "Choose from existing HIT Type"</td>
        <td><code>addHITsToTuttiHITBatch</code></td>
    </tr>
    <tr>
        <td>delete</td>
        <td>In <span class="alias">HITs page</span>, expand a row of a Tutti HIT Batch and click "Delete HITs"</td>
        <td><code>deleteTuttiHITBatch</code></td>
    </tr>
</table>
