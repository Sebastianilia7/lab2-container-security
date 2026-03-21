## Lab 2: Container Security ##

Vad jag gjort
	•	Vulnerable image: node:18-alpine + npm deps → många CVEs, ~58MB
	•	Hardened image: node:18-alpine + non-root user + pinned deps → färre CVEs, ~45MB
	•	Trivy scanning: scan-before.txt vs scan-after.txt
	•	SBOM: CycloneDX-format (sboms/hardened-app.sbom.json) för supply chain-transparens
	•	Cosign: Signed & verified localhost:5001/lab2-hardened-app:latest
	•	Gatekeeper: Policies i chas-ff namespace testade:
	•	Bad Pod → DENIED
	•	Hardened Pod → ALLOWED (med warning om default SA)

Verktyg
	•	Trivy: Vulnerability scanning & SBOM generation
	•	Docker: Container builds & tagging
	•	Cosign: Signing & verification av images
	•	Kubernetes + Gatekeeper/OPA: Policy enforcement, dry-run tests av pods

Reflektion

Container-säkerhet: Jag lärde mig att minimera attackytan är kritiskt. Att köra non-root hindrar privilege escalation, och att använda pinned deps i en slim-baserad image minskar antalet sårbarheter dramatiskt. Trivy visar konkret hur riskerna minskar – skillnaden mellan lab2-vulnerable-app och lab2-hardened-app är tydlig.

SBOM och signering: CycloneDX-SBOM ger full transparens i supply chain och listar alla komponenter (OS + npm deps), vilket gör CVE-hantering snabbare. Cosign-signering garanterar att ingen obehörig ändrat image, vilket stärker säkerheten i CI/CD-pipelines.

Gatekeeper-policys: Policy-as-code nekar automatiskt osäkra pods (“Bad Pod”), kräver labels, non-root och resource limits. Hardened Pod gick igenom med en varning om default service account. Detta flyttar säkerheten “left” i DevOps och gör DevSecOps praktiskt och repetitivt.

### Screenshots
Bad pod
<img width="1410" height="706" alt="labb2bild" src="https://github.com/user-attachments/assets/5cc42a6f-9c51-40d0-b616-211d8d573a5c" />


Hardend Pod
<img width="1343" height="552" alt="labb2" src="https://github.com/user-attachments/assets/44393add-f942-464b-8520-69bcbde31651" />
