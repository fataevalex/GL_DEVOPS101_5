# Kubernetes Prompt Engineering Portfolio

This repository contains Kubernetes manifests generated using prompt engineering techniques with the help of `kubectl-ai`. Each manifest is based on a clearly defined prompt and use-case.

| NAME                  | PROMPT                                                                                                                   | DESCRIPTION                                                      | EXAMPLE |
|-----------------------|--------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------|---------|
| app.yaml              | Generate simple deployment for pod wich labels (app:demo, run:demo) and contaner name app and contaner port 8000         | Base deployment with default container setup                     | [link](./yaml/app.yaml) |
| app-livenessProbe     | Add a livenessProbe that checks '/healthz' every 10s and fails after 3 failed attempts                                   | Adds self-healing check to base deployment                       | [link](./yaml/app-livenessProbe.yaml) |
| app-readinessProbe    | Add a readinessProbe that checks `/ready` with initialDelay of 5s and timeout of 2s                                      | Adds availability detection for traffic routing                  | [link](./yaml/app-readinessProbe.yaml) |
| app-volumeMounts      | Add Mount a persistent volume to `/data` in the container                                                                | Demonstrates basic volume mount setup                            | [link](./yaml/app-volumeMounts.yaml) |
| app-cronjob           | Create a CronJob that runs a cleanup script daily at midnight                                                            | Automates periodic background job                                | [link](./yaml/app-cronjob.yaml) |
| app-job               | Create a one-time job that initializes the database                                                                      | One-time execution resource example                              | [link](./yaml/app-job.yaml) |
| app-multicontainer    | Generate simple deployment for pod and modify the deployment to have a sidecar container that logs the main app’s stdout | Showcases multi-container Pod design                             | [link](./yaml/app-multicontainer.yaml) |
| app-resources         | Add CPU/Memory resource limits and requests to the container                                                             | Enforces resource management best practices                      | [link](./yaml/app-resources.yaml) |
| app-secret-env        | Inject environment variables from a Kubernetes Secret named `app-secrets`                                                | Demonstrates secure configuration management                     | [link](./yaml/app-secret-env.yaml) |
