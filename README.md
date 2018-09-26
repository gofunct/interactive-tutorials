# Katacoda Interactive Tutorials
## What layouts can I use?
<details><summary>show</summary>
<p>

Katacoda supports three allowing you to match the Katacoda layout to your content.

Supported layouts are:
* Terminal

* 2x Terminal Split

* Editor + Terminal

* Terminal + Editor for the embedded presentation

**Note:** Make sure to include data-katacoda-layout="editor-terminal-split" in the html for the embedded code.

* Terminal + Iframe (application inside the dashboard). Requires backend.port within index.json

Within the scenario index.json file, the UI can be defined under the environment node.

</p>
</details>

##  What environments can I use?
<details><summary>show</summary>
<p>

Supported environments are:

**Environment**:**Image ID**
Docker: docker
Docker Swarm: swarm
Kubernetes: kubernetes
Kubernetes Cluster: kubernetes-cluster
CoreOS: coreos
Ubuntu: ubuntu
Ansible: ansible
Node.js v6: node6
Go: go
.Net Core: dotnet
Java: java8
Bash: bash

Within the scenario index.json file, the imageid can be defined under the backend node.

</p>
</details>

## Understanding Katacoda's index.json

<details><summary>show</summary>
<p>

The index.json file is the place where the information about the scenario is defined. This page will outline the options.

### Titles and Descriptions
* pathwayTitle: Title of the collection of scenarios
* title: Title the scenario
* description: Description of the scenario, displayed on the intro screen
* difficulty: Provide users with a sense of the depth of content, displayed on the intro screen
* time: Provide users with an estimated time to complete, displayed on the intro screen
### Details Node
* steps: Details for the scenario steps
* intro: Details for the intro screen
* finish: Details for the finish screen
* assets: Files to upload to the environment. There are different places where the files can be uploaded, the client and any of the hosts. The client is the container defined in the index.json file. host01 is where Docker itself is running. When using a cluster of containers, there will be others hosts, for example host02.

* To copy all files to the /root folder in the host01 you can use:
```
"assets": { "host01": [ {"file": "*", "target": "/root"} ] }
```

* To copy the file example to the home of the client and to grant execution privileges:
```
"assets": { "client": [{"file": "example", "target": "/home/scrapbook/", "chmod": "+x"}] }
```

### Steps Node
* title: Title for the step.
* text: Filename containing the body for the step.
* answer: Filename containing the answer body for the step.
* verify: Bash script to run to check if the user can proceed.
* courseData: Bash script to run in the background.
* code: Bash script to run in the foreground.
### Intro/Finish Node
* text: Filename containing the body for the screen.
* credits: Display a link on the intro screen, useful for linking to blog post for giving credit.
* courseData: Bash script to run in the background.
code: Bash script to run in the foreground.
### Files
For use with the UI Editor layout, define filenames that would be opened by default.
### Environment
* hideintro: Boolean field that control if the intro step is shown to the user. By default it is hidden in the embedded mode.
Example:

```
"environment": { "uilayout": "terminal", "hideintro": false}
```

* hidefinish: Boolean field that control if the finish step is shown to the user. By default it is hidden in the embedded mode.
Example:
```
"environment": { "uilayout": "terminal", "hidefinish": true }
```

* uisettings: Especify the format of the files for syntax highlighting in the editor (useful when it doesn't recognize the extension you are using). The suported formats are: reactjs, makefile, dockerfile, dockercompose, csharp, javascript, golang, java and xml.
Example:
```
"environment": {"uisettings": "yaml"}
```

* icon: The icon for the scenario. The list of icons is at the  home page . The possible values are: fa-docker, fa-weave, fa-kubernetes, fa-openshift, fa-dcos, fa-tensorflow, fa-runc, fa-coreos, fa-elixir, fa-csharp, fa-fsharp, fa-rlang, fa-golang, fa-java, fa-node and fa-ruby.
Example:
```
"icon": "fa-node"
```

* showdashboard: Should Dashboard tabs be shown in UI.
dashboards
Easily provide links for accessing dashboard/UI ports running in the environment. When using the terminal-iframe layout, it also show the exposed port of the container, without the need of open a new window. You can specify the name, port and the host identifier.
To display a tab called App showing the port 8080 from the host host02:
```
"dashboards": [{ "name": "App", "port": 8080, "host": "host02" }
```

* uilayout: The layout ID provided by Katacoda.
* uimessage1: Message to display at the top of the interative terminal.
* Backendimageid: Environment image id provided by Katacoda. 

### Example

```
{
 "pathwayTitle": "Pathway Title",
  "title": "Scenario Title",
  "description": "Scenario Description",
  "difficulty": "beginner",
  "time": "5-10 minutes",
  "details": {
    "steps": [
      {
        "title": "Step Title",
        "text": "step1.md",
        "answer": "step1-answer.md",
        "verify": "step1-verify.sh",
        "courseData": "run-command-in-background.sh",
        "code": "run-command-in-terminal.sh"
      },
      {
        "title": "Step 2 - Step Title",
        "text": "step2.md"
      }
    ],
    "intro": {
      "text": "intro.md",
      "courseData": "courseBase.sh",
      "credits": "",
      "code": "changecd.sh"
    },
    "finish": {
      "text": "finish.md"
    },
    "assets": {
      "client": [
        {
          "file": "docker-compose.yml",
          "target": "~/"
        }
      ],
      "host01": [
        {
          "file": "config.yml",
          "target": "~/"
        }
      ]
    }
  },
  "files": [
    "app.js"
  ],
  "environment": {
    "showdashboard": true,
    "dashboards": [{"name": "Tab Name", "port": 80}, {"name": "Tab Name", "port": 8080}],
    "uilayout": "terminal",
    "uimessage1": "\u001b[32mYour Interactive Bash Terminal. A safe place to learn and execute commands.\u001b[m\r\n"
  },
  "backend": {
    "imageid": "docker"
  }
}
```
 
</p>
</details>

## Markdown Syntax

<details><summary>show</summary>
<p>

Katacoda has added extended markdown# Markdown Example# Terminal IntegrationAllow a code block to be executed `some-command`{{execute}}

For multiple terminals, execute the command on HOST1 `some-command`{{execute HOST1}}
For multiple terminals, execute the command on HOST2 `some-command`{{execute HOST2}}

For multiple terminals, execute the command on T1 `some-command`{{execute T1}}
For multiple terminals, execute the command on T2 `some-command`{{execute T2}}

Allow a code block to be copied `some-command`{{copy}}
          
### Editor Integration

```
<pre class="file" data-filename="app.js" data-target="replace">var http = require('http');
var requestListener = function (req, res) {
  res.writeHead(200);
  res.end('Hello, World!');
}

var server = http.createServer(requestListener);
server.listen(3000, function() { console.log("Listening on port 3000")});
</pre>
          
<pre class="file" data-target="clipboard">Test</pre>
          
<pre class="file" data-target="regex???">Test</pre>


```

</p>
</details>

## Displaying Web UI and Accessing Ports

<details><summary>show</summary>
<p>

Katacoda allows ports running include the environment to be made available to users. This is designed for displaying dashboards and UI elements running inside a container.
Within the markdown for a step, use a URL shown below that will be updated when the environment is connected.
Markdown Example

Render port 8500: https://[[HOST_SUBDOMAIN]]-8500-[[KATACODA_HOST]].environments.katacoda.com/

Render port 80: https://[[HOST_SUBDOMAIN]]-80-[[KATACODA_HOST]].environments.katacoda.com/

Display page allowing user to select port:
https://[[HOST_SUBDOMAIN]]-[[KATACODA_HOST]].environments.katacoda.com/
          
### Display Dashboard Tabs

```
"environment": {
  "showdashboard": true,
  "dashboards": [{"name": "Display 80", "port": 80}, {"name": "Display 8080", "port": 8080}],
}
```

</p>
</details>

##  Running commands to customise environment

<details><summary>show</summary>
<p>

* Katacoda environments are pre-configured with the base configuration, for example , Docker or Kubernetes. 
* Commands can be run in the background when the user connects that allow the environment to be customised. 
* Files from the directory can also be uploaded.
### Running Commands
* Bash scripts defined in courseData will run in the background when the user begins that step. 
* For intro, this means the commands will run when the environment becomes available. 
* For steps, this is when the user starts the step. This is perfect for configuring scenarios and environments to help guide the user.
* Bash scripts defined in code work the same as courseData but will run in the foreground.
### Uploading Files
* Files in a **assets** directory within the scenario can have files uploaded to the client or host of the environment.

### Index.json Example

```
"details": {
  "intro": {
    "courseData": "courseBase.sh",
  },
  "assets": {
    "client": [
      {
        "file": "docker-compose.yml",
        "target": "~/"
      }
    ],
    "host01": [
      {
        "file": "config.yml",
        "target": "~/"
      }
    ]
  }
}
```

</p>
</details>