# UV Setup Guide

This guide provides step-by-step commands to install UV and initialize it.

---

## Table of Contents

- [Step 1: Install UV](#step-1-install-uv)
- [Step 2: Initialize UV](#step-2-initialize-uv)

---

## Step 1: Install UV

UV provides a standalone installer to download and install uv:

  - On macOS and Linux:
  
    Use `curl` to download the script and execute it with `sh`:
    
    ```bash
    curl -LsSf https://astral.sh/uv/install.sh | sh
    ```
    
    If your system doesn't have `curl`, you can use `wget`:
    
    ```bash
    wget -qO- https://astral.sh/uv/install.sh | sh
    ```
    
    Request a specific version by including it in the URL:
    
    ```bash
    curl -LsSf https://astral.sh/uv/0.10.3/install.sh | sh
    ```


  - On Ubuntu/Debian:

    Use irm to download the script and execute it with `iex`:
     
    ```bash
    PS > powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
    ```
    
    Changing the execution policy allows running a script from the internet.
    Request a specific version by including it in the URL:
      
    ```bash
    PS > powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/0.10.3/install.ps1 | iex"
    ```
      
---

## Step 2: Initialize UV

UV manages project dependencies and environments, with support for lockfiles, workspaces, and more, similar to rye or poetry:
  ```bash
  $ uv init example
  Initialized project `example` at `/home/user/example`
  
  $ cd example
  
  $ uv add ruff
  Creating virtual environment at: .venv
  Resolved 2 packages in 170ms
     Built example @ file:///home/user/example
  Prepared 2 packages in 627ms
  Installed 2 packages in 1ms
   + example==0.1.0 (from file:///home/user/example)
   + ruff==0.5.0
  
  $ uv run ruff check
  All checks passed!
  
  $ uv lock
  Resolved 2 packages in 0.33ms
  
  $ uv sync
  Resolved 2 packages in 0.70ms
  Audited 1 package in 0.02ms```
  ```
