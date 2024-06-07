## DevOps Automation: A Secure CI/CD Pipeline for Containerized Workload


import matplotlib.pyplot as plt
import matplotlib.patches as mpatches

# Create a new figure
fig, ax = plt.subplots(figsize=(15, 10))
ax.axis('off')

# Colors
box_color = "#A3C1DA"
arrow_color = "#0056A4"

# Add boxes for each component
components = {
    "GitHub": (1, 8),
    "Jenkins": (4, 8),
    "Maven": (4, 6),
    "JUnit": (4, 5),
    "SonarQube": (4, 4),
    "JFrog Artifactory": (4, 3),
    "Docker": (4, 2),
    "Trivy": (4, 1),
    "AWS S3": (7, 3),
    "Docker Hub": (7, 2),
    "EKS": (10, 8),
    "ArgoCD": (10, 6),
    "Prometheus": (10, 4),
    "Grafana": (10, 3),
    "Slack": (10, 2),
    "HashiCorp Vault": (7, 6)
}

for component, (x, y) in components.items():
    rect = mpatches.FancyBboxPatch((x, y), 2, 1, boxstyle="round,pad=0.3", edgecolor=arrow_color, facecolor=box_color)
    ax.add_patch(rect)
    ax.text(x + 1, y + 0.5, component, ha='center', va='center', fontsize=10, color='black')

# Add arrows between components
arrows = [
    ("GitHub", "Jenkins"),
    ("Jenkins", "Maven"),
    ("Maven", "JUnit"),
    ("JUnit", "SonarQube"),
    ("SonarQube", "JFrog Artifactory"),
    ("JFrog Artifactory", "Docker"),
    ("Docker", "Trivy"),
    ("Trivy", "AWS S3"),
    ("Trivy", "Docker Hub"),
    ("AWS S3", "EKS"),
    ("Docker Hub", "EKS"),
    ("EKS", "ArgoCD"),
    ("ArgoCD", "Prometheus"),
    ("Prometheus", "Grafana"),
    ("Jenkins", "Slack"),
    ("Prometheus", "Slack"),
    ("HashiCorp Vault", "Jenkins"),
    ("HashiCorp Vault", "Docker")
]

for start, end in arrows:
    start_x, start_y = components[start]
    end_x, end_y = components[end]
    ax.annotate("", xy=(end_x + 1, end_y), xytext=(start_x + 1, start_y + 1), 
                arrowprops=dict(arrowstyle="->", color=arrow_color, lw=2))

# Add labels
ax.text(5, 9, "Secure CI/CD Pipeline Architecture", fontsize=16, ha='center', va='center', color=arrow_color, weight='bold')

# Show the plot
plt.show()
