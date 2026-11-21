# DevOps & Automation Lab (ENSP461)

**Name:** Kazi Shabnam Ali
**Roll No:** 2301420047

## Contents

| Question | Topic | Report | Source |
|----------|-------|--------|--------|
| 1 | Configure infrastructure and automate application (Ansible) | [PDF](pdfs/Q1_Configure_Infrastructure_Ansible.pdf) | [ansible/](ansible/) |
| 2 | Provision scalable infrastructure (Terraform) | [PDF](pdfs/Q2_Provision_Scalable_Infrastructure_Terraform.pdf) | [terraform/](terraform/) |

## Q1: Ansible

Installs and configures Nginx on target nodes using a playbook, a Jinja2 template and a handler.

    cd ansible
    ansible -i inventory.ini webservers -m ping
    ansible-playbook -i inventory.ini site.yml

## Q2: Terraform

Provisions a security group, launch template and Auto Scaling Group on AWS.

    cd terraform
    terraform init
    terraform plan -out=tfplan
    terraform apply tfplan
    terraform destroy -auto-approve   # clean up

## Notes

- No credentials or keys are stored in this repository.
- Terraform state files are excluded via `.gitignore`.
