+++
title = "What Is a Data Platform"
date = "2025-09-08T12:00:00+02:00"
draft = false
slug = "what-is-a-data-platform"

#
# description is optional
#
# description = "An optional description for SEO. If not provided, an automatically created summary will be used."

tags = ["Data Engineering", "Data Platform", "Data Infrastructure", "Data stack"]
+++

Lately I have been thinking quite a bit about the question "What is a data
platform in its most basic form". When I set out to answer this question my
first thought was that it's not possible to answer this in a useful way. Reason
being that what a data platform is varies a lot depending on the company.
Basically, the data platform that a big tech company with 1000's of engineers use
and a team with 100 engineers use will be wildly different. So I shifted my focus
from the tech and scale to capabilities. I think this is a good way of actually
making clear what a data platform is, by saying what it should be able to do at
the absolute minimum. This post provides the foundational framework, over the next few posts, we'll dive deep into each layer with real implementation details and lessons learned. So the layers below are what I came up with based on
experience and my own opinions.

In order to make the information more digestible I will make this into a series
of posts. This first post will be the high level overview and in the following
posts in this series I will go into more detail about each layer.

### Key components
I have identified 4 layers or maybe pillars that makes a data platform,
regardless of size and bells and whistles:

- Data pipelines, this is pretty much ELT/ETL, i.e just moving data from sources
  outside of the platform into the platform
- Orchestration, this can sometimes be combined with pipelines
- Data storage
- BI/Data science. Essentially using the data to create value.

### Data pipelines and Ingestion - Getting data into your system
This is the process of moving data from various sources, i.e. logs, third party
apps/APIs, OLTP systems etc, into your analytics environment.

The three approaches for this is:
- Fully custom code for the entire process
- Low code ingestion tools such as Fivetran, Matillion etc.
- Code based frameworks such as Prefect, Dagster and Airflow. These tools give
a lot of control and help orchestrate complex multi step pipelines.

Now each of these options usually also nowadays makes use of a transformation
tool such as dbt or sqlmesh.

### Data storage - Building a central source truth
The analytics environment mentioned above is referencing essentially this
storage layer. This could be either a data warehouse, data lake or a lakehouse.
The main idea here is that we want a centralized data layer where teams can work
from the same definitions, metrics and logic. So instead of team members going
and pulling data themselves from different sources they can just come and join
in whatever it is that they need directly in the storage layer. Thus a good
storage layer unlocks:
- Standardized data models
- Governance and access control
- Data Quality and consistency
- Faster time to insight

### Data visualization - Turning data into decisions
Ingestion and storage are necessary but not enough. To drive impact and actually
solve problems people actually need to use the data.

Dashboards, KPIs, self-service tools, and even excel reports are all ways teams
consume and make decisions. When done right, reporting provides:
- Executives with clarity
- Enable teams to track initiatives
- Help analysts test hypotheses

The ultimate goal is to build reports that are trusted, actionable and aligned
with how your business operates

### Data science and Notebooks
While dashboards are built for monitoring and decision-making, data science tools support exploration, experimentation, and advanced analysis. This is where notebooks and model development environments come in.

Not every company needs machine learning from day one. But as your data maturity grows, you’ll likely want to go beyond just looking at the past, you’ll want to forecast, segment, classify, or personalize based on that data.

Notebooks are ideal for:
- Prototyping models quickly
- Exploring data visually with Python, R, or SQL
- Combining code, charts, and documentation in one place
- Sharing insights across teams or stakeholders
They’re especially useful for cross-functional work, where a data scientist wants to show not just the results, but how they got there.

Use cases for this layer:
- Customer segmentation
- Predictive modeling (churn, sales, etc)
- Experiment analysis (A/B tests, feature rollouts, etc)
- Time series forecasting
- NLP for internal documents or chat data

Think of notebooks as the R&D lab of your data stack. They don’t replace pipelines or BI, they complement them by enabling deeper questions and experimentation.

As your team matures, this layer becomes more important not just for data scientists, but for analysts and product teams who want to explore without committing to production changes right away.

## Summary
At its core, a data platform is about enabling teams to make better decisions with data. The four layers pipelines, storage, visualization, and experimentation provide the essential capabilities every organization needs, regardless of size or technical sophistication.

This framework gives you a lens for evaluating tools and planning your data infrastructure. In the next post, we'll dive into the first layer: building reliable data pipelines.


