# TB Household Contact Investigation Design { #tb-hh-design }

## Use Case

The TB Household Contact Investigation Tracker enables **registration and longitudinal tracking of household contacts of bacteriologically confirmed pulmonary TB cases**. The program captures a recommended set of data points required for contact tracing and data analysis purposes. The data points include baseline and demographic information about the case, index case information, screening results, X-ray, IGRA, TST, Active TB test results, TPT eligibility, TPT regimens provided and outcomes.

**This tracker program is not designed to support clinical management nor patient care**. Rather, the program serves as an electronic registry that supports decentralized electronic data capture of case surveillance data down to health facilities.

Household Contact Investigation tracker is closely linked with the [TB Case Surveillance tracker](#tb-cs-design). 

## Program Structure

The Household Contact Investigation tracker has been designed to reflect a **more generic workflow** able to integrate data entry both from the health facility side (e.g. clinicians and nurses), and the laboratory side. Workflows in countries may vary and the case surveillance program should be adapted as needed to local context.

> **IMPORTANT**
>
> Please note that given the generic design of the tracker, this system design guide contains throughout the sections useful information, considerations, and justifications that can be of great importance for the implementation. The document should therefore be thoroughly revised both by the requesting and implementing parties.

![Theoretical workflow](resources/images/tb_cs_001.png)

Following this initial workflow, the program has been structured as follows:

![Data flow within the tracker stages](resources/images/tb_cs_029.png)

### Rationale for Program Structure

The design of the tracker has been improved to expand the information that could potentially be collected from the laboratory side. The expansion touched of course the metadata, which is now more comprehensive and allows to **discern between diagnostic and monitoring tests**, but it also created a more generic baseline upon which implementers and countries could build their own workflow. Depending on the local needs, the “Diagnostic laboratory results” and the “Diagnosis and notification” stages could have different orders; depending on the local resources and connectivity, the data entry could be centralized or decentralized between the laboratory and the health facility; depending on the local data flow, the data could be entered live or retrospectively.

In particular, there could theoretically be **three general scenarios** that could change the flow and the order of the stages:

1) **The laboratory staff receives the tests requests and the “Diagnostic Laboratory Results” is the very first step** in the journey of the suspected case upon their enrollment. This scenario represents the theoretical work flow and should be the target to which countries should aim in the long-term investment in the digitalization of routine health data.

   - In this case the “Enrollment” and “ Diagnostic Laboratory Results” stages are completed directly by the laboratory staff. The “Diagnosis and notification” stage can then occur in parallel or after the registration of the laboratory results. At this point the case can either become an official TB case, in which case they will have to receive a TB registration number in their enrollment stage, or they will receive negative laboratory results and be denotified.

2) **The suspected case gets enrolled and registered from the clinical side of the workflow before the samples are sent to the laboratory.**

   - In this case the “Enrollment” and the “Diagnosis and notification” stages are completed first.
   - The “Diagnostic Laboratory Results” gets completed by the laboratory staff.
   - The clinicians will complete the “Diagnosis and notification“ stage confirming or not their diagnosis and notifying the case if necessary. If the latter happens, the clinician will have to reopen the “Enrollment” and assign a TB registration number to the now-confirmed case.

   Please note that scenarios 1 and 2 are based on the assumption that the laboratory staff has the capacity to enter the data in the system locally and directly in their own facility.

   Should this capacity be inadequate for the local context, then scenario number 3 would occur:

3) **The data entry is centralized and there is no differentiation between laboratory and clinical staff** - the data entry relies on one (or multiple) data encoder.
In this case the stages should be arranged in order to match the local flow of information and data.

Independently from the best fitting scenario for a defined local context,**the adaptation of the tracker workflow and its stages should also take in consideration whether the data entry will occur in real time or if it will rely on batch retrospective data entry**.

## Intended Users

This document is intended for audiences responsible for implementing TB data systems and/or HMIS in countries, including:

- **System admins/HMIS focal points**: those responsible for installing metadata packages, designing and maintaining HMIS and/or TB data systems
- **TB program focal points** responsible for overseeing data collection, management, analysis and reporting functions of the national TB programme
- **Implementation support specialists**, e.g. those providing technical assistance to the TB programme or the core HMIS unit to support & maintain DHIS2 as a national health information system and/or TB data system

## Tracker Program Configuration

![Overview of stages and sections](resources/images/tb_cs_030.png)
## Workflow

```mermaid
---
title: TB Household Contact Investigation
---
%%{init: {'mirrorActors': false } }%%

flowchart LR
  subgraph A[Identification of Household contacts]
      direction TB
        subgraph AA[TB Case Surveillance]
        style AA fill:#7393B3,stroke:#333,stroke-width:4px
          id1([New episode of bacteriologically confirmed pulmonary TB])
        end
        subgraph AB[Household]
          id2[Household contact of the TB patient]
        end
        id1-->id2
  end
  id3{{Relationship widget <br> in TB Case Surveillance <br> tracker}}
  subgraph B[Household Contact Investigation Tracker]
      direction LR
        subgraph BA[Registration and Screening of a Household Contact]
            direction TB
              id4[Enrollment date <br> Unique code <br> Personal details <br> Index case details]
              id5[HIV status <br> Symptoms <br> X-Ray <br> IGRA <br> TST <br> Active TB testing recommendation <br> Active TB testing result]
              id4---id5
        end
        subgraph BB[TB Case Surveillance]
        style BB fill:#7393B3,stroke:#333,stroke-width:4px
          id9([Enrollment in TB Case Surveillance <br> Registration of diagnostic TB test results])
          
        end
        subgraph BC[TPT]
          id7[TPT eligibility <br> TPT start date <br> TPT regimen <br> Expected TPT end date]
        end
        BA --> BC -->BD
        BA -. Active TB testing request .-> BB
        subgraph BD[TPT Outcome]
          id8[TPT outcome <br> Eventual delay in outcome <br> Comments ]
        end
  end
  A <--> id3 <--> B
  
```
## Reporting and Calculation of TPT Outcomes

All indicator data for the Household Contacts is aggregated quarterly. The period for the recording of the outcome is dependent on the length of the TPT regimen. In the current module, the longest recommended TPT regimen is 12 months. The period to report the outcomes for an active cohort is set up to 12 months after the end of the period during which the members of the active cohort are registered in the Household Contacts Investigation program. This period may be subject to change depending on country guidelines (eg. longest regimen available). 
The algorythm for reporting and calculating TPT outcomes in DHIS2 is described in the table below. 

 ```mermaid

%%{init: {'mirrorActors': false, 'theme': 'dark' } }%%

timeline
    section Jan - Mar 2023 <br> 2023Q1
      Active cohort (Household contacts identified during 2023Q1 started on TPT): Included in report if outcome is available during this period
    section Apr - Jun 2023 <br> 2023Q2
      Members of active cohort with an outcome: Included in report if outcome is available during this period
    section Jul - Sep 2023 <br> 2023Q3
      Members of active cohort with an outcome: Included in report if outcome is available during this period
    section Oct - Dec 2023 <br> 2023Q4
      Members of active cohort with an outcome: Included in report if outcome is available during this period
    section Jan - Mar 2024 <br> 2024Q1
      Members of active cohort with an outcome: Included in report if outcome is available during this period: Outcomes for members of active cohort will be reported during this period
    section Apr 2024
      Final Outcomes Report for active cohort is available
```

