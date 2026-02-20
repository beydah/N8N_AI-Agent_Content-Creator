# Contributing to AI Content Creator Agent

Thank you for your interest in contributing to the AI Content Creator Agent! This document provides guidelines and information for contributors.

## 🤝 How to Contribute

We welcome contributions in many forms:

- Bug reports and feature requests
- Code improvements and new features
- Documentation updates
- Workflow optimizations
- API integrations
- Testing and quality assurance

## 📋 Getting Started

### Prerequisites

Before contributing, ensure you have:

- Docker and Docker Compose installed
- Basic understanding of n8n workflows
- Familiarity with the APIs used (Gemini, WordPress, LinkedIn, Google Drive)
- Git knowledge for version control

### Development Setup

1. **Fork the Repository**

   ```bash
   git clone https://github.com/beydah/AI-Agent_Content-Creator.git
   cd AI-Agent_Content-Creator
   ```

2. **Set Up Development Environment**

   ```bash
   # Follow the Docker setup instructions in README.md
   # Set up your test n8n instance
   # Configure test API credentials
   ```

3. **Create a Feature Branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

## 🐛 Reporting Bugs

When reporting bugs, please include:

### Bug Report Template

```markdown
**Bug Description**
A clear description of what the bug is.

**Steps to Reproduce**

1. Go to '...'
2. Click on '....'
3. Scroll down to '....'
4. See error

**Expected Behavior**
What you expected to happen.

**Actual Behavior**
What actually happened.

**Environment**

- OS: [e.g. Ubuntu 20.04]
- Docker Version: [e.g. 20.10.7]
- n8n Version: [e.g. 0.200.0]
- Browser: [e.g. Chrome 95.0]

**Screenshots**
If applicable, add screenshots to help explain your problem.

**Additional Context**
Add any other context about the problem here.
```

## 💡 Feature Requests

For feature requests, please provide:

### Feature Request Template

```markdown
**Feature Description**
A clear description of the feature you'd like to see.

**Problem Statement**
What problem does this feature solve?

**Proposed Solution**
How would you like this feature to work?

**Alternative Solutions**
Any alternative solutions you've considered.

**Use Cases**
Specific scenarios where this feature would be useful.

**Additional Context**
Any other context, mockups, or examples.
```

## 🔧 Development Guidelines

### Code Style

1. **n8n Workflow Standards**

   - Use descriptive node names
   - Add comments to complex expressions
   - Group related nodes visually
   - Use consistent naming conventions

2. **Documentation**

   - Update README.md for significant changes
   - Add inline comments for complex logic
   - Document new API integrations
   - Include setup instructions for new features

3. **Configuration**
   - Use environment variables for sensitive data
   - Provide example configurations
   - Document all required settings
   - Include validation for critical parameters

## 🤖 Agent Contribution Rules

To maintain high standards and interoperability, all agents must follow these rules:

1. **Storage Location**
   - All agents must be placed in the `agent` folder.
   - Example path: `agent/your_agent_name/`

2. **Required Files**
   - Each agent directory **must** contain:
     - `agent.json`: The exported n8n workflow.
     - `README.md`: Comprehensive documentation for the agent.

3. **Documentation Requirements**
   - The agent's `README.md` must include:
     - Download link for the `.json` file.
     - Return link to the main project [README.md](../../README.md).
     - Visual representation of nodes (using Mermaid diagrams).
     - Author information and last update date.

4. **Interoperability**
   - Agents should be designed to be called by other agents.
   - Implement **HTTP Request** nodes or **Execute Workflow** nodes for cross-agent communication.
   - Inputs and outputs should be standardized where possible.

### Workflow Modifications

When modifying the n8n workflow:

1. **Test Thoroughly**

   - Test with sample data
   - Verify all API connections
   - Check error handling
   - Validate output formats

2. **Maintain Backward Compatibility**

   - Don't break existing configurations
   - Provide migration guides if needed
   - Document breaking changes clearly

3. **Performance Considerations**
   - Optimize API calls
   - Handle rate limits properly
   - Implement proper error retry logic
   - Consider memory usage for large datasets

### API Integration Guidelines

When adding new API integrations:

1. **Security First**

   - Use OAuth2 when available
   - Store credentials securely
   - Implement proper token refresh
   - Follow API security best practices

2. **Error Handling**

   - Implement comprehensive error handling
   - Provide meaningful error messages
   - Add retry logic for transient failures
   - Log errors appropriately

3. **Documentation**
   - Document API setup process
   - Include credential configuration steps
   - Provide troubleshooting guides
   - Add usage examples

## 🧪 Testing

### Testing Checklist

Before submitting changes:

- [ ] Workflow imports successfully
- [ ] All API connections work
- [ ] Error handling works as expected
- [ ] Documentation is updated
- [ ] No sensitive data is exposed

### Test Environment

Set up a test environment with:

- Separate API credentials for testing

## 📝 Pull Request Process

### Before Submitting

1. **Update Documentation**

   - Update README.md if needed
   - Add/update API setup instructions
   - Document new features or changes

2. **Test Your Changes**

   - Run through the complete workflow
   - Test error scenarios
   - Verify all integrations work

3. **Clean Up**
   - Remove test data and credentials
   - Clean up temporary files
   - Ensure no sensitive information is included

### Pull Request Template

```markdown
## Description

Brief description of changes made.

## Type of Change

- [ ] Bug fix
- [ ] New feature
- [ ] Documentation update
- [ ] Performance improvement
- [ ] Refactoring

## Testing

- [ ] Tested locally
- [ ] All API integrations work
- [ ] Documentation updated
- [ ] No breaking changes

## Screenshots

If applicable, add screenshots of the changes.

## Checklist

- [ ] Code follows project guidelines
- [ ] Self-review completed
- [ ] Documentation updated
- [ ] No sensitive data included
```

## 🏷️ Versioning

We use [Semantic Versioning](https://semver.org/):

- **MAJOR**: Breaking changes
- **MINOR**: New features (backward compatible)
- **PATCH**: Bug fixes (backward compatible)

## 📚 Resources

### Helpful Links

- [n8n Documentation](https://docs.n8n.io/)
- [Google Gemini API Docs](https://ai.google.dev/docs)

### Learning Resources

- [n8n Community](https://community.n8n.io/)
- [Docker Documentation](https://docs.docker.com/)

## 🎯 Contribution Areas

We especially welcome contributions in:

### High Priority

- **Error Handling**: Improve robustness and error recovery
- **Performance**: Optimize API calls and workflow execution
- **Security**: Enhance credential management and data protection
- **Documentation**: Improve setup guides and troubleshooting

### Medium Priority

- **New Integrations**: Add support for other platforms (Twitter, Facebook, etc.)
- **Content Templates**: Create templates for different content types
- **Scheduling**: Add workflow scheduling capabilities
- **Analytics**: Integrate performance tracking

### Low Priority

- **UI Improvements**: Enhance workflow visual organization
- **Localization**: Add support for multiple languages
- **Advanced Features**: AI model selection, custom prompts
- **Monitoring**: Add health checks and monitoring

## 🏆 Recognition

Contributors will be recognized in:

- README.md contributors section
- Release notes for significant contributions
- Project documentation
- Community acknowledgments

## 📞 Getting Help

If you need help:

- Check existing issues and discussions
- Review the documentation
- Ask questions in issues (use the "question" label)
- Reach out to maintainers

## 📄 License

By contributing, you agree that your contributions will be licensed under the same license as the project.

## 🙏 Thank You

Thank you for contributing to the AI Content Creator Agent! Your contributions help make automated content creation accessible to everyone.

---

**Happy Contributing! 🚀**
