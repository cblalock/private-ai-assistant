# ollama-ansible

Ansible playbook that provisions a fully working private AI assistant from scratch with a single command. Built around a real-world scenario: a business that needs an internal AI tool but cannot send sensitive client data to third-party servers like OpenAI or Anthropic.

## What it deploys

# ollama-ansible

Ansible playbook that provisions a fully working private AI assistant from scratch with a single command. Built around a real-world scenario: a business that needs an internal AI tool but cannot send sensitive client data to third-party servers like OpenAI or Anthropic.

## What it deploys

- **Ollama** — runs the LLM locally as a background service
- **llama3.2:3b** — a capable open-source model that runs on modest hardware
- **Open WebUI** — a polished ChatGPT-like browser interface for non-technical users
- **Systemd + Docker** — everything runs as a service and survives reboots

## Real world use case

Built around the scenario of a day spa (Serenity Spa & Wellness) that wants an internal AI assistant for staff — answering questions about treatments, health contraindications, and client communication — without confidential client health data ever leaving their network.

The same pattern applies to any privacy-sensitive industry: healthcare, legal, finance, or any company with proprietary internal data.

## Requirements

**Your local machine:**
- Ansible installed → `pip install ansible` or `sudo apt install ansible-core`

**Target machine:**
- Ubuntu/Debian Linux
- SSH access with sudo privileges
- 8GB+ RAM recommended (16GB for comfort)

## Usage

**1. Clone the repo**
```bash
git clone https://github.com/cblalock/ollama-ansible.git
cd ollama-ansible
```

**2. Edit `inventory.ini`**
```ini
# Remote server
my_server ansible_host=YOUR_SERVER_IP ansible_user=YOUR_USERNAME

# Or run locally on your own machine
my_server ansible_host=127.0.0.1 ansible_connection=local ansible_user=YOUR_USERNAME
```

**3. Run the playbook**
```bash
ansible-playbook -i inventory.ini playbook.yml --ask-become-pass
```

That's it. Once it finishes, open a browser and go to:
http://localhost:3000

## Changing the model

Edit the `ollama_model` variable in `playbook.yml`:
```yaml
vars:
  ollama_model: "llama3.2:3b"  # swap for mistral, llama3.2, deepseek-coder, etc.
```

Browse all available models at [ollama.com/library](https://ollama.com/library)

## Starting and stopping

This is designed for a always-on server. If running locally via WSL2:

```bash
# Start
sudo systemctl start ollama
sudo docker start open-webui

# Stop
sudo systemctl stop ollama
sudo docker stop open-webui

# Check status
sudo systemctl status ollama
sudo docker ps
```

## Project structure
ollama-ansible/
├── inventory.ini   # Defines which server(s) to deploy to
├── playbook.yml    # All automation steps
└── README.md

## Tech stack

- Ansible
- Ollama
- Docker
- Open WebUI
- llama3.2:3b (Meta)
- Ubuntu/systemd
