# DevOps & Automation Lab (ENSP461)

**Name:** Kazi Shabnam Ali
**Roll No:** 2301420047

## Repository Structure

    devops-lab-ENSP461/
    ├── README.md
    ├── .gitignore
    ├── pdfs/
    │   ├── Q1_Configure_Infrastructure_Ansible.pdf
    │   ├── Q2_Provision_Scalable_Infrastructure_Terraform.pdf
    │   └── lab3/            (Lab 3 reports)
    ├── ansible/
    └── terraform/

---

## Part A: Lab 3 - Monitoring, Logging & DevSecOps

| # | Topic | Report |
|---|-------|--------|
| 1 | Set up monitoring using Prometheus and Grafana | [PDF](pdfs/lab3/lab3_prometheus_pdf_Q1_Set_Up_Monitoring_Prometheus_Grafana_Completed_Updated.pdf) |
| 2 | Implement centralized logging (ELK stack) | [PDF](pdfs/lab3/lab3_pdfs_Q1_Centralized_Logging_ELK_Completed_Updated.pdf) |
| 3 | Integrate security into the CI/CD pipeline (DevSecOps) | [PDF](pdfs/lab3/lab3_pdfs_Q2_Integrate_Security_CICD_DevSecOps_Completed_Updated.pdf) |
| 4 | Implement feature flags and perform different testing | [PDF](pdfs/lab3/lab3_pdfs_Q3_Feature_Flags_And_Testing_Completed_Updated.pdf) |
| 5 | Prepare a monitoring, logging and DevSecOps report | [PDF](pdfs/lab3/lab3_pdfs_Q4_Monitoring_Logging_DevSecOps_Report_Completed_Updated.pdf) |

### 1. Monitoring with Prometheus and Grafana
Installed Prometheus and Node Exporter on Ubuntu, configured scrape targets
(`localhost:9090`, `localhost:9100`) with a 15s interval, and connected Prometheus as a
Grafana data source. Imported dashboard ID 1860 (Node Exporter Full) to visualize CPU,
memory, uptime and disk usage.

    sudo apt install -y prometheus prometheus-node-exporter grafana
    sudo systemctl enable --now prometheus prometheus-node-exporter grafana-server

### 2. Centralized Logging with ELK
Configured Elasticsearch, Logstash and Kibana. Logstash reads
`/var/log/webapp/access.log`, parses fields with a Grok filter
(timestamp, level, msg) and indexes them into `webapp-logs-YYYY.MM.dd`. A Kibana index
pattern (`webapp-logs-*`) shows event counts by severity (INFO, WARN, ERROR).

    sudo systemctl enable --now elasticsearch logstash kibana
    curl -s http://localhost:9200

### 3. DevSecOps: Security in the CI/CD Pipeline
Added automated security gates to a Jenkins pipeline: Bandit for static analysis, and
Trivy for dependency, secret and Dockerfile scanning. The pipeline fails
(`--exit-code 1`) on any High/Critical finding, which blocks deployment.

    bandit -r app/ -f txt
    trivy fs --severity HIGH,CRITICAL --exit-code 1 .
    trivy fs --scanners secret --exit-code 1 .

### 4. Feature Flags and Testing
Implemented runtime feature flags using a `flags.json` file, so features can be toggled
without redeploying. Load-tested the checkout endpoint with ApacheBench and validated
the UI with headless Selenium tests run through pytest.

    ab -n 500 -c 25 http://localhost:5000/checkout
    pytest test_selenium_ui.py -v

### 5. Monitoring, Logging and DevSecOps Report
Summary report combining the observability setup (ELK), the security gates (Bandit and
Trivy), feature flag governance, and the load and UI testing results.

---

## Part B: Infrastructure Automation

| # | Topic | Report | Source |
|---|-------|--------|--------|
| 1 | Configure infrastructure and automate application (Ansible) | [PDF](pdfs/Q1_Configure_Infrastructure_Ansible.pdf) | [ansible/](ansible/) |
| 2 | Provision scalable infrastructure (Terraform) | [PDF](pdfs/Q2_Provision_Scalable_Infrastructure_Terraform.pdf) | [terraform/](terraform/) |

### 1. Ansible
Installs and configures Nginx on target nodes using an inventory, a playbook, a Jinja2
template and a handler. Running the playbook a second time reports `changed=0`,
which confirms it is idempotent.

    cd ansible
    ansible -i inventory.ini webservers -m ping
    ansible-playbook -i inventory.ini site.yml

### 2. Terraform
Provisions a scalable web tier on AWS (security group, launch template and Auto Scaling
Group) using the init, plan and apply workflow. Capacity is scaled by changing the
`desired_capacity` variable.

    cd terraform
    terraform init
    terraform plan -out=tfplan
    terraform apply tfplan
    terraform destroy -auto-approve   # clean up

---

## Notes

- No credentials or keys are stored in this repository.
- Terraform state files are excluded via `.gitignore`.
