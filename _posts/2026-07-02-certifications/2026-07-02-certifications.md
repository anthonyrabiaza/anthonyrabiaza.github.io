---
layout: post
title:  "My Professional Certifications"
permalink: /certifications/
---

<style>
.cert-dashboard { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif; }
.cert-stats { display: flex; justify-content: space-around; text-align: center; margin: 1.5rem 0 2rem; flex-wrap: wrap; gap: 1rem; }
.cert-stats .stat-value { font-size: 2rem; font-weight: 700; color: #000; }
.cert-stats .stat-label { font-size: 0.85rem; opacity: 0.7; margin-top: 0.2rem; }
.cert-chart { margin: 1.5rem 0 2rem; }
.chart-row { display: flex; align-items: center; margin-bottom: 0.6rem; gap: 0.75rem; }
.chart-label { width: 380px; flex-shrink: 0; text-align: right; font-size: 0.85rem; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
.chart-bar-bg { width: 200px; flex-shrink: 0; background: rgba(128,128,128,0.15); border-radius: 4px; height: 22px; position: relative; }
.chart-bar { height: 100%; border-radius: 4px; transition: width 0.6s ease; min-width: 2px; }
.chart-count { min-width: 28px; text-align: right; font-size: 0.85rem; font-weight: 600; }
.cert-filters { display: flex; flex-wrap: wrap; gap: 0.5rem; margin: 1.5rem 0; padding: 1rem 0; border-top: 1px solid rgba(128,128,128,0.2); border-bottom: 1px solid rgba(128,128,128,0.2); }
.cert-filter-btn { padding: 0.4rem 0.9rem; border-radius: 20px; border: 1px solid rgba(128,128,128,0.3); background: transparent; cursor: pointer; font-size: 0.85rem; transition: all 0.2s; color: inherit; }
.cert-filter-btn:hover { border-color: #000; }
.cert-filter-btn.active { background: #000; color: #fff; border-color: #000; }
.cert-filter-btn .count { font-weight: 700; margin-left: 0.3rem; }
.cert-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(320px, 1fr)); gap: 1rem; margin-top: 1.5rem; }
.cert-card { display: flex; align-items: center; gap: 1rem; padding: 1rem; border-radius: 10px; background: rgba(128,128,128,0.08); transition: transform 0.2s, box-shadow 0.2s; }
.cert-card:hover { transform: translateY(-2px); box-shadow: 0 4px 12px rgba(0,0,0,0.15); }
.cert-avatar { width: 72px; height: 72px; border-radius: 12px; display: flex; align-items: center; justify-content: center; font-weight: 700; font-size: 1rem; color: #fff; flex-shrink: 0; letter-spacing: 0.5px; }
.cert-badge { width: 72px; height: 72px; object-fit: contain; flex-shrink: 0; }
.cert-info .cert-name { font-weight: 600; font-size: 0.9rem; line-height: 1.3; }
.cert-info .cert-category { font-size: 0.78rem; opacity: 0.6; margin-top: 0.15rem; }
.cert-card.hidden { display: none; }
@media (max-width: 600px) {
  .chart-label { width: 160px; font-size: 0.75rem; }
  .cert-grid { grid-template-columns: 1fr; }
  .cert-stats .stat-value { font-size: 1.5rem; }
}
</style>

<div class="cert-dashboard">

<div class="cert-stats">
  <div><div class="stat-value">70</div><div class="stat-label">Total certifications</div></div>
  <div><div class="stat-value">9</div><div class="stat-label">Domains</div></div>
  <div><div class="stat-value">13</div><div class="stat-label">Largest domain</div></div>
  <div><div class="stat-value">20+</div><div class="stat-label">Years experience</div></div>
</div>

<div class="cert-chart" id="certChart"></div>

<div class="cert-filters" id="certFilters"></div>

<div class="cert-grid" id="certGrid"></div>

</div>

<script>
(function() {
  var categories = [
    {
      name: "Integration & API Platforms",
      color: "#1e3a8a",
      certs: [
        "Dell Boomi Certified Professional Architect",
        "Dell Boomi Certified Professional Developer",
        "Dell Boomi Certified Associate Developer",
        "Dell Boomi Certified Production Administrator",
        "Dell Boomi Professional API Design and Management",
        "Dell Boomi Professional Sales Accreditation",
        "Dell Boomi Pre-Sales Technical Certification",
        "MuleSoft Certified Architect - Solution Design Specialist",
        "MuleSoft Certified Developer - Integration Professional",
        "MuleSoft Certified Developer - Integration and API Associate",
        "MuleSoft Certified Developer - API Design Associate",
        "Kong Gateway Foundations"
      ]
    },
    {
      name: "Event-Driven & Messaging",
      color: "#ff8a65",
      certs: [
        "Confluent Fundamentals Accreditation - Apache Kafka",
        "Solace Certified Solutions Consultant",
        "Solace Certified Agent Mesh Practitioner",
        "Solace Certified Event-Driven Integration Architect - Boomi",
        "Solace Certified Event-Driven Integration Architect - MuleSoft",
        "Boomi Associate Event Streams Certification"
      ]
    },
    {
      name: "Cloud Platforms",
      color: "#5b8def",
      certs: [
        "AWS Certified Solutions Architect Professional",
        "AWS Certified Solutions Architect Associate",
        "Oracle Cloud Infrastructure 2021 Architect Professional",
        "Oracle Cloud Infrastructure 2019 Architect Associate",
        "Microsoft Certified - Azure Fundamentals",
        "Alibaba Cloud Certified Associate - Cloud Computing",
        "Alibaba Cloud Certification - Elastic Compute Service",
        "Alibaba Cloud Certification - Alibaba Cloud Database",
        "Alibaba Cloud Certification - Cloud-Native Apps",
        "Alibaba Cloud Certification - Network and Security",
        "Alibaba Cloud Certification - ElasticSearch"
      ]
    },
    {
      name: "Automation & Low-Code",
      color: "#6fcf97",
      certs: [
        "Workato Automation Pro I (2020)",
        "Workato Automation Pro II (2020)",
        "Workato Automation Pro I (2024)",
        "Workato Automation Pro II (2024)",
        "Workato Automation Pro III (2024)",
        "Workato Orchestrate Essentials",
        "n8n Level 1",
        "n8n Level 2",
        "Dell Boomi/ManyWho Associate Flow Developer",
        "Dell Boomi/ManyWho Professional Flow Developer",
        "Automation Anywhere Certified Advanced RPA Professional",
        "Certified Mendix Rapid Developer"
      ]
    },
    {
      name: "Cybersecurity & Identity",
      color: "#6b21a8",
      certs: [
        "ISC2 Certified Information Systems Security Professional (CISSP)",
        "CompTIA Security+ Certified CE",
        "EC-Council Certified Ethical Hacker (CEH) Practical",
        "Okta Certified Professional - Hands-On Configuration",
        "Fortinet Network Security Expert (NSE 1)",
        "Fortinet Network Security Expert (NSE 2)",
        "ICSI Certified Network Security Specialist",
        "CyberArk Certified Trustee"
      ]
    },
    {
      name: "AI, Data & Machine Learning",
      color: "#56cccc",
      certs: [
        "Google Cloud Professional Data Engineer",
        "Google Cloud Generative AI Leader",
        "Microsoft Certified - Power Platform Fundamentals",
        "Databricks Lakehouse Fundamentals Accreditation",
        "Dataiku Core Designer",
        "Dataiku ML Practitioner",
        "Oracle Cloud Infrastructure AI Foundations Associate",
        "Oracle Cloud Infrastructure Generative AI Professional",
        "Boomi Associate Master Data Hub Certification"
      ]
    },
    {
      name: "Observability & Monitoring",
      color: "#f2c94c",
      certs: [
        "Splunk Core Certified User",
        "New Relic APM Fundamental Certification",
        "New Relic Full Stack Observability Practitioner",
        "Sumo Logic Certified Fundamentals",
        "Sumo Logic Certified Cloud Security Monitoring & Analytics"
      ]
    },
    {
      name: "Project Management & Enterprise Architecture",
      color: "#000",
      certs: [
        "PRINCE2 Foundation",
        "PRINCE2 Registered Practitioner",
        "TOGAF 9 Foundation",
        "TOGAF 9 Certified"
      ]
    },
    {
      name: "Kubernetes, Network & Reliability",
      color: "#eb5757",
      certs: [
        "CNCF Certified Kubernetes Application Developer (CKAD)",
        "Certified Calico Operator: L1-K8s Networking and Security",
        "Gremlin Certified Chaos Engineering Practitioner"
      ]
    }
  ];

  // Badge image lookup: exact cert name -> filename in assets/certifications/badges/
  // Certs without an entry fall back to a colored initials avatar.
  var BADGE_BASE = "{{ '/assets/certifications/badges/' | relative_url }}";
  var badges = {
    // Integration & API Platforms
    "Dell Boomi Certified Professional Developer": "dell-boomi-logo.png",
    "Dell Boomi Certified Professional Architect": "dell-boomi-logo.png",
    "Dell Boomi Certified Associate Developer": "dell-boomi-logo.png",
    "Dell Boomi Certified Production Administrator": "dell-boomi-logo.png",
    "Dell Boomi Professional API Design and Management": "dell-boomi-logo.png",
    "Dell Boomi Professional Sales Accreditation": "dell-boomi-logo.png",
    "Dell Boomi Pre-Sales Technical Certification": "dell-boomi-logo.png",
    "Boomi Associate Master Data Hub Certification": "boomi-master-data-hub.png",
    "MuleSoft Certified Architect - Solution Design Specialist": "mulesoft-architect-solution-design.png",
    "MuleSoft Certified Developer - Integration Professional": "mulesoft-integration-professional.png",
    "MuleSoft Certified Developer - Integration and API Associate": "mulesoft-integration-api-associate.png",
    "MuleSoft Certified Developer - API Design Associate": "mulesoft-api-design-associate.png",
    "Kong Gateway Foundations": "kong-gateway-foundations.png",
    // Event-Driven & Messaging
    "Confluent Fundamentals Accreditation - Apache Kafka": "confluent-kafka-fundamentals.png",
    "Solace Certified Solutions Consultant": "solace-solutions-consultant.png",
    "Solace Certified Agent Mesh Practitioner": "solace-agent-mesh-practitioner.png",
    "Solace Certified Event-Driven Integration Architect - Boomi": "solace-edi-architect-boomi.png",
    "Solace Certified Event-Driven Integration Architect - MuleSoft": "solace-edi-architect.png",
    "Boomi Associate Event Streams Certification": "boomi-event-streams.png",
    // Cloud Platforms
    "AWS Certified Solutions Architect Professional": "aws-sa-professional.png",
    "AWS Certified Solutions Architect Associate": "aws-sa-associate.png",
    "Oracle Cloud Infrastructure 2021 Architect Professional": "oracle-certified-professional.png",
    "Oracle Cloud Infrastructure Generative AI Professional": "oracle-certified-professional.png",
    "Oracle Cloud Infrastructure 2019 Architect Associate": "oracle-certified-associate.png",
    "Oracle Cloud Infrastructure AI Foundations Associate": "oracle-certified-associate.png",
    "Microsoft Certified - Azure Fundamentals": "azure-fundamentals.png",
    "Alibaba Cloud Certified Associate - Cloud Computing": "alibaba-aca-cloud-computing.png",
    "Alibaba Cloud Certification - Elastic Compute Service": "alibaba-technical-certificate.png",
    "Alibaba Cloud Certification - Alibaba Cloud Database": "alibaba-technical-certificate.png",
    "Alibaba Cloud Certification - Cloud-Native Apps": "alibaba-technical-certificate.png",
    "Alibaba Cloud Certification - Network and Security": "alibaba-technical-certificate.png",
    "Alibaba Cloud Certification - ElasticSearch": "alibaba-technical-certificate.png",
    // Automation & Low-Code
    "Workato Automation Pro I (2020)": "workato-automation-pro-1.png",
    "Workato Automation Pro II (2020)": "workato-automation-pro-2.png",
    "Workato Automation Pro I (2024)": "workato-automation-pro-1.png",
    "Workato Automation Pro II (2024)": "workato-automation-pro-2.png",
    "Workato Automation Pro III (2024)": "workato-automation-pro-3.png",
    "Workato Orchestrate Essentials": "workato-orchestrate-essentials.png",
    "Dell Boomi/ManyWho Associate Flow Developer": "dell-boomi-logo.png",
    "Dell Boomi/ManyWho Professional Flow Developer": "dell-boomi-logo.png",
    "n8n Level 1": "n8n-badge-level1.png",
    "n8n Level 2": "n8n-badge-level2.png",
    "Automation Anywhere Certified Advanced RPA Professional": "automation-anywhere-advanced.png",
    "Certified Mendix Rapid Developer": "mendix-rapid-developer.png",
    // Cybersecurity & Identity
    "ISC2 Certified Information Systems Security Professional (CISSP)": "cissp.png",
    "CompTIA Security+ Certified CE": "comptia-security-plus.png",
    "EC-Council Certified Ethical Hacker (CEH) Practical": "ceh-practical.png",
    "Okta Certified Professional - Hands-On Configuration": "okta-professional.png",
    "CyberArk Certified Trustee": "cyberark-trustee.png",
    "Fortinet Network Security Expert (NSE 1)": "fortinet-nse-1.png",
    "Fortinet Network Security Expert (NSE 2)": "fortinet-nse-2.png",
    "ICSI Certified Network Security Specialist": "icsi-network-security.png",
    // AI, Data & Machine Learning
    "Google Cloud Professional Data Engineer": "google-cloud-data-engineer.png",
    "Microsoft Certified - Power Platform Fundamentals": "power-platform-fundamentals.png",
    "Databricks Lakehouse Fundamentals Accreditation": "databricks-lakehouse.svg",
    "Dataiku Core Designer": "dataiku-core-designer.png",
    "Dataiku ML Practitioner": "dataiku-ml-practitioner.png",
    "Google Cloud Generative AI Leader": "google-cloud-genai-leader.png",
    // Observability & Monitoring
    "Splunk Core Certified User": "splunk-core-user.png",
    "New Relic APM Fundamental Certification": "new-relic-foundation.png",
    "New Relic Full Stack Observability Practitioner": "new-relic-full-stack.png",
    "Sumo Logic Certified Fundamentals": "sumo-logic-fundamentals.png",
    "Sumo Logic Certified Cloud Security Monitoring & Analytics": "sumo-logic-cloud-security.png",
    // Project Management & Enterprise Architecture
    "PRINCE2 Foundation": "prince2-foundation.png",
    "PRINCE2 Registered Practitioner": "prince2-practitioner.png",
    "TOGAF 9 Foundation": "togaf-9-foundation.png",
    "TOGAF 9 Certified": "togaf-9-certified.png",
    // Kubernetes, Network & Reliability
    "CNCF Certified Kubernetes Application Developer (CKAD)": "ckad.png",
    "Certified Calico Operator: L1-K8s Networking and Security": "calico-operator.png",
    "Gremlin Certified Chaos Engineering Practitioner": "gremlin-chaos.png"
  };

  // Sort categories by count descending
  categories.sort(function(a, b) { return b.certs.length - a.certs.length; });

  var maxCount = categories.reduce(function(m, c) { return Math.max(m, c.certs.length); }, 0);
  var totalCerts = categories.reduce(function(s, c) { return s + c.certs.length; }, 0);

  // Update stats
  document.querySelector('.cert-stats .stat-value').textContent = totalCerts;
  document.querySelectorAll('.cert-stats .stat-value')[2].textContent = maxCount;

  // Build chart
  var chartHtml = '';
  categories.forEach(function(cat) {
    var pct = (cat.certs.length / maxCount * 100).toFixed(1);
    chartHtml += '<div class="chart-row">' +
      '<div class="chart-label">' + cat.name + '</div>' +
      '<div class="chart-bar-bg"><div class="chart-bar" style="width:' + pct + '%;background:' + cat.color + '"></div></div>' +
      '<div class="chart-count">' + cat.certs.length + '</div></div>';
  });
  document.getElementById('certChart').innerHTML = chartHtml;

  // Build filter buttons
  var filtersHtml = '<button class="cert-filter-btn active" data-filter="all">All<span class="count">' + totalCerts + '</span></button>';
  categories.forEach(function(cat) {
    var shortName = cat.name.replace(/ & /g, ', ').replace(/ and /g, ', ');
    filtersHtml += '<button class="cert-filter-btn" data-filter="' + cat.name + '">' + shortName + '<span class="count">' + cat.certs.length + '</span></button>';
  });
  document.getElementById('certFilters').innerHTML = filtersHtml;

  // Build cert cards
  function getInitials(name) {
    var words = name.replace(/[^a-zA-Z0-9 ]/g, '').split(/\s+/);
    if (words.length === 1) return words[0].substring(0, 2).toUpperCase();
    return (words[0][0] + words[1][0]).toUpperCase();
  }

  function escAttr(s) { return s.replace(/&/g, '&amp;').replace(/"/g, '&quot;'); }

  var gridHtml = '';
  categories.forEach(function(cat) {
    cat.certs.forEach(function(cert) {
      var visual;
      if (badges[cert]) {
        visual = '<img class="cert-badge" loading="lazy" src="' + BADGE_BASE + badges[cert] +
          '" alt="' + escAttr(cert) + ' badge">';
      } else {
        visual = '<div class="cert-avatar" style="background:' + cat.color + '">' + getInitials(cert) + '</div>';
      }
      gridHtml += '<div class="cert-card" data-category="' + cat.name + '">' + visual +
        '<div class="cert-info"><div class="cert-name">' + cert + '</div>' +
        '<div class="cert-category">' + cat.name + '</div></div></div>';
    });
  });
  document.getElementById('certGrid').innerHTML = gridHtml;

  // Filter logic
  document.getElementById('certFilters').addEventListener('click', function(e) {
    var btn = e.target.closest('.cert-filter-btn');
    if (!btn) return;
    document.querySelectorAll('.cert-filter-btn').forEach(function(b) { b.classList.remove('active'); });
    btn.classList.add('active');
    var filter = btn.getAttribute('data-filter');
    document.querySelectorAll('.cert-card').forEach(function(card) {
      if (filter === 'all' || card.getAttribute('data-category') === filter) {
        card.classList.remove('hidden');
      } else {
        card.classList.add('hidden');
      }
    });
  });
})();
</script>
