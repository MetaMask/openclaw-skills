# Contributing to MetaMask OpenClaw Skills

Thank you for your interest in contributing to the MetaMask OpenClaw Skills repository! This document provides guidelines for contributing new skills and improving existing ones.

## Code of Conduct

This project follows the [MetaMask Code of Conduct](https://github.com/MetaMask/.github/blob/main/CODE_OF_CONDUCT.md). By participating, you agree to uphold this code.

## What Makes a Good Skill?

A well-crafted skill should:

- **Solve a real problem** — Address a genuine need for MetaMask users or Ethereum developers
- **Be self-contained** — Include all necessary context for an AI agent to execute the skill
- **Follow best practices** — Implement security considerations and proper error handling
- **Be well-documented** — Include clear instructions, examples, and expected outcomes

## How to Contribute

### Adding a New Skill

1. **Fork and clone** the repository:
   ```bash
   git clone https://github.com/YOUR_USERNAME/openclaw-skills.git
   cd openclaw-skills
   ```

2. **Create a branch** for your skill:
   ```bash
   git checkout -b add-skill/your-skill-name
   ```

3. **Create the skill directory structure**:
   ```bash
   mkdir -p your-provider/your-skill-name
   ```

4. **Add your `SKILL.md`** — This is the only required file. See the [skill template](.github/SKILL_TEMPLATE.md) for the expected format.

5. **Add optional supporting files**:
   - `references/` — Additional documentation, API references, examples
   - `scripts/` — Helper scripts (bash, Python, etc.)

6. **Test your skill** — Ensure the skill works as expected with an AI agent before submitting.

7. **Submit a Pull Request** with:
   - A clear title describing the skill
   - A description of what the skill does
   - Any relevant context or use cases

### Improving Existing Skills

- Fix typos, improve clarity, or add examples
- Update outdated information
- Add missing edge cases or error handling
- Improve security considerations

### Reporting Issues

If you find a bug or have a suggestion:

1. Check if an issue already exists
2. If not, open a new issue with:
   - A clear, descriptive title
   - Steps to reproduce (if applicable)
   - Expected vs actual behavior
   - Any relevant context

## Skill Structure

Each skill should follow this structure:

```
provider-name/
└── skill-name/
    ├── SKILL.md           # Required: Main skill definition
    ├── references/        # Optional: Supporting docs
    │   ├── api.md
    │   └── examples.md
    └── scripts/           # Optional: Helper scripts
        └── helper.sh
```

### SKILL.md Format

Your `SKILL.md` should include:

1. **Title and description** — What the skill does
2. **Prerequisites** — Required tools, APIs, or setup
3. **Instructions** — Step-by-step guidance for the AI agent
4. **Examples** — Concrete usage examples
5. **Troubleshooting** — Common issues and solutions

## Review Process

1. A maintainer will review your PR
2. They may request changes or clarifications
3. Once approved, your skill will be merged
4. Skills may be tested by the community before full approval

## Security Considerations

- **Never include private keys, seeds, or secrets** in skill files
- **Validate all user inputs** in any scripts
- **Use secure defaults** for any configurations
- **Document security implications** of the skill's actions

If you discover a security issue in an existing skill, please report it privately following MetaMask's [security policy](https://github.com/MetaMask/metamask-extension/security/policy).

## Questions?

Feel free to open an issue for any questions about contributing. We're here to help!
