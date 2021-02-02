# azdo-pipelines-reporting

## Overview

This repository contains a report that consolidates pipeline runs across multiple projects in the Azure DevOps Organization. The Power BI Template  queries Azure DevOps using REST APIs and fetches organization-wide data using multiple project-level queries.

### Data Tables

All Azure DevOps data tables use the Azure Analytics OData service as the data source. This is done by building a query URL in PowerBI Advanced Data Editor.

#### Projects

The Projects Table is retrieved at the organization level.

#### IPro

List of project names retrieved from the Projects Table. The list data type is necessary to allow queries to iterate through the projects. Additional filtering can be done to IPro to easily filter down to a specific subset of projects.

#### Pipelines

Pipeline data is retrieved at the project level. In order to fetch pipeline information for all the projects in an organization, we use a function to iterate an OData query through a list of Projects(IPro). This information is aggregated into the Pipelines table.

#### PipelineRuns

Similar to Pipelines, PipelineRun data is retrieved at a project level. We use the same method as Pipelines to create the PipelineRuns Table.

#### LastRefresh_UTC

This table is used to store the last data import time. It has a single entry that records the datetime in UTC time zone. Whenever a data refresh is performned, this field will automatically update.

### Calculated Columns (CC) & Measures (M)

#### Pipelines

- AverageQueueDurationinMinutes (CC): For each pipeline, this formula averages the QueueDurationSeconds of all pipeline runs and converts the time to minutes.

- AverageRunDurationinMinutes (CC): For each pipeline, this formula averages the RunDurationSeconds of all pipeline runs and converts the time to minutes.

- SucceededPipelineRuns (CC): For each pipeline, this formula conts the number of runs where the RunOutcome was "Succeed".

- FailedPipelineRuns (CC): For each pipeine, this formula counts the number of runs where the RunOutcome was "Failed".

- TotalPipelineRuns (CC): For each pipeline, this formulat counts the number of pipeline runs that exist. (Note: this includes other run outcomes such as Canceled and PartiallySucceeded)

- SuccessRate (CC): For each pipeline, this formula divides the SucceededPipelineRuns from the TotalPipelineRuns.

- FailureRate (CC): For each pipeline, this formula divides the FailurePipelineRuns from the TotalPipelineRuns.

#### PipelineRuns

- PipelineMatch (CC): If a pipelines gets deleted from a project, the pipeline run may still be recorded. This formula returns True if the pipeline run references an existing entry in the Pipelines Table and False otherwise.

#### LastRefresh_UTC

- Last Refresh (UTC) (M): Formats the datetime value in LastRefresh_UTC as a string.

*Authors: Narmatha Balasundaram, Richard Guo*