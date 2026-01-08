<!-- 
 * AIR_GAP_SECURITY.md
 * 
 * @copyright Lackadaisical Security Inc. 2025
 * @version 3.4.0
-->
# Air Gap Security Features

## Overview

The Lackadaisical Traffic Emulator System includes comprehensive security features designed for air-gapped environments. These features ensure that the system can operate safely in environments with strict network isolation requirements.

## Components

The air gap security architecture consists of the following components:

- **Air Gap System Controller**: Central controller for air-gapped deployments
- **Air Gap Executable Validator**: Validates executable files and scripts for integrity and security
- **Air Gap Boot Verifier**: Ensures system integrity during startup
- **Air Gap Config Verifier**: Validates configuration files for security compliance
- **Unified Security Framework**: Integrates all security components into a centralized framework
- **Security Testing Framework**: Provides tools for security testing in isolated environments
- **Air Gap Packager**: Prepares deployments for air-gapped environments
- **Sandbox Manager**: Manages secure sandboxed environments for browser automation
- **Docker Container Manager**: Secure container management for isolated environments

## Getting Started

### Basic Usage

To enable air gap security in your application:

```javascript
const AirGapSystemController = require('./src/controllers/air-gap-system-controller');
const UnifiedSecurityFramework = require('./src/security/unified-security-framework');

async function initializeSecurity() {
  // Create unified security framework
  const securityFramework = new UnifiedSecurityFramework({
    airGapMode: true // Enable air gap mode
  });
  
  // Initialize security components
  await securityFramework.initialize();
  
  // Create air gap system controller
  const airGapController = new AirGapSystemController({
    securityFramework,
    configDir: './config/security'
  });
  
  await airGapController.initialize();
  
  return { securityFramework, airGapController };
}
```

### Securing Nightmare Instances

To secure Nightmare instances for air-gapped environments:

```javascript
const Nightmare = require('nightmare');
const SandboxManager = require('./security/sandbox-manager');
const { securityFramework } = await initializeSecurity();

// Create sandbox manager
const sandboxManager = new SandboxManager({
  dockerization: true,
  maxProfiles: 10
});

await sandboxManager.initialize();

// Create Nightmare instance
const nightmare = Nightmare({
  show: true,
  webPreferences: {
    webSecurity: true
  }
});

// Secure the instance with sandbox
const sandboxId = await sandboxManager.createSandbox({ 
  secure: true,
  airGapMode: true
});

// Now use Nightmare with air gap security enforcement
await nightmare
  .goto('http://localhost:3000') // Only local URLs will be allowed
  .wait(1000)
  .end();

// Cleanup sandbox when done
await sandboxManager.cleanupSandbox(sandboxId);
```

## Security Validation and Assessment

### Executable Validation

To validate executables in a directory:

```javascript
const AirGapExecutableValidator = require('./src/security/air-gap-executable-validator');

async function validateExecutables() {
  const validator = new AirGapExecutableValidator({
    configDir: './config/security',
    knownHashesFile: './config/security/executable-hashes.json'
  });
  
  await validator.initialize();
  
  // Scan a directory recursively
  const results = await validator.scanDirectory('./bin', {
    updateHashes: false,
    maxDepth: 5
  });
  
  console.log(`Scanned ${results.scannedFiles} files`);
  console.log(`Valid files: ${results.validFiles}`);
  console.log(`Invalid files: ${results.invalidFiles}`);
  
  return results;
}
```

### Security Assessment

To run a security assessment:

```javascript
const { securityFramework } = await initializeSecurity();

// Perform comprehensive security assessment
const assessment = await securityFramework.performSecurityAssessment();

console.log(`Overall security status: ${assessment.overallStatus}`);
console.log(`Issues found: ${assessment.issues.length}`);

// Handle critical issues
const criticalIssues = assessment.issues.filter(issue => issue.severity === 'critical');
if (criticalIssues.length > 0) {
  console.error('Critical security issues found:', criticalIssues);
}
```

### Running Security Tests

To run security tests:

```javascript
const SecurityTestingFramework = require('./security/security-testing-framework');

async function runSecurityTests() {
  const testFramework = new SecurityTestingFramework({
    testsDir: './tests/security',
    reportDir: './reports/security',
    autoConfigure: true
  });
  
  await testFramework.initialize();
  
  const results = await testFramework.runTests({
    category: 'air-gap',
    severity: 'high'
  });
  
  console.log(`Tests completed with ${results.findings.length} findings`);
  return results;
}
```

## Docker Security Features

The system includes Docker container security features for air-gapped environments:

```javascript
const { sandboxManager } = await initializeSecurity();

// Clean up unused Docker containers for security
await sandboxManager.cleanupDockerContainers();

// Set up Docker for containerized browser instances
await sandboxManager.setupDocker();

// Check if Docker is properly configured
const isDockerReady = await sandboxManager.checkDockerConfiguration();

// Create secure Docker container
const containerId = await sandboxManager.createDockerizedSandbox({
  image: 'zenika/alpine-chrome:latest',
  securityOpts: ['no-new-privileges']
});

// Clean up when done
await sandboxManager.cleanupDockerContainers();
```

## Configuration Import/Export

For secure configuration transfer between environments:

```javascript
const { airGapController } = await initializeSecurity();

// Export security configuration
const exportResults = await airGapController.exportSecurityConfig({
  outputDir: './export',
  includeHashes: true,
  includeCertificates: true
});

console.log(`Exported to ${exportResults.outputPath}`);

// Import security configuration
const importResults = await airGapController.importSecurityConfig({
  inputDir: './import',
  validateIntegrity: true
});

console.log(`Imported ${importResults.fileCount} configuration files`);
```

## Configuration

### Environment Variables

- `AIR_GAP_MODE`: Set to "true" to enable air gap mode system-wide

### Configuration Files

The following configuration files control air gap security behavior:

- `config/security/air-gap-allowed-domains.json`: List of allowed domains in air gap mode
- `config/security/air-gap-policies/`: Security policies for air-gapped operation
- `config/security/executable-whitelist.json`: Whitelisted executable paths
- `config/security/executable-hashes.json`: Known and trusted executable hashes
- `config/security/certificates/`: Trusted certificates for air-gapped operation

## Security Policies

The system includes several predefined security policies:

- **standard**: Default security policy with moderate restrictions
- **air-gap-strict**: Strict security policy for air-gapped environments
- **development**: Relaxed security for development environments

You can create custom security policies to meet specific requirements.

## Running Security Validation

To validate the air gap security configuration:

```bash
node src/tools/air-gap-security-validator.js
```

This will run a comprehensive validation of all security components and generate a report.

## Best Practices

1. **Regularly clean up unused profiles and containers** using the sandbox manager
2. **Validate executables before use** in air-gapped environments
3. **Perform periodic security assessments** to identify potential issues
4. **Use Docker containerization** for additional isolation
5. **Export and backup security configurations** regularly
6. **Apply the appropriate security policy** based on your security requirements
7. **Run comprehensive security tests** before deployment

## Troubleshooting

### Common Issues

- **Docker container persistence**: Use `sandbox.cleanupDockerContainers()` to remove unused containers
- **Certificate validation errors**: Ensure certificates are properly trusted
- **Executable blocking**: Check the executable whitelist configuration
- **File integrity issues**: Run `securityFramework._checkFileIntegrity()` to validate files

### Logs

Security-related logs can be found in the standard log files with the following module prefixes:

- `AirGapSystemController`
- `AirGapExecutableValidator`
- `UnifiedSecurityFramework`
- `SecurityTestingFramework`
- `SandboxManager`

## Further Reading

- [Network Security Configuration](./NETWORK_SECURITY.md)
- [Certificate Management](./CERTIFICATE_MANAGEMENT.md)
- [Security Policies](./SECURITY_POLICIES.md)
- [Docker Security](./DOCKER_SECURITY.md)
