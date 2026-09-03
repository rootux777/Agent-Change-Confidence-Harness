# Limitations

- V0.1 was validated through one local .NET pilot in one project.
- No integration, deployment, Azure, database, or provider-redaction proof is included.
- Passing tests do not prove complete correctness or absence of regressions outside tested behavior.
- Human review remains required; the harness does not authorize changes or make the PM decision.
- Existing dependency warnings remain separate risks and are not cleared by a green focused test.
- Shell helpers assume a POSIX-like environment with Bash, standard file utilities, and `shasum`; JSON Schema validation additionally requires the `jsonschema` CLI.
- Provider rendering and exception redaction are outside the harness unless a provider-specific test is explicitly run.
- The package does not provide CI integration, access control, signing, or tamper-proof evidence storage.
