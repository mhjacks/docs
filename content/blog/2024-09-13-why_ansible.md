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
platform and the language. This post is an explanation as to why.

## First: Our use of Ansible is not exclusive

We sometimes describe the Validated Patterns framework as "opinionated." In general, opinionated frameworks will place
certain restrictions on how they are used. In some cases, this will prevent users from using their own preferred
solutions in certain aspects of the framework. To be clear, several aspects of the Validated Patterns framework are
implemented in ansible; specifically:

* Secrets loading
* Unsealing the vault
* Gathering CAs from Edge Clusters

## Second: Ansible has some key advantages

### Declarative (enough)

### Clear definitions of "success" and "failure"

### Easy-to-use Retry mechanism

### Good Built-In YAML Tooling

### Very easy to "wrap" CLI tools if needed

