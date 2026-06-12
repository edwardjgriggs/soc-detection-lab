# Telemetry Generation

Detections are only as good as the activity you test them against. This
document describes how I safely produce the telemetry that drives detection
development. Every technique here runs against lab-only identities and machines
that I own.

## Principles

1. Only generate activity against accounts and hosts you own.
2. Never touch production or any third-party system.
3. Document the expected log result for each reproduction so validation is
   repeatable.

## Password spray reproduction

From a single host, attempt one failed sign-in against each of several test
accounts inside a short window. This produces the wide, shallow failure pattern
that the password spray detection looks for.

## Brute force reproduction

Attempt many failed sign-ins against a single test account inside a short
window. To validate the compromise follow-on, perform one successful sign-in
to the same account immediately after the failure burst.

## Suspicious inbox rule reproduction

In a lab mailbox, create an inbox rule that forwards mail to a second test
address you control. Allow for ingestion lag before checking the logs.

## Atomic Red Team (roadmap)

The next step is mapping individual ATT&CK techniques to Atomic Red Team tests,
so each detection has a known trigger and a documented expected result. Pairing
a detection with its atomic test closes the loop between attack and alert and
makes validation push-button rather than manual.

## Cleanup

After each exercise, remove test inbox rules, unlock any locked test accounts,
and note the run in the validation file for the relevant detection.
