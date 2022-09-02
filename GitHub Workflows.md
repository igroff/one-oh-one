
### Workflow Execution

- Workflow Jobs are run in parallel[^paralell]
- Workflow Job sequencing and dependencies[^depends] are created using the `needs` property. If a job fails all other jobs that 'need' it will be skipped.
- It is possible to make a job with dependencies _always_ run regardless of the success of their dependencies using an `if`[^always]
```yml
    jobs:
      job1:
      job2:
        needs: job1
      job3:
        if: ${{ always() }}
        needs: [job1, job2]
```
* From a job's perspective, each one can run on a different host. i.e. The `hostname` command run as a job step reports a different host for each job.
* Job steps run on the same host.

#### [Concurrency](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#concurrency)

* Concurrent execution can be managed either at the Workflow or Job level. Jobs or Workflows using the same 'Concurrency Group' identifier will not run at the same time but will remain queued until any existing Job or Workflow with a matching 'Concurrency Group' completes.
  <details><summary>Example</summary>
  ```yml
    concurrency: "blue-jeans"
  ```
  </details>
* Newly queued Workflows or Jobs can cause currently running Jobs or Workflows using the same 'Concurrency Group' to stop running if the 'cancel-in-progress' property is set to true
  <details><summary>Example</summary>
    ```yml
      concurrency: 
        group: "blue-jeans"
        cancel-in-progress: true
    ```
    </details>
[^paralell]: [Parallel Job Execution](https://docs.github.com/en/actions/using-workflows/about-workflows#creating-dependent-jobs)
[^depends]: [Creating Dependent Jobs](https://docs.github.com/en/actions/using-jobs/using-jobs-in-a-workflow#defining-prerequisite-jobs)
