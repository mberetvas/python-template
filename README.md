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
- [Contributing](#contributing)
- [License](#license)

## Features

-   **Modern Tooling**: Utilizes `uv` for fast package management.
-   **Organized Structure**: A clean `src/` layout for source code and `tests/` for tests.
-   **Testing Ready**: Comes with `pytest` configured for running tests.
-   **Code Quality**: Integrated with `ruff` for linting and formatting to maintain high code standards.
-   **CI/CD Ready**: Includes a basic GitHub Actions workflow for continuous integration.
-   **Version Control**: Pre-configured `.gitignore` file.

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

This template uses `pytest` for testing.
```sh
# Install pytest if you haven't already
uv add --dev pytest

# Run tests
# don't forget to activate the venv first !
pytest
```

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
