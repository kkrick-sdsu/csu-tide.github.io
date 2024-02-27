# CSU TIDE Docs
This repo contains the CSU TIDE Technical documentation.
For general information about TIDE see the [official TIDE website](https://tide.sdsu.edu).

This is a [GitHub Pages](https://pages.github.com/) site built with [Jekyll](https://jekyllrb.com/) and assumes that editors are familiar with both, as well as with [Markdown](https://www.markdownguide.org/).

## Local Development
This website can be built and run locally with Docker for Desktop, Visual Studio Code and the Dev Containers extension.
For more information on this workflow and setup, see the [official docs on dev containers](https://code.visualstudio.com/docs/devcontainers/containers).

You can then open the command palette (Windows: Ctrl + Shift + P; Mac: ⌘ + ⇧ + P ) and select "Reopen in container."
This will open the repo in its provided dev container which has Jekyll and its dependencies pre-installed.

Alternatively, you must have Jekyll and its dependencies installed on your local machine.

## Building the Website Locally
1. Open your terminal in the repo's root directory
  - In VS Code this can be done with Ctrl + `
1. Run `bundle install`
1. Run `bundle update`
1. Run `bundle exec jekyll serve`
1. Open your browser and navigate to localhost:4000
