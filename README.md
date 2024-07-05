# PROJECT TO OCI

## Step 1: Assessment and Planning

### Inventory of Current Resources:
- **Database Server:** Identify the type of database and version.
- **Application Server:** Identify the application stack and dependencies.
- **Domain Configuration:** Verify the domain configuration and DNS settings.
- **Performance Metrics:** Collect performance metrics from the current servers to understand the load and identify bottlenecks.

## Step 2: Design the New Architecture

### Configure OCI Tenancy:
- Configure the OCI account to migrate this application. You can start with a free account with credits and convert it to a pay-as-you-go account after the POC.
- Configure Identity Domain to manage users, policies, permissions, etc.
- Create a compartment specifically for this project for better cost management and segregation by tag and by compartment.

### Overview:
- **Database:** Use Oracle Autonomous Database or Oracle MySQL/PostgreSQL Database Service for a managed database solution or instance with database services (according to the previous Assessment and Planning).
- **Application:** Use Oracle Container Engine for Kubernetes (OKE) to run the application in pods. Using pods allows for scaling based on application usage (memory and CPU consumption per pod).
- **Load Balancing:** Use OCI Load Balancer to distribute traffic across multiple instances.
- **Domain Management:** Use OCI DNS for DNS management.

### High-Level Architecture Diagram:
1. Internet (User)
2. DNS (OCI DNS)
3. Load Balancer (OCI Load Balancer)
4. OKE (Oracle Container Engine for Kubernetes)
5. Database (Oracle Autonomous Database / Oracle MySQL/PostgreSQL Database Service / Instance with database services)
6. Configure backup routines

## Step 3: Setup the Cloud Environment

- **Create a VCN (Virtual Cloud Network):** Create subnets, route tables, and security lists.
- **Create the Database:** Create a database based on the possibilities found during the overview. Configure security lists to allow access from the application server to enable communication between servers (database and application).
- **Setup an OKE cluster:** Define deployment and service configurations for the application pods.
- **Load Balancer:** During the creation of the application on Kubernetes, you can create a Load Balancer using YAML code for better management or create a Load Balancer separately and manually configure the IP and Port on the backend from the Load Balancer.
- **Configure DNS:** Create a DNS zone for the domain “topsurvey.contoso.com”. Point the domain to the Load Balancer.

## Step 4: Migrate the Workloads

### Database Migration:
- Export the database from the current server.
- Import the database into the Autonomous Database or MySQL/PostgreSQL Database Service or Instance with database services.

### Application Migration:
- Containerize the application if not already done.
- Push the container image to Oracle Cloud Infrastructure Registry (OCIR).
- Deploy the container to the OKE cluster.

## Step 5: Optimize and Test

### Performance Testing:
- Perform load testing to ensure the new setup can handle the expected traffic.

### Monitoring and Logging:
- Set up OCI Monitoring and Logging services.
- Configure alarms for critical metrics.

### Auto Scaling:
- Configure auto-scaling for OKE and the database to handle variable loads.

## Step 6: Configure Backup Routines

### Possibilities of Backup:
- **Oracle Autonomous Database:** Automatically backs up your database to Object Storage, also perform manual backups if needed.
- **MySQL/PostgreSQL Database Service:** Can configure automatic backups and perform manual backups.
- **Instance with Database Services:** Use scripts to run backup and send the files to an Object Storage and configure lifecycle to these objects.
- **PS:** We can also configure backup to the boot volume if using an Instance.

## Step 7: Cutover and Go Live

- **DNS Cutover:** Update the DNS records to point to the new infrastructure.
- **Final Testing:** Perform final testing to ensure everything is working as expected.
- **Decommission Old Servers:** Once the new setup is confirmed to be stable, decommission the old servers.

## Additional Information

This is an explanation of this environment, but if there is more budget to create the workloads, we can improve security with a Firewall, so all traffic passes through the firewall first and redirects to a private load balancer and some service like NGINX to distribute the packages. Also, if the application must be public, we can use WAF to protect all calls to the URL, blocking by region, type of calls, robot traffic, etc.

- Configure permissions according to the principle of least privilege.
- Use guardrails to guarantee security for this environment, some of which are already mentioned above.
- To automate this environment, we should use Terraform/Ansible to improve agility and management of changes.

I already did the same project for AWS and OCI.
