# Zlux Server Framework Changelog

All notable changes to the Zlux Server Framework package will be documented in this file.
This repo is part of the app-server Zowe Component, and the change logs here may appear on Zowe.org in that section.

## 1.28.0

- Bugfix: keyring_js did not worked properly for finding CAs due to using an older version in package.json than needed for the listKeyring function
- Bugfix: Prevent loop upon EACCES error encountered when doing a TCP port bind

## 1.27.0

- Bugfix: Added small check on agent network configuration to avoid throwing an exception if the agent configuration included JWT information, but not network information

## 1.25.0

- Enhancement: Improved callRootService when targeting agents such as ZSS to issue the request direct to the destination rather than using an additional loopback request to the app-server first. This should improve performance, reduce the need for the app-server being a client of itself, and allow for more request options when calling the agent.
- Enhancement: Allow timeout parameter to be specified in a callService or callRootService command, such as when needing a long timeout to request an agent response.
- Enhancement: Removed need for app-server to be a client of itself when using the callService API, by adding the option for requests to this API to be executed internal to app-server. This option is very compatible with pre-existing use of callService but is disabled by default to avoid disruption. You can enable it by setting the server configuration property `node.internalRouting=true`
- Enhancement: Allow new pluginDefinition webContent destination variable for substitution, ZWE_EXTERNAL_PORT which matches the environment variable value, or if absent, the gateway or app-server ports conditional to configuration.
- Bugfix: App-server would ignore when `VERIFY_CERTIFICATE=false` was set, and try to verify APIML servers. This would lead to login failures when APIML server was on a different system than App-server. Now, app-server will or will not verify APIML certificates according to `VERIFY_CERTIFICATE` value.
- Bugfix: app-server could throw an uncaught exception when a proxied server had a socket error

## 1.24.0

- Bugfix: Fixed issue where server could not bind to a hostname value for node.https.ipAddresses
- Bugfix: Fixed issue where if the server started slowly, a timeout at cluster storage setup could be encountered.
- Bugfix: Fix no agent section causing server error (makes trivial-auth work)
- Enhancement: Add ciphers for use with TLS 1.3
- Enhancement: sso-auth plugin now sends error.body containing a server's error response so that it can be shown in browser if appropriate
- Enhancement: Version numbers of plugins are shown when loading, even if loading failed.

## 1.23.0

- Bugfix: Removed warning ZWED0144W about failure to read keyrings by having terminal proxy read the tls options loaded by the server instead of loading twice
- Bugfix: Dynamic plugins could not be loaded due to parameter mismatch
- Enhancement: Any HA incompatible security plugins are disabled if Zowe runs in HA mode.
- Enhancement: Updated proxy utility to treat PATCH similarly to PUT and POST
- Cleanup: Removed 'x-powered-by' header
- Enhancement: Get list of Discovery Services using environment variable, and provide the list for Eureka JS Client, in order to have it failover connection to the next one on the list when the one its currently talking to fails.

## 1.22.0

- Enhancement: Plugins can push state out to the Caching Service for high availability storage via a improved storage API, available to dataservices as `context.storage`
- Enhancement: Storage API V2 added which has parameters to specify whether plugin cache and state should be stored local to a worker, in the cluster, or remote for high availability
- Enhancement: Decrease verbosity and duplication of startup logs. Log messages omitted have been moved to debug messaging.
- Enhancement: Change missing swagger warning message to debug as it is a warning for developers, not for end users.
- Bugfix: Fix /server/agent route when using APIML
- Bugfix: Fix issue with CORS rejection when accessing zss through APIML gateway 

## 1.21.0

- Bugfix: Use hostname given by zowe config in order to avoid errors from the hostname certificate matching when accessing the app server through APIML
- Enhancement: app-server will contact zss through apiml if apiml is enabled and app-server finds that zss is accessible from apiml
- sso-auth plugin no longer keeps zss cookie within app-server; the cookie will now be sent to and used from the browser, to facilitate high availability

## 1.19.0

- Increased default retry attempts to connect to api discovery server from 3 to 100, for a total retry duration of a little over 15 minutes.

## 1.17.0

- Add support for DER encoded X.509 certificates

## 1.16.0

- [D] Feature: Expose GET /server/environment endpoint with minimal data when RBAC is off, to share only environment details that are required to do dependency checks and more accurate server-to-server communication (#237)
- Add support for PKCS#12 certificates and certificate authorities
- Enhancement: Added JSON plugin environment checks for App server and Agent components that verify if plugin requirements, specified in
the plugin definition, for OS, CPU, endpoints are satisfied.

## 1.15.0

- Bugfix: Fixed desktop prompting for session renewal and failure due to sso-auth plugin incorrectly stating that session refresh is possible when using Zowe SSO. In reality, the SSO tokens are non-renewable with expiration after several hours, leading to a prompt to re-authenticate to continue using the Desktop. This bugfix should now allow for that behavior.

## 1.14.0

- Bugfix: Plugin default server config could not exist in plugins own directory, and had to exist in the instance
- Bugfix: Terminal handlers had to exist within the root directory, rather than also being possible to exist within the instance directory
- Bugfix: Support more config parameters as env vars for edge case characters *, ., and _
- Bugfix: Fix for regression where session would expire prematurely because app server would not request a refresh from ZSS
- [A][D] Bugfix: RBAC checks will now use the Zowe instance value present in instance.env, when it is non-default (For compatibility; RBAC was defaulting to a different value from instance.env)
- Server will now load recognizers & actions from appDir/config/recognizers or /actions into the config

## 1.12.0

- Bugfix: Server handles if implementationDefaults or mediationLayer objects are missing
- Bugfix: SSH connecting from terminal-proxy was very slow on node v12+
- Bugfix: Lease info for mediation layer was a value that caused periodic heartbeat failure
- Add ability to state where a plugin path is relative to, instead of just where the server is running.
- Bugfix: Logout now allows security plugins to clear cookies
- Removed tokenInjector from sso-auth, since when SSO is being used token injection logic is not needed anymore.
- Bugfix: When trying to dynamically load a plugin with unmet dependencies, the response from the server would hang
- Support for reading keys, certificates, and certificate authority content from SAF keyrings via safkeyring:// specification in the node.https configuration object
- App server will now reattempt to connect to zss if it doesn't initially

