# Evaluation Cases

`evals.json` is a manual regression set, not an automated test runner. Run each prompt against a clean sample project state and compare the response with that case's expected behavior.

When changing the workflow, update the affected case. Evaluate whether the agent selected the correct path, used the right project artifacts, respected user-confirmation boundaries, and reported tests honestly; do not require exact wording.
