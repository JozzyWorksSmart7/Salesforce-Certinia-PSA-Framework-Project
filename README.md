# Salesforce-Certinia-PSA-Framework-Automation-Project 
Project Type: Data Architecture, Automation and Security Optimization (Certinia/FinancialForce Replication)
Environment: Salesforce Developer Org

Executive Summary

This project bridges the gap between sales wins and operational execution. In scaling professional service agencies, the handoff between Sales Cloud (Opportunities) and project delivery teams is notoriously manual and error-prone.
To solve this, I engineered a functioning framework replica of Certinia PSA inside a Salesforce sandbox environment. This system completely automates post-sale project creation, manages resource allocation and deploys strict validation logic to protect the company's financial metrics and tracking integrity.

 1. Architecture & Schema Design

To mirror a standard enterprise system layout, I created custom objects using precise structural relationships:
• PSA Project (PSA_Project__c): The primary container tracking post-sale timelines and total budgets. Includes an automatic Roll-Up Summary field calculating overall employee hours.
• Project Assignment ('Project_Assignment__c'): A junction object linking a unique worker (Contact) to an active engagement, calculating individual hourly billing metrics.
• Timecard (Timecard__c): The operational core where resources input weekly hours worked, using automated sequential formatting (TC-{0000}) to enforce systemic order.

 2. Automated Sales-to-Service Handoff (Salesforce Flow)

I designed and activated an optimized Record-Triggered Flow configured for Actions and Related Records to execute when an Opportunity hits Closed Won.
Mapping Logic Implemented:
• Name  {!$Record.Name} (Maintains record identity)
• Account__c  {!$Record.AccountId} (Maintains client structural links)
• Total_Budget__c  {!$Record.Amount} (Transfers financial capital directly)
• Project_Status__c  Active (Instantly flags project as operational)

 3. Financial Compliance Guardrail (Validation Rule)

To prevent tracking errors and data corruption, I created a cross-object Validation Rule on the Timecard object to instantly block hours from leaking onto closed accounts.
Formula Logic Deployed:
text
OR(
    ISPICKVAL(PSA_Project__r.Project_Status__c, "Closed"),
    AND(
        ISCHANGED(Hours_Worked__c),
        ISPICKVAL(PSA_Project__r.Project_Status__c, "Closed")
    )
)

Error Message Displayed: " You cannot log or modify hours on this project because it has been marked as Closed. Please contact your Project Manager."
