CloudBees Multibranch CI Pipeline (Spring Boot – Maven Only)

This repository demonstrates a CloudBees Multibranch Pipeline implemented using the Pipeline Template Catalog.
It is designed to validate branch discovery, PR builds, merge-to-main builds, and Maven caching with a minimal Spring Boot project.

This setup is intentionally simple and CI-focused.

🚀 What This Pipeline Does

The pipeline automatically triggers builds for:

Feature branches

Pull Requests (PRs)

Main branch after PR merge

It currently runs only Maven build stages to keep testing fast and deterministic.

🔧 Technologies Used

CloudBees CI

Jenkins Multibranch Pipeline

Pipeline Template Catalog

GitHub

Java 17

Maven

🧩 Pipeline Architecture
Pipeline Template Catalog
        ↓
Multibranch Pipeline Job
        ↓
Branch / PR Discovery
        ↓
Single Jenkinsfile (branch-aware)


The template controls branch discovery

The Jenkinsfile controls pipeline logic

One Jenkinsfile runs for all branches

🔀 Branching & Build Behavior
Branch Types Supported
Branch Type	Example	Pipeline Trigger
Main	main	✅ Yes
Feature	feature-login	✅ Yes
Fix	fix-bug	✅ Yes
Pull Request	PR-12	✅ Yes
Other	dev, test	❌ No

Branch filtering is handled by the Pipeline Template Catalog, not in the Jenkinsfile.

🔁 CI Flow (End-to-End)
1️⃣ Feature branch push
git checkout -b feature-demo
git commit -m "Test feature"
git push origin feature-demo


➡️ Pipeline runs for feature-demo

2️⃣ Pull Request opened (feature → main)

Jenkins creates a virtual branch: PR-<id>

Pipeline runs for the PR

➡️ CI validation only (no deploy)

3️⃣ Pull Request merged into main

GitHub creates a new commit on main

Multibranch pipeline detects the change

Main branch pipeline runs automatically

➡️ Confirms merge-to-main behavior

🧱 Pipeline Stages (Current Test Setup)

For testing, the pipeline runs only these stages:

Restore Maven Cache
↓
Maven Build (skip tests)
↓
Save Maven Cache

Why only these stages?

Fast execution

Easy debugging

Validates cache + build mechanics

No external dependencies

🧪 Jenkinsfile Logic (Summary)

Uses checkout scm (multibranch-safe)

Skips builds triggered only by branch indexing

Same Jenkinsfile works for:

feature branches

PR branches

main branch

Maven dependencies are cached across builds

📦 Maven Cache Behavior

Maven local repository stored in workspace cache

First build downloads dependencies

Subsequent builds reuse cache

Cache shared across branches and PRs

This dramatically improves build performance.

📁 Repository Structure
.
├── Jenkinsfile
├── pom.xml
└── src/
    └── main/
        └── java/
            └── com/example/demo/

🎯 Purpose of This Repository

This repository is not a production application.

It exists to:

Validate CloudBees multibranch configuration

Confirm PR build behavior

Confirm merge-to-main triggers

Test Maven caching

Serve as a reference/template for other services

Once CI behavior is verified, you can safely add:

SonarQube

Trivy

Kaniko / Docker builds

Artifact registration

Deployment stages

✅ Key Guarantees

✔ One Jenkinsfile
✔ Multiple branches
✔ Automatic PR builds
✔ Automatic main builds after merge
✔ No duplicate builds
✔ No hardcoded branches
✔ Enterprise-grade CI pattern

🧠 Best Practices Followed

Multibranch over classic pipelines

Template-driven governance

Branch-aware Jenkinsfile

No deploy from feature or PR

Minimal stages for early validation

📌 Next Steps (Optional)

Add Dockerfile + Kaniko build

Enable security scanning

Register build artifacts

Add environment deployments

Convert this into a reusable CI standard

📘 Summary

Feature → PR → Main → CI validation

This setup reflects CloudBees-recommended enterprise CI design and provides a solid foundation for scaling to real microservices.

If you want, I can next:

Add the Pipeline Template Catalog YAML inline here

Add a pipeline diagram

Turn this into an internal CI guideline

Just say 👍
