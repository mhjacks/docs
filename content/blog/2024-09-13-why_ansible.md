---
 date: 2024-09-13
 title: Why Ansible?
 summary: Why should we use Ansible, as opposed to other frameworks or scripting languages?
 author: Martin Jackson
 blog_tags:
 - patterns
 - ansible
 - gitops
---

<https://www.reddit.com/r/ProgrammerHumor/comments/17uddqh/yes/>

# Why Ansible?

Some observers have noticed that the Validated Patterns framework has a certain affinity for Ansible, both the
platform and the language. This post will primarily deal with the benefits we see in Ansible the language.

We sometimes describe the Validated Patterns framework as "opinionated." In general, opinionated frameworks will place
certain restrictions on how they are used. In some cases, this will prevent users from using their own preferred
solutions in certain aspects of the framework. To be clear, several aspects of the Validated Patterns framework are
implemented in Ansible; specifically:

* Secrets loading
* Unsealing the vault
* Gathering CAs from Edge Clusters
* Setup tasks that depend on runtime config

This post is an explanation as to why we have used Ansible for these and other things within the framework.

## First: Our use of Ansible is not exclusive

Some opinionated frameworks prevent the use of things things are opposed to the framework's opinion. That has never
been the intention with Validated Patterns and its use of Ansible. There are a number of tasks that fit the
criteria discussed above, but use shell scripts instead of Ansible.

One key feature of container-based platforms is that it is comparatively easy to import a new technology into a
solution or architecture simply by containerizing it and including it. Likewise, it is very straightforward to run
batch processes or setups in Kubernetes by including a script as a ConfigMap and running one of the pre-existing
container images with it. There are several examples of this in the Validated Patterns framework, and this article
is not an attempt to prevent or discourage that kind of scripting.  For many tasks shell is entirely
appropriate. But as tasks grow in complexity, or need to be more widely distributed and re-used, Ansible has a
number of things going for it that made it our preferred  mechanism for doing some of this more complex work.

Of course none of this precludes other people doing other things, even within the Validated Patterns framework. We
have some examples of bespoke Python scripting in some places, for example, and we have worked to make and keep
available extension features like config management plugins in ArgoCD/OpenShift GitOps to specifically enable this
kind of freedom. Furthermore, there are no magic criteria to determine when a task is "too complex", and sometimes
things grow more complex because of forces beyond the author's control. So we do not want to preclude choice of
tooling in these cases.

## Second: Ansible has some key advantages

That said, we believe that once tasks get to a certain level of complexity, or need for re-use, that Ansible is
a great choice for them. One of the first major pieces of the framework that was written in Ansible was the first
generation of the Secrets loader. The reason for this is that it had to handle complex data structures and a surprising
diversity of error conditions.

### Well-known / Easy to Learn

Many of the people who find Validated Patterns useful (or would), already are very familiar with Ansible. Further,
there are many resources available to learn Ansible for those who do not already know it.

### Declarative (enough)

Critics of Ansible are always quick to point out that Ansible is not inherently declarative. This is true - but for
years now, Ansible guides have encouraged people to use the declarative forms of Ansible modules. It is always
possible to use the `command` or `shell` modules to run a command, but in many cases there are better modules suited
for managing particular resources, that will know whether they need to to do work or not, and intelligently report
success and failure.

### Clear definitions of "success" and "failure"

Every task in Ansible requires the task to return whether it succeeded or failed, and further allows
this definition to be overridden by the caller via the `failed_when` task directive.

### Easy-to-use Retry mechanism

One constant requirement in an eventual-consistency environment is that resources may not be ready to be worked on. In
such situations, it is useful to be able to easily retry a given task until it is successful.

### Easy to Compose workflows

Facts set in Ansible plays can be carried over from play to play. It is straightforward to join multiple playbooks
together into a single workflow. This makes it extremely easy to build very complex and dynamic workflows with Ansible.

### Works well with Kubernetes and OpenShift

Ansible provides well-established content for interacting with Kubernetes-native resources, which is indispensable
when dealing with a framework that primarily lives inside OpenShift and Kubernetes.

### Ansible works well with many of the other resources in Cloud Environments

Ansible also provides high-quality, well-tested and widely used content for interacting with all of the major
hyperscalers, and has some very nice capabilities for dealing with APIs in addition to its traditional strengths
doing configuration management at the operating system layer. In the Validated Patterns framework, we have found
several uses for Ansible that make it easier for us to be as "declarative as possible", even when that means
implementing our own control and reconciliation loops with processes implemented in Ansible.

### Re-usability, testability and Code sharing

Ansible has a well-understood mechanism for packaging content -- collections. And these collections can either be
hosted on Ansible Galaxy (or Red Hat Automation Hub) for public consumption, or else can be installed directly from git
or other hosting mechanisms.

Ansible also has a very good (and constantly improving) suite of development and testing tools, to help ensure quality
in Ansible content. It has first-rate support in VSCode, with a number of extensions maintained and published by Red
Hat, as well as tooling available for other IDEs like Vim and neovim for those so inclined.

### Good Built-In YAML Tooling

### Very easy to "wrap" CLI tools if needed

