# What is Continuous Delivery (CD)

**Continuous Delivery (CD)** is a software development practice where code changes are automatically built, tested, and prepared for release to production  so that software can be deployed at any time with minimal manual effort.

## How Continuous Delivery Works

1. **Developers commit code** to a shared repository.
2. The system automatically:
    - Builds the application
    - Runs automated tests
    - Validates quality checks
3. If everything passes, the system prepares the application for release.
4. A human can deploy it to production with a simple approval (often just a button click).

Deployment could also mean:

- Server Provisioning
- Dependencies
- Conf Changes
- Network/ Firewall rule changes
- Artifact Deploy

**Automation Tools:**

- Ansible, Puppet, Chef (System Automation)
- Terraform, CFormation (Cloud Info Automation)
- Jenkins, Octopus Deploy (CI/CD Automation)
- HELM Charts
- Code Deploy

 **Software Testing** must be automated as well, these test processes could be:

- Functional
- Load
- Performance
- DataBase
- Network/Security

