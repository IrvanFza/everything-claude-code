# everything-claude-code Development Patterns

> Auto-generated skill from repository analysis

## Overview

This repository contains development patterns and utilities for the Claude Code plugin system. It's a JavaScript-based project that focuses on plugin configuration, cross-platform compatibility, security fixes, and documentation enhancement. The codebase emphasizes iterative improvement and robust plugin system integration.

## Coding Conventions

### File Naming
- Use **camelCase** for JavaScript files
- Example: `setupPackageManager.js`, `utils.js`

### Import/Export Style
- Mixed import styles are acceptable
- Use ES6 modules where possible:
```javascript
import { someFunction } from './utils.js';
export const myFunction = () => {};
```

### Commit Messages
- Follow conventional commit format
- Keep messages around 52 characters
- Use prefixes: `fix:`, `feat:`, `docs:`, `style:`, `revert:`
- Examples:
  - `fix: update plugin.json configuration paths`
  - `feat: add cross-platform hook support`

### Testing
- Test files use `*.test.js` pattern
- Place tests in `tests/` directory structure

## Workflows

### Plugin Configuration Fix
**Trigger:** When plugin installation or loading fails due to configuration errors  
**Command:** `/fix-plugin-config`

1. Identify the specific plugin system error from logs or user reports
2. Open `.claude-plugin/plugin.json` and examine configuration
3. Common fixes:
   - Correct file paths (use relative paths or `${CLAUDE_PLUGIN_ROOT}`)
   - Update plugin metadata (version, name, description)
   - Fix JSON syntax errors
4. Test plugin loading in development environment
5. Commit with descriptive message: `fix: update plugin.json configuration`

```json
// Example plugin.json fix
{
  "name": "claude-code",
  "version": "1.0.0",
  "main": "./src/index.js",
  "hooks": "./hooks/hooks.json"
}
```

### Hooks Path Correction
**Trigger:** When hooks fail to execute due to incorrect path resolution  
**Command:** `/fix-hooks-paths`

1. Identify hook execution failures in the plugin system
2. Open `hooks/hooks.json` and examine path configurations
3. Update paths using one of these strategies:
   - Use `${CLAUDE_PLUGIN_ROOT}` variable for dynamic resolution
   - Use relative paths from plugin root
   - Ensure paths work in both development and installed contexts
4. Test hook execution in different environments
5. Commit fix: `fix: correct hook file paths for installation context`

```json
// Example hooks.json with correct paths
{
  "pre-commit": "${CLAUDE_PLUGIN_ROOT}/scripts/hooks/preCommit.js",
  "post-install": "./scripts/setup.js"
}
```

### Security Vulnerability Fix
**Trigger:** When security vulnerabilities are identified in existing code  
**Command:** `/security-fix`

1. Identify the specific security issue (command injection, XSS, etc.)
2. Locate affected files, commonly:
   - `scripts/lib/utils.js`
   - `scripts/setup-package-manager.js`
   - Agent documentation files
3. Implement security fixes:
   - Sanitize user inputs
   - Use parameterized commands
   - Validate file paths
   - Escape output appropriately
4. Add security warnings to documentation
5. Test fixes thoroughly
6. Commit: `fix: address security vulnerability in utility scripts`

```javascript
// Example security fix
// Before (vulnerable)
exec(`npm install ${packageName}`);

// After (secure)
exec('npm install', [packageName], { stdio: 'inherit' });
```

### README Enhancement Iteration
**Trigger:** When documentation needs better organization or visual appeal  
**Command:** `/improve-readme`

1. Identify specific improvement opportunities:
   - Structure reorganization
   - Missing sections
   - Outdated information
   - Visual elements (badges, images, diagrams)
2. Update README.md with improvements:
   - Add clear sections and headers
   - Include code examples
   - Add badges for build status, version, etc.
   - Improve formatting and readability
3. Test markdown rendering on different platforms
4. Commit: `docs: enhance README structure and visual elements`

### Cross-Platform Compatibility
**Trigger:** When bash-only scripts need to work on Windows/macOS/Linux  
**Command:** `/cross-platform-convert`

1. Identify bash-only scripts that limit platform support
2. Create equivalent Node.js implementations:
   - Use `path` module for file system operations
   - Use `child_process` for system commands
   - Handle platform-specific differences
3. Add cross-platform utilities to `scripts/lib/`
4. Create comprehensive tests for all platforms
5. Update `hooks/hooks.json` to reference new Node.js scripts
6. Commit: `feat: convert bash scripts to Node.js for cross-platform support`

```javascript
// Example cross-platform utility
const path = require('path');
const { execSync } = require('child_process');

function installPackage(packageName) {
  const isWindows = process.platform === 'win32';
  const npmCmd = isWindows ? 'npm.cmd' : 'npm';
  execSync(`${npmCmd} install ${packageName}`, { stdio: 'inherit' });
}
```

## Testing Patterns

- Place test files alongside source files or in dedicated `tests/` directory
- Use `*.test.js` naming convention
- Focus on cross-platform compatibility testing
- Test plugin configuration changes before committing
- Validate hook execution in different environments

## Commands

| Command | Purpose |
|---------|---------|
| `/fix-plugin-config` | Fix plugin.json configuration issues |
| `/fix-hooks-paths` | Correct hook file paths for proper resolution |
| `/security-fix` | Address security vulnerabilities in code |
| `/improve-readme` | Enhance documentation structure and content |
| `/cross-platform-convert` | Convert bash scripts to Node.js for compatibility |