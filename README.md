# jenkins-pipeline-example
deploying a jenkins pipeline

Summary of Stages:

	1.	Build Stage:
	•	Creates source files (prog.py and calc.py).
	•	Compiles them into bytecode (__pycache__).
	•	Stashes the compiled results for later use.

	2.	Test Stage:
	•	Unstashes the compiled files.
	•	Creates a dummy test file test_calc.py.
	•	Runs tests using pytest, generating a JUnit-compatible XML report.
	•	Archives the test results.

	3.	Deliver Stage:
	•	Unstashes compiled results.
	•	Runs PyInstaller in a Docker container to generate an executable.
	•	Archives the executable as an artifact.
	•	Cleans up temporary build and dist directories.
