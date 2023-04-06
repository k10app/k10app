# Build app

To build and release the Helm chart you need to modify `helm/k10app/Chart.yaml` on the `main` branch.

This modification should at least consists of bumping the chart version in `helm/k10app/Chart.yaml` and adding a corresponding entry into `helm/k10app/CHANGELOG.md`, otherwise the release will fail and need manual clean-up.

It's expected that chart changes are added to the main branch via PRs which will be tested.
