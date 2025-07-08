# Python Project Template

[![CI](https://github.com/mberetvas/python-template/actions/workflows/ci-python.yml/badge.svg)](https://github.com/mberetvas/python-template/actions/workflows/ci-python.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A professional, modern, and opinionated Python project template designed to kickstart your development process. This template provides a clean and organized structure, incorporating best practices and popular tools like `uv`, `pytest`, and `ruff`.

## Table of Contents

- [Features](#features)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
- [Development](#development)
  - [Running the Application](#running-the-application)
  - [Running Tests](#running-tests)
  - [Code Quality](#code-quality)
  - [Configuring Ruff](#configuring-ruff)
- [AI-Powered Development](#ai-powered-development)
  - [GitHub Copilot Integration](#github-copilot-integration)
  - [Using Custom Prompts](#using-custom-prompts)
  - [Available Prompts](#available-prompts)
- [Project Structure](#project-structure)
- [Contributing](#contributing)
- [License](#license)

## Features

-   **Modern Tooling**: Utilizes `uv` for fast package management.
-   **Organized Structure**: A clean `src/` layout for source code and `tests/` for tests.
-   **Testing Ready**: Comes with `pytest` configured for running tests with JUnit XML output.
-   **Code Quality**: Integrated with `ruff` for linting and formatting to maintain high code standards.
-   **CI/CD Ready**: Includes a basic GitHub Actions workflow for continuous integration.
-   **Version Control**: Pre-configured `.gitignore` file with comprehensive Python exclusions.
-   **VS Code Integration**: Pre-configured VS Code settings for optimal development experience.
-   **AI-Powered Development**: Extensive collection of GitHub Copilot prompts for automated code generation, testing, and review.
-   **Pull Request Templates**: Structured PR templates for consistent contributions.

## Getting Started

Follow these instructions to get a copy of the project up and running on your local machine for development and testing purposes.

### Prerequisites

Make sure you have [uv](https://github.com/astral-sh/uv) installed.

### Installation

1.  **Create a new repository from this template.**
    Click the "Use this template" button at the top of the repository page on GitHub.

2.  **Clone your new repository.**
    ```sh
    git clone https://github.com/YOUR-USERNAME/YOUR-REPOSITORY-NAME.git
    cd YOUR-REPOSITORY-NAME
    ```

3.  **Create and activate a virtual environment.**
    This command creates a virtual environment in a `.venv` directory.
    ```sh
    uv init
    uv venv
    ```
    Activate it:
    -   On macOS/Linux: `source .venv/bin/activate`
    -   On Windows: `.venv\Scripts\activate`

4.  **Install dependencies.**
    Install any necessary packages for your project. For example:
    ```sh
    uv add -r requirements.txt
    ```
    *(Note: You'll need to create a `requirements.txt` file for your project's dependencies.)*

## Development

### Running the Application

To run the example entry point when the venv has been activated:
```sh
python src/main.py
```
If the the venv is not activated use:
```sh
uv run src/main.py
```

### Running Tests

This template uses `pytest` for testing with JUnit XML output for CI/CD integration.

```sh
# Install pytest if you haven't already
uv add --dev pytest

# Run tests with JUnit XML output
pytest --junitxml=junit/test-results.xml

# Run tests (simple)
pytest
```

The test results are automatically saved to `junit/test-results.xml` for integration with CI/CD systems.

### Code Quality

 [VS Code](https://docs.astral.sh/ruff/editors/setup/#vs-code)

Install the Ruff extension from the [VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=charliermarsh.ruff). It is recommended to have the Ruff extension version `2024.32.0` or later to get the best experience with the Ruff Language Server.

For more documentation on the Ruff extension, refer to the [README](https://github.com/astral-sh/ruff-vscode/blob/main/README.md) of the extension repository.

Or by simply using the ruff tool:

This template uses `ruff` for linting and formatting.
```sh
# Install Ruff globally.
uv tool install ruff@latest

# Once installed, you can run Ruff from the command line:

ruff check   # Lint all files in the current directory.
ruff format  # Format all files in the current directory.
```

### Configuring Ruff

Ruff's behavior is controlled by the `Ruff-config/ruff.toml` file. This template comes with a pre-configured file that sets sensible defaults for the project, including:

-   **Excluding common directories** from linting and formatting.
-   **Setting the line-length** to 100 characters.
-   **Targeting Python 3.9** for compatibility.
-   **Enabling a standard set of rules** for code quality.

You can customize the linting and formatting rules by editing this file. For a complete list of options, see the [official Ruff documentation](https://docs.astral.sh/ruff/configuration/).

## AI-Powered Development

This template includes a comprehensive collection of GitHub Copilot prompts designed to accelerate your Python development workflow. These prompts are located in the `.github/prompts/` directory and are automatically recognized by VS Code with GitHub Copilot.

### GitHub Copilot Integration

The project is pre-configured to work seamlessly with GitHub Copilot through VS Code settings:

- **Prompt Files**: Custom prompts are automatically detected from `.github/prompts/`
- **Chat Modes**: AI-powered development assistance
- **Instructions**: Context-aware coding guidelines

### Using Custom Prompts

To use the included prompts:

1. **Open VS Code** with the GitHub Copilot extension installed
2. **Select code** you want to work with
3. **Open GitHub Copilot Chat** (Ctrl+Shift+I / Cmd+Shift+I)
4. **Type `@workspace`** followed by the prompt name (e.g., `@workspace /write-pytest`)
5. **Execute the prompt** to generate code, tests, or reviews

### Available Prompts

The template includes the following ready-to-use prompts:

#### Code Generation
- **`asyncify-io.prompt.md`** - Convert synchronous I/O operations to async/await
- **`create-class.prompt.md`** - Generate Python classes with proper structure
- **`extract-function.prompt.md`** - Extract reusable functions from code blocks
- **`generate-dataclass.prompt.md`** - Create Python dataclasses with type hints
- **`generate-new-cli.prompt.md`** - Generate complete CLI applications
- **`quick-cli.prompt.md`** - Convert code selections into CLI scripts

#### Testing & Documentation
- **`docstringify.prompt.md`** - Generate comprehensive docstrings
- **`generate-pytest-tests.prompt.md`** - Create comprehensive test suites
- **`write-pytest.prompt.md`** - Generate pytest cases for selected code

#### Code Review & Quality
- **`lightweight-review.prompt.md`** - Quick code quality assessment
- **`review-code-general.prompt.md`** - Comprehensive code review
- **`review-code-performance.prompt.md`** - Performance-focused code analysis
- **`security-review.prompt.md`** - Security-centric code review

#### Refactoring & Optimization
- **`optimize-loop.prompt.md`** - Optimize loop performance
- **`refactor-add-error-handling.prompt.md`** - Add robust error handling
- **`refactor-readability.prompt.md`** - Improve code readability

#### Utilities
- **`explain-code-block.md`** - Get detailed code explanations

Each prompt is designed to work with code selections and provides context-aware suggestions following Python best practices.

## Project Structure

```
python-template/
├── .github/
│   ├── prompts/               # AI-powered development prompts
│   │   ├── asyncify-io.prompt.md
│   │   ├── create-class.prompt.md
│   │   ├── docstringify.prompt.md
│   │   ├── explain-code-block.md
│   │   ├── extract-function.prompt.md
│   │   ├── generate-dataclass.prompt.md
│   │   ├── generate-new-cli.prompt.md
│   │   ├── generate-pytest-tests.prompt.md
│   │   ├── lightweight-review.prompt.md
│   │   ├── optimize-loop.prompt.md
│   │   ├── quick-cli.prompt.md
│   │   ├── refactor-add-error-handling.prompt.md
│   │   ├── refactor-readability.prompt.md
│   │   ├── review-code-general.prompt.md
│   │   ├── review-code-performance.prompt.md
│   │   ├── security-review.prompt.md
│   │   └── write-pytest.prompt.md
│   ├── workflows/
│   │   └── ci-python.yml      # GitHub Actions CI/CD workflow
│   └── pull_request_template.md
├── .vscode/
│   └── settings.json          # VS Code workspace settings
├── junit/
│   └── test-results.xml       # JUnit test results (generated)
├── Ruff-config/
│   └── ruff.toml             # Ruff linting and formatting configuration
├── src/
│   ├── __init__.py
│   └── main.py               # Main application entry point
├── tests/
│   ├── __init__.py
│   ├── conftest.py           # pytest configuration
│   └── tests.py              # Test cases
├── .gitignore                # Git ignore rules
├── LICENSE                   # MIT License
├── README.md                 # This file
└── requirements.txt          # Python dependencies
```

## Contributing

Contributions are what make the open-source community such an amazing place to learn, inspire, and create. Any contributions you make are **greatly appreciated**.

If you have a suggestion that would make this better, please fork the repo and create a pull request. You can also simply open an issue with the tag "enhancement".

1.  Fork the Project
2.  Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3.  Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4.  Push to the Branch (`git push origin feature/AmazingFeature`)
5.  Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
