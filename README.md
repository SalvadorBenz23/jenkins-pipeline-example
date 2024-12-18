# jenkins-pipeline-example
deploying a jenkins pipeline

## Build Stage

### What’s Happening?
	•	Agent: Runs this stage in a python:3.8-alpine3.16 Docker container.
	•	Steps:
	•	Compiles Python files to bytecode.
	•	Stashes compiled files for later use in the pipeline.

### Why?
	•	To validate that the Python code compiles correctly before testing or deployment.
