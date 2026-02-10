# OWASP CRS - Referer Hardening Plugin

## Description

This plugin introduces stricter validation of the `Referer` HTTP header in compliance with the relevant RFC specifications.

The plugin performs two main tasks:

- Verifies the syntactic validity of the `Referer` header.
- Extracts the individual components of the `Referer` URL (host, port, path, query) and appends them to the inspection targets of various CRS rules, such as those detecting SQL injections or other common web attacks.

The goal is to ensure that malicious payloads embedded in the `Referer` header are not ignored and instead are thoroughly inspected by existing CRS rules.

This plugin uses the `SecRuleUpdateTargetById` directive, which **cannot be skipped** via CRS plugin toggles. To disable this plugin, it **must be entirely removed** from the configuration.

## Plugin installation

For full and up to date instructions for the different available plugin installation methods, refer to [How to Install a Plugin](https://coreruleset.org/docs/concepts/plugins/#how-to-install-a-plugin) in the official CRS documentation.

## Plugin Configuration

This plugin does not offer any configuration directives or user-tunable settings.

## Testing

After installation, plugin should be tested, for example, using:  
`curl http://localhost --header "Referer: invalid referer header"`

Using default CRS configuration, this request should end with status 403 with
the following message in the log:

`ModSecurity: Warning. Match of "rx (?i)^(?:[a-z][a-z0-9+-.]*://|/|about:blank$)" against "REQUEST_HEADERS:Referer" required. [file "/path/plugins/referer-hardening-before.conf"] [line "65"] [id "9524110"] [msg "Invalid Referer header"] [data "invalid referer header"] [severity "CRITICAL"] [ver "referer-hardening-plugin/1.0.0"] [tag "application-multi"] [tag "language-multi"] [tag "platform-multi"] [tag "attack-protocol"] [tag "paranoia-level/1"] [tag "capec/1000/210/272"] [hostname "localhost"] [uri "/"] [unique_id "aEwf_tw2Cy8sdR-qD-IR6QAAAGs"]`

If you are running with a higher [Anomaly Threshold](https://coreruleset.org/docs/concepts/paranoia_levels/#how-paranoia-levels-relate-to-anomaly-scoring), you probably won't be blocked, but the alert message will still be there.

## License

Copyright (c) 2025-2026 OWASP CRS project. All rights reserved.

The OWASP CRS and its official plugins are distributed
under Apache Software License (ASL) version 2. Please see the enclosed LICENSE
