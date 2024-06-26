# Simple GitHub API Library

This library provides a straightforward interface to interact with the
GitHub API, allowing users to fetch repository information, download files,
and more.

## Features

- Fetch repository trees and blobs
- Download repository contents
- Support for parallel downloads using threading
- Automatic rate limit handling
- Environment variable and secrets support for authentication

## Installation

To install the library, you can use pip:

```bash
pip install git+https://github.com/mkpro118/gh-api.git
```

## Usage

### Basic usage

```python
from gh_api.repository import Repository

# Get all repositories for a user
user: str = 'example-user'
repos: list[Repository] = Repository.get_repositories(user=user)

# Use the first repository as an example
repo = repos[0]

# Get repository tree
tree: list[dict[str, str]] = repo.get_tree(
    ref_or_sha='main',  # Retrieves from the main branch
    recursive=True
)

# Get repository blobs
blobs = repo.get_blobs()

# Download repository contents
repo.download_blobs(
    root="./folder",  # Root directory to place downloaded files in,
                      # In this example, a file 'example.txt' located at the
                      # repository root will be placed at
                      # ./folder/example-user/example-repo/example.txt
    makedirs=True,    # Make any necessary directories
                      # Including this option is usually a good idea
    overwrite=False,  # Do not overwrite existing files
    use_threads=True  # Download in parallel, can be an int to specify the
                      # number of worker threads
)
```

### Authentication

The library uses environment variables for authentication.
Set the `GITHUB_PAT` environment variable with your GitHub Personal Access Token

```bash
export GITHUB_PAT=your_personal_access_token
```

Alternatively, you can use a `.env` file.

Authentication is supported in Docker containers using Docker secrets,
this requires the secret be name `gh_secret`

## Dependencies

- requests
- python-dotenv

## Contributing

Contributions are welcome! Please feel free to submit a [Pull Request](https://github.com/mkpro118/gh-api/pulls).

## License

This project is licensed under the MIT License.
