# REFLECTIONS

## 1. Multi-account

If `costctl` had to run across 100 AWS accounts instead of one, I would not keep
the current single-account direct boto3 model. I would switch to a cross-account
role pattern, where each target account exposes a dedicated IAM role and the CLI
assumes that role before running `list`, `cost`, `terminate`, or `tag`. I would
also add support for iterating through multiple AWS profiles or an account list
file, then write the output in a structured format such as CSV or JSON so the
results can be aggregated by account, region, and service.

## 2. `clean --apply` blast radius

If `clean --tag Environment=dev --apply` were run in a shared account by mistake,
the damage could be large because the command is destructive and tag-based. To
reduce that risk, I would want stronger guardrails before production use: a
mandatory confirmation summary with exact resource counts, an allowlist of safe
tag keys such as `purpose=practice`, and support for account or region scoping.
In a more mature version, I would also require dry-run output review first and
log every deletion target before executing the action.

## 3. W7 carry-over

The commands I would keep for W7 are `list`, `cost`, and `tag`, because they are
the most useful for ongoing visibility and governance across environments. `list`
helps quickly audit missing tags, `tag` helps fix attribution gaps, and `cost`
connects infrastructure to actual spending. I would keep `terminate` and `clean`
only with stronger safety controls, because they are useful for remediation but
too risky to run casually in a shared or production-style environment.
