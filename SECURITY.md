# Security

AI VoiceForge is an active development project. No independent security audit has been completed.

## Reporting a vulnerability

Please do not publish active exploits, credentials, private audio, or another person's conversation data in a public issue.

Use GitHub private vulnerability reporting if it is enabled for this repository. Otherwise contact the maintainer privately through the [GitHub profile](https://github.com/Realavdulmouhd).

A useful report includes the affected component, expected and observed behavior, security impact, and reproduction steps that do not disclose real credentials.

## Credentials and personal data

Never commit API keys, JWT signing secrets, access tokens, password files, database exports, private voice recordings, or conversation exports.

Environment files with real credentials should remain local. Example environment templates must use clearly nonfunctional values.

If a secret is exposed through Git, revoke or rotate it. Removing the file in a later commit does not erase the secret from previous commits.

## Scope and authorization

Only assess systems and accounts that you own or are authorized to test. Do not use this project to access another user's conversations or impersonate a real person's voice without verified permission.

## Current limitations

The application is not ready for unrestricted public deployment. Areas still requiring review include user-scoped data access, authentication boundaries, WebSocket origin checks, rate limits, file validation, model output handling, and data retention.

Security fixes should be tested and reviewed before publication.
