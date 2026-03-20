# Lab 2 – Supply Chain Security & Network Policies

Jag har slutfört labben och verifierat alla steg:

- SBOM genererad: `sboms/vulnerable-app.spdx.json`
- Cosign key-based signering utförd och verifierad
- NetworkPolicies testade:
  - Frontend → Backend fungerar
  - Frontend → Mongo blockeras
- Sårbar och härdad Dockerfile, Trivy-scan klar
- Reflektion / dokumentation inkluderad

Repo: [https://github.com/Sebastianilia7/lab2-container-security/tree/main/app](https://github.com/Sebastianilia7/lab2-container-security/tree/main/app)
