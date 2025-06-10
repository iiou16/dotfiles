# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This repository contains comprehensive development rules and guidelines for Claude Code, designed to be used in `~/.claude/CLAUDE.md`. The rules are organized into specialized files for different development environments with advanced file referencing capabilities.

## Key Features

- **File Reference System**: Uses `@syntax` to dynamically import specialized guidelines
- **Modular Architecture**: Separated by development context for better organization  
- **Multilingual Support**: Japanese base rules with English specialized guidelines
- **Practical Implementation**: Real-world development patterns and tool configurations

## File Structure

### Core Files
- `.claude/CLAUDE.md` - Main configuration file with file references (Japanese)
- `CLAUDE.md` - Basic development rules documentation (Japanese)

### Specialized Guidelines (.claude/)
- `python_dev.md` - Python development comprehensive guide (English)
  - uv package management rules
  - Strict type hints requirements  
  - pytest/anyio testing strategies
  - Ruff/pyright code quality tools

- `typescript_dev.md` - TypeScript development comprehensive guide (English)
  - Strict TypeScript configuration
  - npm/yarn package management
  - Jest/Vitest testing frameworks
  - Prettier/ESLint code formatting

- `web_app_dev.md` - Full-stack web application guide (English)
  - FastAPI + React architecture
  - Full-stack integration patterns
  - Security, testing, and deployment
  - API-driven development methodology

## Implementation

The main `.claude/CLAUDE.md` uses file referencing to dynamically load appropriate guidelines:
```markdown
- Python開発: @python_dev.md
- TypeScript開発: @typescript_dev.md  
- Webアプリケーション開発（FastAPI + React）: @web_app_dev.md
```

## Development Philosophy

This rules system implements:
- **Test-Driven Development (TDD)** as the primary development approach
- **Strict typing and code quality** across all languages
- **Comprehensive toolchain integration** for each technology stack
- **Security-first development practices** especially for web applications
- **Performance optimization** and deployment best practices

## Recent Improvements

- Implemented file reference system using `@syntax`
- Added comprehensive Python development rules with uv/Ruff/pyright
- Created strict TypeScript development guidelines  
- Established full-stack web application development framework
- Integrated TDD philosophy and commit message standards
- Added security checklists and performance optimization guides