# Security Improvements and Privacy Enhancements

This document outlines the security and privacy improvements implemented in VSCodium.

## Telemetry Disabled by Default

All telemetry is disabled out of the box:

```json
{
  "telemetry.enableTelemetry": false,
  "telemetry.enableCrashReporter": false,
  "workbench.enableExperiments": false,
  "chat.commandCenter.enabled": false
}
```

### What's Disabled

1. **Usage Telemetry**: No data sent about feature usage
2. **Crash Reports**: No automatic crash data collection
3. **A/B Testing**: No participation in Microsoft experiments
4. **Natural Language Search**: Disabled (sends queries to Microsoft)

## Offline Mode

Complete offline operation available:

```json
{
  "offlineMode.enabled": true,
  "offlineMode.disableExtensions": true,
  "offlineMode.disableGallery": true
}
```

### Features When Offline

- ✅ Local LLM inference (llama.cpp)
- ✅ Code editing and debugging
- ✅ Extensions already installed
- ✅ All local features

- ❌ Extension marketplace
- ❌ Update checks
- ❌ Remote repositories
- ❌ Cloud sync

## Command Filtering

Security patch adds command filtering to prevent malicious extensions from executing dangerous commands.

### Protected Commands

- System shell execution
- File system operations outside workspace
- Network requests to untrusted domains
- Native module loading

## Extension Security

### Malicious Extension Protection

Option to scan extensions for known malicious patterns:

```json
{
  "security.scanExtensions": true,
  "security.blockMaliciousExtensions": true
}
```

### Signature Verification

Extension signature verification can be enabled:

```json
{
  "extensions.verifySignature": true
}
```

## Network Security

### CORS Proxy Warnings

⚠️ **Warning**: Built-in CORS proxy is experimental
- Do not expose to untrusted environments
- Intended for local development only
- May be removed or changed in future versions

### Connection Management

Extensions must explicitly declare network permissions:
```json
{
  "extensionNetworkPermissions": {
    "allowedDomains": ["api.trusted-service.com"],
    "blockUnknown": true
  }
}
```

## Local LLM Security

### Model Loading Security

When using llama.cpp integration:

1. **Model Verification**: Check model hashes before loading
2. **Path Validation**: Only load from trusted directories
3. **Resource Limits**: Prevent DoS via large models

```json
{
  "llamaCpp.trustedPaths": ["/home/user/models"],
  "llamaCpp.maxModelSizeGB": 50,
  "llamaCpp.verifyHashes": true
}
```

### Prompt Injection Protection

Built-in protections against prompt injection attacks:

```json
{
  "llamaCpp.promptValidation": true,
  "llamaCpp.maxPromptLength": 100000
}
```

## Data Protection

### Workspace Isolation

Each workspace is isolated:
- Separate extension state
- Isolated cache directories
- No cross-workspace data leakage

### Encryption Options

For sensitive projects:
```json
{
  "security.encryptWorkspace": true,
  "security.encryptionAlgorithm": "AES-256-GCM"
}
```

## Audit Logging

Enable security audit logging:

```json
{
  "security.auditLog": true,
  "security.auditLogPath": "/var/log/vscodium/security.log",
  "security.logLevel": "info"
}
```

### Logged Events

- Extension installations
- Network connections
- File access outside workspace
- Command executions
- Authentication events

## Best Practices

### For Users

1. **Keep Updated**: Regularly update VSCodium
2. **Review Extensions**: Only install from trusted sources
3. **Use Offline Mode**: When internet not required
4. **Monitor Logs**: Check audit logs periodically

### For Developers

1. **Minimal Permissions**: Request only necessary permissions
2. **Transparent Network Use**: Declare all network endpoints
3. **Secure Defaults**: Ship with secure default settings
4. **Regular Audits**: Review code for security issues

## Security Advisories

### Known Limitations

1. **CORS Proxy**: Experimental, do not expose publicly
2. **Built-in Tools**: May change in future versions
3. **LLM Integration**: Models may have their own vulnerabilities

### Reporting Vulnerabilities

Report security issues to: security@vscodium.com

Please include:
- VSCodium version
- Steps to reproduce
- Impact assessment
- Suggested fix (if any)

## Compliance

### GDPR

VSCodium complies with GDPR by:
- Not collecting personal data by default
- Providing offline operation
- Allowing complete data deletion

### SOC 2

For enterprise deployments:
- Audit logging available
- Access controls implemented
- Data encryption supported

## Security Checklist

Before deploying in production:

- [ ] Telemetry disabled (default)
- [ ] Unnecessary extensions removed
- [ ] Offline mode enabled if possible
- [ ] Audit logging configured
- [ ] Network permissions reviewed
- [ ] Extension sources verified
- [ ] Latest security patches applied
- [ ] Backup strategy in place

## Resources

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [VSCode Security Guidelines](https://code.visualstudio.com/api/references/security)
- [Extension Security Best Practices](https://code.visualstudio.com/api/references/extension-guidelines)
