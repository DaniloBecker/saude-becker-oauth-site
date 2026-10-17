# Saúde Becker OAuthReader

Technical documentation website for **Saúde Becker OAuthReader**, a personal software-development utility that provides persistent, read-only access to authorized Google Apps Script projects.

## Purpose

OAuthReader supports engineering and operational tasks in the Saúde Becker project, including authenticated source inspection, diagnostics, automation continuity and maintenance. It is not a commercial service and is not offered as a public OAuth platform.

## Implementation

The Windows implementation uses:

- Google OAuth 2.0
- `openid`
- `email`
- `https://www.googleapis.com/auth/script.projects.readonly`
- PowerShell
- Windows DPAPI (`CurrentUser`) for local refresh-credential protection
- ephemeral access tokens
- explicit subject, email, audience and exact-scope validation
- fail-closed behavior for invalid bindings, missing credential stores or refresh failures

The sole Google resource scope is `script.projects.readonly`; the reader does not request mutation privileges for Google Apps Script projects.

## Authentication flow

1. An authorized user performs a one-time OAuth enrollment.
2. The authenticated Google identity, audience and granted scopes are verified.
3. Refresh material is protected locally with Windows DPAPI.
4. Later sessions perform non-interactive token refresh.
5. A short-lived access token is used only for authorized Apps Script API read operations.
6. Access tokens are not persisted by the reader.

## Validation

The current implementation has been tested for:

- real OAuth refresh in a new PowerShell process;
- real OAuth refresh after Windows reboot;
- expected account and subject binding;
- expected OAuth audience;
- exact granted scope set;
- rejection of wrong identity binding;
- rejection of a missing protected credential store;
- authorized Apps Script project reads;
- absence of resource-write execution;
- absence of persistent access-token storage.

## Website

The GitHub Pages site contains the public technical description and privacy policy used for the OAuth application's public-facing documentation.

- Project site: https://danilobecker.github.io/saude-becker-oauth-site/
- Privacy policy: https://danilobecker.github.io/saude-becker-oauth-site/privacidade.html
- Maintainer: Danilo Becker
- Contact: danilobecker@hotmail.com
