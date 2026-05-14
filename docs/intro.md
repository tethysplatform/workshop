---
sidebar_position: 1
title: Introduction
---

# Tethys Platform Workshop

Welcome! This workshop is a hands-on introduction to building geoscientific web applications and dashboards with the [Tethys Platform](https://docs.tethysplatform.org/) and [TethysDash](https://tethysdash.readthedocs.io/). Over the course of four tutorials you will build a traditional Tethys app, a modern component-based app, an interactive TethysDash dashboard, and a custom TethysDash plugin — all using real hydrologic and geophysical data.

## What You Will Build

| # | Tutorial | What you build | Where it runs |
|---|---|---|---|
| 1 | [Getting Started](./workshop/getting-started) | An **Earthquake Calculator** Tethys app with forms, interactive maps, and saved measurement results | Local Tethys server |
| 2 | [Component Applications](./workshop/component-applications) | A **Well Drawdown Calculator** Component App driven by reactive state, sliders, and live plots | Local Tethys server |
| 3 | [TethysDash: Creating Dashboards](./workshop/tethysdash-creating-dashboards) | A **GEOGLOWS Demo** dashboard showing global water-model river flowlines for China | Hosted on the TGF Portal — no local install required |
| 4 | [TethysDash: Creating Custom Plugins](./workshop/tethysdash-creating-custom-plugins) | Extend the GEOGLOWS dashboard with a custom forecast-plot plugin tied to map clicks | Local TethysDash install |

The tutorials are designed to be completed in order, but each one is self-contained — feel free to jump to a specific topic if you already know the rest. A reference solution is provided at the end of every tutorial in case you get stuck.

## Prerequisites

Before starting the workshop you should have:

- Working knowledge of **Python** (functions, imports, classes, virtual environments).
- A **terminal** you are comfortable using (`bash`, `zsh`, PowerShell, etc.).
- A modern **web browser** (Chrome, Firefox, Edge, or Safari).
- An **internet connection** — several tutorials pull from live data services.
- A **code editor** of your choice (VS Code, PyCharm, etc.).

You do **not** need prior experience with Tethys Platform, Django, React, or web development. The tutorials introduce everything you need as you go.

## Environment Setup

You will use a single Python virtual environment for all four tutorials. The steps below follow the official [Tethys virtual environment](https://docs.tethysplatform.org/en/latest/supplementary/virtual_environment.html) and [Tethys quickstart](https://docs.tethysplatform.org/en/latest/) instructions, using Python's built-in [`venv`](https://docs.python.org/3/library/venv.html) module and `pip`.

### Step 1 — Install Python

Download and install **Python 3.10 or newer** from [python.org/downloads](https://www.python.org/downloads/) (skip if you already have a recent Python). After installation, open a **new** terminal and verify:

```bash
python --version
```

You should see `Python 3.10.x` or higher. If `python` is not recognized, try `python3 --version` — on some systems the executable is named `python3`. Substitute `python3` for `python` in the rest of these steps if that is the case.

:::tip
On Windows, make sure to check **"Add Python to PATH"** during installation so the `python` command is available in any terminal.
:::

### Step 2 — Create a workshop directory and virtual environment

Create a directory to hold your workshop work, change into it, and create a virtual environment named `tethys` inside:

**Linux / macOS:**

```bash
mkdir tethys-workshop
cd tethys-workshop
python -m venv ./tethys
```

**Windows:**

```powershell
mkdir tethys-workshop
cd tethys-workshop
python -m venv .\tethys
```

This creates an isolated Python environment in a new `tethys/` folder inside `tethys-workshop/`. None of the packages you install here will affect your system Python.

### Step 3 — Activate the virtual environment

The activation command depends on your operating system:

**Linux / macOS:**

```bash
source ./tethys/bin/activate
```

**Windows:**

```powershell
.\tethys\Scripts\activate
```

After activation you should see `(tethys)` prepended to your terminal prompt. **Every subsequent command in the workshop assumes this environment is active.** If you open a new terminal, re-run the activation command above before continuing.

To leave the environment at any time, run `deactivate`.

:::tip
If PowerShell blocks the activation script with an execution-policy error, run `Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned` once and try again.
:::

### Step 4 — Install Tethys Platform

With the environment active, upgrade `pip` and install Tethys Platform:

```bash
pip install --upgrade pip
pip install tethys-platform
```

Each tutorial will tell you which additional packages it needs and walk you through installing them at the appropriate step.

:::tip
The full [Tethys Platform installation docs](https://docs.tethysplatform.org/en/latest/installation.html) cover advanced setups (conda, Docker, PostgreSQL, production deployments) if you need them. The `pip` install above is sufficient for this workshop.
:::

### Step 5 — Run `tethys quickstart`

Tethys ships with a single command that configures the local database, starts the development server, and opens the Tethys Portal in your browser:

```bash
tethys quickstart
```

When it is done, your browser should open [http://127.0.0.1:8000](http://127.0.0.1:8000) automatically. Log in with the default credentials:

- **Username:** `admin`
- **Password:** `pass`

If you see the Tethys Portal home page, your environment is ready. Press `Ctrl+C` in the terminal to stop the server when you are done — you will start it again in Tutorial 1.

:::tip
The [Tethys quickstart guide](https://docs.tethysplatform.org/en/latest/) explains exactly what `tethys quickstart` does under the hood (database setup, default user creation, server launch) in case anything goes wrong.
:::

## Ready to Begin

You are all set! Head over to [**Tutorial 1: Getting Started**](./workshop/getting-started) to scaffold your first Tethys app.

:::tip
Tutorial 3 runs entirely in the browser on the hosted [TGF Portal](https://demo.tethysgeoscience.org/) and does **not** require your local environment. If you only plan to do Tutorial 3, you can skip the setup above — just create a TGF Portal account when you reach that tutorial.
:::

## Troubleshooting

- **`python` not found** — Try `python3` instead, or reinstall Python and make sure it is added to your `PATH`.
- **PowerShell blocks the activation script** — Run `Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned` once, then retry the activation.
- **`pip install` fails to build a native dependency (GDAL, etc.)** — Make sure you are using Python 3.10+ on a 64-bit OS. If the issue persists, see the [Tethys Platform installation docs](https://docs.tethysplatform.org/en/latest/installation.html) for a conda-based install path that avoids building from source.
- **`tethys` command not found after activation** — Confirm the virtual environment is active (your prompt should start with `(tethys)`) and that `pip install tethys-platform` finished without errors.
- **`tethys quickstart` fails** — See the [Detailed Installation guide](https://docs.tethysplatform.org/en/latest/installation.html) for what quickstart does under the hood; you can run the individual steps manually if needed.
- **Port 8000 already in use** — Stop any other process using that port, or start Tethys on a different port with `tethys start -p 8001`.
- **Anything else** — The [Tethys Platform documentation](https://docs.tethysplatform.org/) and [TethysDash documentation](https://tethysdash.readthedocs.io/) are the authoritative references for installation and runtime issues.
