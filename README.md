# Configuring_Up-Time_Monitoring_Using_Gatus

## Task 1 — Install and Set Up Gatus Locally
1. Install Docker

If Docker isn’t already installed:

Visit Docker’s official installation page
Follow the instructions for your operating system (Windows, macOS, or Linux)

Verify installation: docker --version

2. Pull the Gatus Docker Image

Run the following command to pull the latest version of Gatus:docker pull twinproduction/gatus

3. Create the Gatus Configuration File

Create a new folder (for example, gatus-setup) and inside it, create a file called config.yaml.
config.yaml Example:
"endpoints:
  - name: Example Website
    url: "https://example.com"
    interval: 60s
    conditions:
      - "[STATUS] == 200"

  - name: GitHub
    url: "https://github.com"
    interval: 60s
    conditions:
      - "[STATUS] == 200"

  - name: Nonexistent
    url: "https://thiswebsitedoesnotexist.com"
    interval: 60s
    conditions:
      - "[STATUS] == 200"

alerts:
  - type: slack
    url: "https://hooks.slack.com/services/YOUR/WEBHOOK/URL"
    failure-threshold: 2
    success-threshold: 2

## Explanation:

endpoints: List of websites to monitor.

interval: How often (in seconds) Gatus checks the endpoint.

conditions: Conditions that must be met (e.g., status code 200).

alerts: Configures Slack alerts for failures.

4. Run Gatus Using Docker
From your project directory (gatus-setup), run:
docker run -d \
  -p 8080:8080 \
  -v $(pwd)/config.yaml:/config/config.yaml \
  twinproduction/gatus

This runs Gatus in a Docker container and exposes it on port 8080.

5. Test the Setup

Open your browser and go to:http://localhost:8080

You should see the Gatus dashboard, showing the three endpoints:

- Example Website – should be healthy

- GitHub – should be healthy

- Nonexistent – should fail (simulated downtime)

## Task 5 — Explore and Customize the Gatus Dashboard
1. View Uptime Statistics

Open the Gatus dashboard (http://localhost:8080

You’ll see color-coded boxes:

- Green = healthy

- Red = failed

2. Customize Dashboard Appearance

You can adjust Gatus appearance in the configuration file. For example:
ui:
  title: "My Local Gatus Monitor"
  theme: dark
  logo: "https://your-logo-url.com/logo.png"
"ui:
  title: "My Local Gatus Monitor"
  theme: dark
  logo: "https://your-logo-url.com/logo.png""

  Add the above under the root of your config.yaml.

  3. Adjust Monitoring Intervals and Conditions

For example:
"  - name: Example Website
    url: "https://example.com"
    interval: 30s
    conditions:
      - "[STATUS] == 200"
      - "[RESPONSE_TIME] < 500""

This checks every 30 seconds and ensures the site responds in under 500ms.

## Quick Summary
Step	Action	Command/File
1	Install Docker	docker --version
2	Pull Gatus image	docker pull twinproduction/gatus
3	Create config file	config.yaml
4	Run container	docker run -d -p 8080:8080 -v $(pwd)/config.yaml:/config/config.yaml twinproduction/gatus
5	Open dashboard	http://localhost:8080
6	Customize dashboard	Add ui: section to config