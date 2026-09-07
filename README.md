# warp

Copier template for **Vertex AI Pipelines** (KFP v2) classification projects backed by **BigQuery ML**.

Generated projects include:

- Training + prediction KFP pipelines (BQML classification)
- `payload.json` for project/region/tables/thresholds
- Makefile compile / trigger targets via Poetry + `google-cloud-aiplatform`
- **Poetry** for dependency management in generated projects
- **Terraform**: GCS, Artifact Registry, Pub/Sub, Cloud Run, Cloud Scheduler, IAM, Cloud Build triggers
- **Cloud Build**: PR checks, push → `terraform apply`, release → docker + KFP artifacts to GCS

## Prerequisites

- Python ≥ 3.11 (or the version you select when copying)
- [Poetry](https://python-poetry.org/) ≥ 1.8 (for generated projects)
- [Copier](https://copier.readthedocs.io/) ≥ 9
- Terraform ≥ 1.5
- A GCP project with Vertex AI, BigQuery, GCS, Pub/Sub, Cloud Run, Scheduler, and Cloud Build APIs
- Application Default Credentials or a service account with pipeline / Terraform rights

## Create a project

```bash
pip install copier
copier copy gh:ali-cabukel/warp ./my-pipeline
# or from a local clone:
# copier copy /path/to/warp ./my-pipeline
```

Then:

```bash
cd my-pipeline
make setup
# edit payload.json and terraform/terraform.tfvars
make compile
make tf-init && make tf-apply
make trigger-training
```

## Update an existing project

```bash
cd my-pipeline
copier update
```

## Template layout

```text
template/
├── payload.json.jinja
├── Makefile.jinja
├── Dockerfile.jinja
├── cloudbuild/          # pr / push / release
├── terraform/           # infra + Cloud Build triggers; crons in terraform.tfvars
└── src/<package>/
    ├── components/
    ├── pipelines/
    ├── trigger/
    └── service/         # Cloud Run Pub/Sub → PipelineJob
```
