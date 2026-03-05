# Claude Code Setup

Claude Code is used as an AI assistant in this project, running via AWS Bedrock.

## 1. Install Claude Code

```sh
curl -fsSL https://claude.ai/install.sh | bash
```

Make sure `claude` is in your PATH:

```sh
export PATH="~/.local/bin:${PATH}"
```

Add the above to your `.bashrc` if it's not already there.

## 2. Configure AWS SSO Profile

```sh
aws configure sso --profile lifescience-services-development
```

You will be prompted for configuration values:

| Field | Value |
|-------|-------|
| `sso_start_url` | `https://d-9a672d56e1.awsapps.com/start` |
| `sso_account_id` | *(your account number)* |
| `sso_role_name` | `cas-developer` |
| `sso_region` | `us-east-2` |
| `region` | `us-east-2` |

> **Note:** During the SSO login flow you may be asked to select a role. Choose **sf-development** (not sf-deployment).

Reference: [Claude Code Setup — AWS Bedrock Inference Profiles](https://acs-cas.atlassian.net/wiki/spaces/CSVS/pages/1297449014/Claude+Code+Setup+-+AWS+Bedrock+Inference+Profiles)

## 3. Shell Setup

Add the following to your `.bashrc` (or source it from a separate file):

```sh
export CLAUDE_BEDROCK_AWS_PROFILE="lifescience-services-development"

claude-bedrock() {
  local profile="${CLAUDE_BEDROCK_AWS_PROFILE}"

  # Check if SSO session is active, login if needed
  if ! aws sts get-caller-identity --profile "${profile}" &>/dev/null; then
    echo "[INFO] AWS SSO session expired, logging in..."
    aws sso login --profile "${profile}"
  fi

  # Export credentials to environment
  eval "$(aws configure export-credentials --profile "${profile}" --format "env")"

  # Set Bedrock-specific environment
  export CLAUDE_CODE_USE_BEDROCK=1
  export ANTHROPIC_BEDROCK_MODEL_ID="${ANTHROPIC_BEDROCK_MODEL_ID:-global.anthropic.claude-opus-4-6-v1}"
  export AWS_REGION="${AWS_REGION:-us-east-2}"
  export AWS_DEFAULT_REGION="${AWS_REGION}"

  # Update Claude Code if needed
  claude update

  command claude "$@"
}

alias cb='claude-bedrock'
alias cbd='claude-bedrock --dangerously-skip-permissions'
alias asl='aws sso login --profile "${CLAUDE_BEDROCK_AWS_PROFILE}"'
```

## 4. Configure Claude Model

Add to `~/.claude/settings.json` at the top level:

```json
{
  "model": "global.anthropic.claude-opus-4-6-v1"
}
```

## 5. Using Claude

### Useful commands

- `/feature` — structured feature implementation
- `/diary` — write a session diary entry
- `/diary-read` — read recent diary entries to restore context

### Starting a session

Load project-specific skills at the beginning of a session:

```
Load these skills:
- sf-requirements
- marklogic-development
- marklogic-ml-gradle
- marklogic-unit-test
- marklogic-query-profiler
- cas-xpath-explorer - localhost / 3693 for the triples
- marklogic-data-discovery
```
