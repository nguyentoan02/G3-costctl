# costctl - XBrain W6 side challenge

Small AWS resource management CLI for the XBrain W6 cost challenge.

This repo is based on the starter template from `TechX-Corp/xbrain-costctl-starter`.
The CLI scaffolding and helpers were provided; the command logic was implemented
in this repo.

## Current status

- Test status: `25/25 passing`
- Implemented commands:
  - `list`
  - `cost`
  - `terminate`
  - `tag`
  - `clean`
- Not implemented:
  - `idle`
  - `migrate-gp3`

## What the CLI does

Supported resource types:

- `ec2`
- `rds`
- `s3`
- `volume`

Supported commands:

- `list <type>`: list resources, filter by tag or missing tag
- `cost --tag k=v --days N`: show cost grouped by AWS service for a tag
- `terminate <type> --id <id>`: terminate/delete one resource with confirmation
- `tag <type> --id <id> --set k=v`: add or update tags on one resource
- `clean --tag k=v [--apply]`: bulk terminate resources by tag, dry-run by default

## Quick start

```bash
git clone https://github.com/nguyentoan02/G3-costctl.git g3-costctl
cd g3-costctl
python -m pip install -r requirements-dev.txt
python -m pytest -q
python costctl.py --help
```

## Test result

Local mocked test suite result:

```text
25 passed
```

Tests are local-only and use `moto`, so they do not require real AWS credentials
and do not call the real AWS account.

## Example commands

```bash
# list
python costctl.py list ec2
python costctl.py list ec2 --tag Environment=dev
python costctl.py list ec2 --missing-tag Application
python costctl.py list volume --tag purpose=practice

# cost
python costctl.py cost --tag Application=HealthBot --days 7

# terminate
python costctl.py terminate ec2 --id i-0123456789abcdef0
python costctl.py terminate ec2 --id i-0123456789abcdef0 --force

# tag
python costctl.py tag ec2 --id i-0123456789abcdef0 --set Owner=alice --set Application=HealthBot

# clean
python costctl.py clean --tag purpose=practice
python costctl.py clean --tag purpose=practice --apply
```

## Project structure

```text
costctl-starter/
|-- costctl.py
|-- commands/
|   |-- _common.py
|   |-- list_cmd.py
|   |-- cost_cmd.py
|   |-- terminate_cmd.py
|   |-- tag_cmd.py
|   |-- clean_cmd.py
|   |-- idle_cmd.py
|   `-- migrate_gp3_cmd.py
|-- tests/
|-- sample_output/
|-- requirements.txt
|-- requirements-dev.txt
`-- README.md
```

## AWS requirements for manual verification

- Python `3.11+`
- `boto3`
- AWS credentials configured for your account
- Recommended region: `us-east-1`

For real CLI runs against AWS:

- Read permissions for EC2, RDS, S3, Cost Explorer, CloudWatch
- Write permissions for EC2, RDS, S3 if you use `terminate`, `tag`, or `clean`

## Sample output

Replace the files in [sample_output](D:\xbrain-costctl-starter\sample_output) with
real outputs from your AWS account before submission.

Suggested examples:

- `list_ec2_example.txt`
- `list_ec2_missing_app_example.txt`
- `cost_example.txt`

## Reflections

Create a `REFLECTIONS.md` file and answer at least 2 prompts.

Suggested prompts:

1. If `costctl` had to run across 100 AWS accounts, what would you change?
2. When is `idle` more useful than Trusted Advisor, and when is Trusted Advisor better?
3. What guardrails would you want before running `clean --apply` in a shared account?
4. Which parts of the implementation did you modify after AI assistance, and why?
5. Which commands would you keep for W7 and which would you drop?

## Submission checklist

- [ ] Rename/fork repo to `g3-costctl`
- [ ] Replace all `3` placeholders with your real group number
- [x] Implement command logic
- [x] Pass local test suite: `25/25`
- [ ] Replace `sample_output/*` with real output from your account
- [ ] Create `REFLECTIONS.md` with at least 2 answers
- [ ] Add your real team member names below
- [ ] Push repo public
- [ ] Create git tag: `w6-sidechallenge-v1`
- [ ] Push tags: `git push --tags`
- [ ] Reply in thread using:
      `G3 - https://github.com/TechX-Corp/xbrain-costctl-starter - 25/25 tests passing - implemented: list, cost, terminate`

## Team

Replace before submission:

- `Nguyen Van Toan`


## Notes

- `cost` depends on Cost Explorer data being available and on cost allocation tags
  being activated in the Billing console.
- `tag` for S3 merges new tags with existing tags before writing back.
- `clean` is destructive only when `--apply` is provided.
- `terminate` asks for confirmation by default; use `--force` only when intended.

## License

MIT - see `LICENSE`.
