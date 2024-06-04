# Advanced Prisma with MongoDB

This repository contains the starter project for the **Advanced Prisma with MongoDB** workshop by [Sabin Adams](https://twitter.com/sabinthedev).

## Setup

### 1. Clone this repository

You can clone this repository with the following command:

```
git clone https://github.com/prisma/prisma-mongodb-advanced-workshop
```

> Alternatively, you can also download the project via the GitHub UI. Click the green **Code**-button in the top-right corner and then click on **Download ZIP**.

### 2. Install dependencies

Navigate into the project directory and install the npm dependencies with the following command:

```
cd prisma-mongodb-workshop
npm install
```

### 3. Start local MongoDB instance via Docker

The `docker-compose.yml` file in this repo lets you spin up an instance of a MongoDB database via [Docker](https://www.docker.com/). Run the following command to start your MongoDB instance:

```
docker-compose up -d
```

# Advanced MongoDB Workshop

*(Prisma + MongoDB Launch Week)*

# Welcome

👋 Welcome to the **Advanced Prisma + MongoDB** workshop.

# Resources

**This Github document**

[`https://pris.ly/prisma-mongo-adv`](https://pris.ly/prisma-mongo-adv)

**GitHub starter project**

`https://github.com/prisma/prisma-mongodb-advanced-workshop`

# Prerequisites

In order to successfully complete the tasks in the workshop, you should have:

- [Node.js](https://nodejs.org/en/) installed on your machine.
- A [GitHub](https://github.com) account.
- [Docker](https://www.docker.com) installed on your machine *or* a hosted MongoDB instance (e.g. on Atlas)
- A code editor installed *(preferably [VS Code](https://code.visualstudio.com/))*.
- A basic understanding of schemaless database concepts.

# What you'll do

In this workshop, you’ll dive into a full-stack application that is *half-built*. The entire application is visually complete and functional, however it is using all static data. Your job will be to hook the site up to a MongoDB database and replace all of the static data with live data from that database.

You'll start by setting up Prisma with a MongoDB database, modeling out some of your required data, and learning about how a *“schema”* fits into the world of *schemaless* databases (*lesson 1).*

Then, you'll use Prisma Client to build all of the queries relating to a user’s data. You will write some simple queries, some more complex ones, and take a look at how to handle *embedded documents* (*lesson 2*).

Next, you’ll update your Prisma schema to account for the next set of data: kudos. You’ll learn how to set up relations, how to implement referential actions, and how Prisma can solve a few of the *gotchas* that come with a schemaless database (*lesson 3*).

Finally, you’ll wrap up the application’s loose ends by building all of the queries that read and write Kudos data (*lesson 4*).

# Lessons

[Set up Prisma](setup-prisma.md)

[Build queries for user data](build-queries.md)

[Update your schema](update-schema.md)

[Add queries for kudos data](add-kudos.md)

# What does a lesson look like?

A *lesson* is structured in two parts:

1. **Host walkthrough:** At the beginning of each *lesson*, your host will walk you through the different *tasks* you'll encounter in this lesson. Please *be attentive* during that time and follow the host's explanations to be sure that you can accomplish the tasks yourself when you're working on them later. **Do not code along or work on the tasks yourself yet!** Instead, you can think of questions or raise anything that you don't understand (e.g. in the **Q & A** section of Zoom).
2. **Do it yourself:** Once the host is done showing and explaining the different tasks, you get dedicated time to work on the tasks yourself!

# Want to host this workshop yourself?

Hosting workshops is incredibly fun! 😄 It's also a great way to deepen your understanding of the topics you're teaching and giving back to the community by sharing your knowledge.

The materials for this workshop are free to use and can be shared with anyone you know! If you want to host this workshop yourself and want some advice on how to get started, feel free to [reach out](mailto:support@prisma.io).