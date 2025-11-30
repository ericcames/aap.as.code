# Development Guide

This project is designed to be used with the Ansible Automation Platform (AAP). The setup is primarily done through the AAP user interface.

## Prerequisites

- Credentials for Automation Hub (certified and validated).
- Ansible Galaxy/Automation Hub API Token.
- A vault credential.
- A public SSH key.

## Setup

The `README.md` describes a manual setup process within the Ansible Automation Platform:

1.  Create credentials for Automation Hub.
2.  Create a vault credential.
3.  Create a project pointing to this git repository.
4.  Create a job template that uses the `main.yml` playbook.

## Configuration

- The project uses a remote vault for secrets.
- Extra variables are required for the job template, as specified in the `README.md`.

## Running the project

The project is run by launching the job template from the Ansible Automation Platform.
