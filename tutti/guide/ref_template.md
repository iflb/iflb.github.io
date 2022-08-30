# Template

## File names and locations

| Template Type | Path |
| ------------- | ---- |
| Preview | `tutti/projects/<project_name>/templates/Preview.vue` |
| Instruction | `tutti/projects/<project_name>/templates/Instruction.vue` |
| Main Template | <code>tutti/projects/\<project_name\>/templates/\<template_name\>/Main.\[vue\|html\]</code> |

?> Preview and instruction files are created with `create_project` command, and the main template file is created with `create_template` command.

## Preview & instruction

The preview and instruction files, with a `.vue` extension, are basically [Vue.js' single file components (SFCs)](https://vuejs.org/v2/guide/single-file-components.html); you can thus not only write HTML collocate CSS and JavaScript in the files.

For those who are not familiar to Vue.js' SFC, here's an example for the minimum code:

```markup
<!-- Works in both Preview.vue and Instruction.vue -->

<template>
    <div>
        This is my test preview/instruction.
    </div>
</template>
```

Note that a whole HTML needs to be wrapped by `<template>`, as well as only one parent tag (*i.e.,* `<div>` in the above example) is allowed in the block.

### Preview.vue

At present, `Preview.vue` is a necessary page component for particularly when the project is deployed as Amazon Mechanical Turk HITs.
A HIT "preview" page is what MTurk workers see when they actually be assigned and start the HIT, thereby better understanding what the HIT would request them to do.
Typical preview pages contain the similar user interface shown during the task or detailed task instruction.
Be sure to design this page component thoughtfully, since it is requesters' responsibility to instruct workers to get better results and for fair work.

<img src="./_media/preview.gif" width="500" />

### Instruction.vue

`Instruction.vue` is another page component that helps workers get better sense of what needs to be done during the task (this is not limited to Amazon Mechanical Turk).
There are two patterns where the instruction page components is shown to workers: i) automatically rendered in a floating dialog for workers who have never visited the project, and ii) rendered in a floating dialog when the worker clicked "See instruction" button in the project interface (the button is only visible when `ProjectScheme.instruction` is set to True).

<img src="./_media/instruction.gif" width="500" />


## Main Template (Vue.js -- Main.vue)

When a template file is named `Main.vue`, the template code needs to be written in a Vue's SFC manner.
Programming references are described below:

### Vue Mixin

All nanotask templates are required to import `nanoMixin` in the `<script>` block:

```javascript
import nanoMixIn from "@/mixins/nano";
export default {
    mixins: [nanoMixIn],
    ...
```

This `nanoMixIn` module contains a set of Tutti-defined [Vue's options](https://vuejs.org/v2/api/#Options-Data) to allow requesters to send and receive Tutti-relevant data with the backend server.

#### data

- `nano.ans` (Object)
  - A writable object which stores worker's answers sent to Tutti's backend server when `submit()` is called.
    Its format should be like:
    ```json
    {
        "mykey1": "some string",
        "mykey2": 12345,
        ...
    }
    ```
- `nano.props` (Object)
  - A readable object which contains "**nano props**", a set of dynamic contents in the template for a loaded nanotask, which is uploaded with `UploadNanotasks` Event.

- ~~`defaultProps`~~ (Object) <span style="color:red">deleted since 0.3.24</span>
- `defaultNanoProps` (Object) <span style="color:green">valid from 0.3.24</span>
  - A set of values initially set to `nano.props` (= nanotask INPUT).
  It is recommended to set this `Object`-typed value, to explicitly indicate expected members to be input to nanotasks.
  Default member values are usually used when 1) the template is previewed in the Templates page, and 2) the template is rendered in a task but no nanotask is registered to it.

  To initialize it, just create `data` in the template file like:
    ```javascript
    <script>
    import nanoMixIn from "@/mixins/nano";
    export default {
        mixins: [nanoMixIn],
        data: () => ({
            defaultNanoProps: {
                mystring: "my default value",
                myurl: "https://some.default.url"
            }
        })
    };
    </script>
    ```
  so the default values are set to `nano.props.mystring` and `nano.props.myurl`.

- `defaultNanoAnswers` (Object) <span style="color:green">valid from 0.3.24</span>
  - A set of values initially set to `nano.ans` (= nanotask OUTPUT).
  It is **necessary** to set this `Object`-typed value, to initialize keys and values used for user inputs in the nanotask.
  
  To initialize it, just create `data` in the template file like:
    ```javascript
    <script>
    import nanoMixIn from "@/mixins/nano";
    export default {
        mixins: [nanoMixIn],
        data: () => ({
            defaultNanoAnswers: {
                myCheckboxAnswer: [],
                myTextInputAnswer: ""
            }
        })
    };
    </script>
    ```

- `workerId` (String)
  - An internal worker ID string.

#### computed property

- ~~`canSubmit`~~ <span style="color:red">Deprecated since 0.3.34</span>
  - A helper property that returns a boolean value whether all required answer fields are filled (*c.f.,* `v-nano`).

- `fileUploadStatus` <span style="color:green">valid from 0.3.24</span>
  - An `Array` that contains either `'waiting' | 'uploading' | 'complete'` in each element, of which index corresponds to that of files uploaded to the queue.

- `allFileUploadComplete` <span style="color:green">valid from 0.3.24</span>
  - A `Boolean` value that returns `true` when all queued files are uploaded, otherwise `false`.
  This is equivalent to `this.fileUploadStatus.every(s => s === 'complete')`.

#### method

<<<<<<< HEAD
- `submit(answers)`
  - A method to finish the current template and step forward to the next template node.
  - When `answers` is not passed (*i.e.,* `undefined`), answers bound to `nano.ans` and data associated with `v-model`+`v-nano` directives is sent to server.
=======
- `submit()`
  - A method to finish the current template and step forward to the next template node. Required in a template.

- `uploadQueuedFile(index)` <span style="color:green">valid from 0.3.24</span>
  - Starts uploading a file queued at the given index.
  - **Arguments**
    - `index` (Number)
- `async addBufferToFileUploadQueue(buffer, path, name, uploadInstantly=false)` <span style="color:green">valid from 0.3.24</span>
  - Add data to be written to the file storage into the queue.
  - **Must be called with `await`**. 
  - **Arguments**
    - `buffer` (TypedArray) - Data to write as a file.
    - `path` (String) - A directory path to write a file to.
    - `name` (String) - A written file name.
    - `uploadInstantly` (Boolean) - Whether to call `uploadQueuedFile()` immediately after the data is queued.
  - **Returns**
    - `index` (Number) - An index of the queued entry.

- `removeFromFileUploadQueue(index)` <span style="color:green">valid from 0.3.24</span>
  - Removes the data from the queue.
  - **Arguments**
    - `index` (Number)

- `addFileUploadStatusChangeHandler(handler)` <span style="color:green">valid from 0.3.24</span>
  - Adds a handler function which is called when a upload status change.
  - **Arguments**
    - `handler` (function)
  - **Returns**
    - `handlerId` (Number)

- `removeFileUploadStatusChangeHandler(handlerId)` <span style="color:green">valid from 0.3.24</span>
  - Removes a handler function called on upload status change.
  - **Arguments**
    - `handlerId` (Number)
>>>>>>> develop

#### directives

- ~~`v-nano`~~ <span style="color:red">Deprecated since 0.3.34</span>
  - Adding `v-nano` directive to input components makes its `v-model`-binded data also synchronized to workers' answer data which is to be sent to Tutti's server.
    If data is bound the `v-nano` directive, the string specified for `v-model` is registered as a key of `nano.ans` object.
  - `.required` modifier is allowed for use to make the input to be a required field (this effects the return value of `canSubmit`).


## Main Template (Pure HTML -- Main.html)

For those who are not familiar with coding in Vue.js and want to stick with pure HTML, Tutti.works also provides that option.
To use this feature, you need to:

1. Create `Main.html` in the template directory, and
2. Delete `Main.vue` (**important**; otherwise `Main.html` will not be loaded)

To make `Main.html` properly work as a Tutti.works template, use APIs described below:

### Imported module

A module object named `nano` is imported.
No additional module import is needed on users' end.

##### `nano.submit(answers)`

Finishes the current template, sends the answers to the server, and step forward to the next template node.
`answers` argument must be passed in `object` type.

##### `nano.setDefaultProps(props)`

Sets default values for `nano.props` when no valid value for nanotask props was found, such as when no assignable nanotask is found for the worker or an unknown child key value was specified.
This is also used in previewing templates in Tutti.works Console.
`props` argument must be passed in `object` type.

##### `nano.applyProps(handler)`

Make nanotask props loaded to the template available from JavaScript and executes the handler method passed as `handler` argument.
`handler` receives `props` argument, which contains an object of all props loaded for the nanotask.

### Basic Usage

An example of a whole `Main.html` file looks like as follows:

```html
<html>
<body>
  <!-- Define your template UI here -->
  <button onClick="nano.submit(answerObject)">
</body>
<script>
let answerObject = {};
window.addEventListener('nanoLoad', () => {
    nano.setDefaultProps({
      // define default values for your nanotask props
    })
    nano.applyProps((props) => {
      // write logics to apply loaded nanotask props to your template
    })
});
</script>
</html>
```

Note that any codes that use `nano` object must be accessed after a custom event called `nanoLoad` is emitted, otherwise `nano` will return `undefined`.
