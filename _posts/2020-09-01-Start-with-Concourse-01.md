---
layout: post
comments: true
title : Start with Concourse v.1
categories: [ CICD, Automation, Pipelines, Devops ]
author: John Stilia
---

<span class="img-container">
    <img src="/assets/Start-with-Concouse-01/Start-with-Concouse-01.png" >
</span>

---

You might be working on a DevOps environment/position or myu might simply like automation.
Usually there are fwe known solutions out there that are in the market for long ( see Jenkins, Gitlab, Cronjobs :) etc )

Reading this article you wil find that concourse is one ( if not the only one ) solution for CICD that its secure, Devops and Dev friendly, requires minimum effort to configure ( at least for basic use ) and after all it is extremely simple to write as the configs are based on the beloved YAML syntax.

---

Let's now have a look to see ho we can deploy concourse and play around with some of the basic functionality.

In a nutshell, we will:
* <a href="#Instalation">Install Concourse with docker on our environment</a>
* <a href="#FlyTool">Look into the `fly` cli tool.</a>
* <a href="#WebFrontend">Look on the web frontend</a>
* <a href="#Buildapipeline">Build a very basic pipeline</a>

---

##  <a name="Instalation"></a>  <img src="https://simpleicons.org/icons/concourse.svg" width="3%"> Install Concourse with docker

* Clone the repo [concourse/concourse-docker](https://github.com/concourse/concourse-docker)
* Prepare the concourse environment with some settings
  * Create the necessary credentials to authorize concourse components with each other
    * Run `./keys/generate`
  * Edit the `docker-compose.yml` and replace the port `8080:8080` with `xxxx:8080`, `xxxx` being the port of your preference.
* Start the concourse containers with `docker-compose up`
  * This will start:
    * A Database ( postgress in this case )
    * A Web Frontend ( accessible on 127.0.0.1:xxxx, `xxxx` being the port configured on previous step )
    * A Worker, responsible to run all the tasks from the pipelines

**Default credentials are `test:test`**

## <a name="FlyTool"></a><img src="https://simpleicons.org/icons/concourse.svg" width="3%"> Fly cli tool
Depends the OS you will need to use your package manager to install the fly tool.<br>
An easy way to get the fly tool is from here:

`curl -LO https://github.com/concourse/concourse/releases/download/v3.3.2/fly_linux_amd64`<br>
`sudo mv ./fly_linux_amd64 /usr/local/bin/fly`<br>
`sudo chmod +x /usr/local/bin/fly`


The `fly` cli tool is the tool used to control and manipulate concourse on it's entirety.<br>
On the one of the next articles about concourse we will look more in depth on how to use this tool and a nice cheat-sheet to have around.

* Logging in to Concourse with fly:
  * `fly --target test login --team-name main --concourse-url localhost:8080`
    * `--target` is used to name the current configuration
    * `--team-name` is used to login to the default **main** team
    * `--concourse-url` is used to connect ot the url your concourse is running

## <a name="WebFrontend"></a><img src="https://simpleicons.org/icons/concourse.svg" width="3%">  Web Frontend

<details>
    <summary>Web frontend screenshot </summary>
<img src="/assets/Start-with-Concouse-01/Start-with-Concouse-01-screen1.png" width="40%" >
</details>
Navigate to [http://localhost:8080](http://localhost:8080) in order to see the the web frontend.<br>
As mentioned before, use the username and password test.<br>

#### <a name="WebFrontend"></a><img src="https://simpleicons.org/icons/concourse.svg" width="1%"> What can we do on the front end?

With Concourse frontend we can primarily visualize the pipelines. <br>
We can see the status, state, progress, dependencies between stages.
A great addition to this frond end is the ability to see pipelines run by different teams ( based on the way someone logged-in )
<details>
    <summary>Pipeline screenshot </summary>
<img src="/assets/Start-with-Concouse-01/Start-with-Concouse-01-screen2.png" width="400px" height="200px">
<img src="/assets/Start-with-Concouse-01/Start-with-Concouse-01-screen3.png" width="400px" height="200px">
<img src="/assets/Start-with-Concouse-01/Start-with-Concouse-01-screen4.png" width="400px" height="200px">
<img src="/assets/Start-with-Concouse-01/Start-with-Concouse-01-screen5.png" width="400px" height="200px">
</details>


## <a name="Buildapipeline"></a><img src="https://simpleicons.org/icons/concourse.svg" width="3%">  Build a Pipeline

In order to build a pipeline we will need 3 steps

* Write the YAML
* Push the YAML on concourse
* See the pipeline from frontend


#### <img src="https://simpleicons.org/icons/concourse.svg" width="1%"> Write the YAML

Open your favorite editor and create a file `pipeline.yml`
Then paste the following text
<details>
    <summary>YAML Code </summary>
    <pre>
---
jobs:
  - name: job
    public: true
    plan:
      - task: simple-task
        config:
          platform: linux
          image_resource:
            type: registry-image
            source: { repository: busybox }
          run:
            path: echo
            args: ["Hello, world!"]
</pre>
</details>

#### <a name="WebFrontend"></a><img src="https://simpleicons.org/icons/concourse.svg" width="1%"> Push the YAML to the concourse server

Here we ae going to use the **fly** cli tool. Open A terminal and paste the following<br>
  `fly -t test set-pipeline --pipeline my-pipeline --config pipeline.yml`

At this stage **fly** will give you a diff of the code it will send on the concourse server for you to check.<br>
After accepting the diff, **fly** will push the code to the frontend and a worker will pick it up and execute it.<br>
In this case the pipeline will start in a **pause** state. so we have the opportunity to manually start it.

Let's got to the [frontend](http://localhost:8080) and review the pipeline.

<details>
    <summary>Pipeline screenshot</summary>
<img src="/assets/Start-with-Concouse-01/Start-with-Concouse-01-screen6.png" width="400px" height="400px">
<img src="/assets/Start-with-Concouse-01/Start-with-Concouse-01-screen7.png" width="400px" height="400px">
<img src="/assets/Start-with-Concouse-01/Start-with-Concouse-01-screen8.png" width="400px" height="400px">
<img src="/assets/Start-with-Concouse-01/Start-with-Concouse-01-screen9.png" width="400px" height="400px">
</details>
