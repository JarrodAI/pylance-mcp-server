# Contributing to Pylance MCP Server

Thank you for your interest in contributing! This document provides guidelines and information for contributors.

## Getting Started

1. **Fork the repository**
2. **Clone your fork**
   ```bash
   git clone https://github.com/YOUR_USERNAME/pylance-mcp-server.git
   cd pylance-mcp-server
   ```

3. **Set up development environment**
   ```bash
   # On Unix/macOS
   ./setup.sh
   
   # On Windows
   setup.bat
   ```

4. **Create a branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

## Development Workflow

### Code Style

We use **Black** for Python formatting and **isort** for import sorting.

```bash
# Format code
black .

# Sort imports
isort .

# Type check
mypy mcp_server/
```

### Testing

All new features must include tests.

```bash
# Run tests
pytest tests/ -v

# Run specific test
pytest tests/test_pylance_mcp.py::TestPylanceTools::test_get_hover

# With coverage
pytest tests/ --cov=mcp_server --cov-report=html
```

### Adding a New Tool

1. **Add the tool function to `mcp_server/tools.py`**
   ```python
   def my_new_tool(self, file_path: str, content: str) -> Dict[str, Any]:
       """Brief description."""
       try:
           validated_path = self.bridge._validate_path(file_path)
           self.bridge.open_document(str(validated_path), content)
           
           params = {
               "textDocument": {"uri": self.bridge._file_uri(str(validated_path))},
               # ... other params
           }
           
           response = self.bridge.send_request("textDocument/myMethod", params)
           self.bridge.close_document(str(validated_path))
           
           return response.get("result", {})
       except Exception as e:
           logger.error(f"my_new_tool error: {e}")
           return {}
   ```

2. **Register in `server.py`**
   ```python
   @mcp.tool()
   def my_new_tool(file_path: str, content: str) -> Dict[str, Any]:
       """Brief description for MCP clients."""
       if not tools:
           raise RuntimeError("Server not initialized")
       return tools.my_new_tool(file_path, content)
   ```

3. **Add tests in `tests/test_pylance_mcp.py`**
   ```python
   def test_my_new_tool(self, tools, calculator_content):
       """Test my_new_tool."""
       result = tools.my_new_tool("calculator.py", calculator_content)
       assert isinstance(result, dict)
       # ... more assertions
   ```

4. **Update README.md** with the new tool documentation

### Adding a New Resource

1. **Add the resource function to `mcp_server/resources.py`**
   ```python
   def my_resource(self, path: str) -> str:
       """Brief description."""
       try:
           # ... implementation
           return result
       except Exception as e:
           logger.error(f"my_resource error: {e}")
           raise
   ```

2. **Register in `server.py`**
   ```python
   @mcp.resource("my-resource://{path}")
   def my_resource(path: str) -> str:
       """Brief description for MCP clients."""
       if not resources:
           raise RuntimeError("Server not initialized")
       return resources.my_resource(path)
   ```

3. **Add tests**

4. **Update README.md**

## Pull Request Process

1. **Ensure all tests pass**
   ```bash
   pytest tests/ -v
   ```

2. **Format your code**
   ```bash
   black .
   isort .
   ```

3. **Update documentation**
   - Add docstrings to new functions
   - Update README.md if needed
   - Add/update ARCHITECTURE.md if changing design

4. **Commit with clear messages**
   ```bash
   git commit -m "Add: my_new_tool for symbol search
   
   - Implements workspace-wide symbol search
   - Adds tests for various symbol types
   - Updates README with usage examples"
   ```

5. **Push to your fork**
   ```bash
   git push origin feature/your-feature-name
   ```

6. **Create Pull Request**
   - Describe what the PR does
   - Link any related issues
   - Include test results
   - Add screenshots/examples if applicable

## Code Review Process

1. Maintainer reviews code
2. Automated tests run
3. Discussion/changes if needed
4. Approval + merge

## Reporting Bugs

Use GitHub Issues with this template:

```markdown
**Describe the bug**
A clear description of what the bug is.

**To Reproduce**
Steps to reproduce:
1. Start server with '...'
2. Call tool '...'
3. See error

**Expected behavior**
What you expected to happen.

**Environment**
- OS: [e.g., Windows 11, macOS 14, Ubuntu 22.04]
- Python version: [e.g., 3.11.5]
- Node.js version: [e.g., 20.10.0]
- Pyright version: [e.g., 1.1.350]

**Additional context**
Any other information about the problem.
```

## Feature Requests

Use GitHub Issues with this template:

```markdown
**Feature description**
Clear description of the feature.

**Use case**
Why is this feature needed? What problem does it solve?

**Proposed solution**
How you think this could be implemented.

**Alternatives considered**
Other approaches you've thought about.
```

## Communication

- **GitHub Issues**: Bug reports, feature requests
- **Pull Requests**: Code contributions
- **Discussions**: Questions, ideas, general chat

## License

By contributing, you agree that your contributions will be licensed under the MIT License.

## Questions?

Open an issue or discussion on GitHub!
