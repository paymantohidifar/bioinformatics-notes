# NextFlow Basic Training

## Part 1: Run Basic operations

The training uses a basic "Hello World" example to demonstrate essential Nextflow operations. Direct terminal commands like `echo 'Hello World!'` and `echo 'Hello World!' > output.txt` are used as a warm-up to show simple text output and writing to a file.

A Nextflow workflow is launched using the command `nextflow run 1-hello.nf --greeting 'Hello World!'`. The console output confirms successful execution, showing a line like `[a3/7be2fa] sayHello | 1 of 1 ✔`, indicating the `sayHello` process ran successfully.

Workflows are often configured to publish final outputs to a designated directory, such as `results/`, which contains the final output files (e.g., `output.txt`).

Nextflow creates a unique **task directory** for every process execution under the `work/` directory.
* The task directory path is visible in the console output (e.g., `[a3/7be2fa]`).
* This directory contains the original output file, as well as crucial log and helper files:
    * **.command.sh**: The main command executed by the process.
    * **.command.out** / **.command.err**: Standard output and error messages.
    * **.exitcode**: The exit code of the command.

### Examine a Workflow Starter Script

A Nextflow script is composed of one or more `process` blocks and a `workflow` block.
* **Process:** A `process` block (e.g., `sayHello`) describes a single step in the pipeline. It defines `input` variables (e.g., using `val`), declares expected `output` files (e.g., using `path`), and contains the `script` to execute.
* **Workflow:** The `workflow` block defines the dataflow logic, connecting various processes. It calls processes (e.g., `sayHello(params.greeting)`), passing inputs.
* **Command-Line Parameters:** The `params` system allows passing values from the command line using double-dash arguments (e.g., `--greeting` corresponds to `params.greeting` in the script).

### Manage Workflow Executions

* **Resuming Runs:** Adding the `-resume` flag to the `nextflow run` command skips processes that have already completed successfully with the same code, settings, and inputs.
    * This is useful for rapid iteration during development and recovering from production failures.
    * The console output indicates a skipped process with `cached:`.
* **Inspecting Logs:** The `nextflow log` command displays a history of all workflow executions launched from the current directory, including the session ID and full command.
* **Cleaning Work Directories:** The `nextflow clean` subcommand is used to delete older work subdirectories.
    * Use the `-n` (dry run) flag first to check what will be deleted, and then the `-f` (force) flag to execute the deletion.
    * Deleting work directories breaks Nextflow's ability to resume those runs, so published outputs should be saved elsewhere.

---