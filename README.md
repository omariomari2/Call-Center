# PersonalBanker

An AWS voice banking lab built around Amazon Lex, Amazon Connect, and Lambda.
The exercise uses mock account information to explore call routing and intent handling.

## Repository contents

- [Lab 1](Lab1.md): configure the bot and its intents.
- [Lab 2](Lab2.md): connect the bot to a telephone flow.
- [Contact flow](lambda.json): an Amazon Connect export, despite its filename.

## Scope

The repository contains lab notes and a flow export. It does not include the Lambda source referenced by the notes.
The notes use an older Node.js runtime. Select a currently supported runtime before recreating the lab.
PIN examples are mock logic, not a banking authentication system.

Use an isolated AWS account and synthetic records. AWS resources can incur charges.
Delete the lab resources and release the phone number when you finish.

