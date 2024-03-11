# TB Household Contact Investigation Design { #tb-hh-design }

## Intended Users

This document is intended for audiences responsible for implementing TB data systems and/or HMIS in countries, including:

- **System admins/HMIS focal points**: those responsible for installing metadata packages, designing and maintaining HMIS and/or TB data systems
- **TB program focal points** responsible for overseeing data collection, management, analysis and reporting functions of the national TB programme
- **Implementation support specialists**, e.g. those providing technical assistance to the TB programme or the core HMIS unit to support & maintain DHIS2 as a national health information system and/or TB data system

## Use Case

The TB Household Contact Investigation Tracker enables **registration and longitudinal tracking of household contacts of bacteriologically confirmed pulmonary TB cases**. The program captures a recommended set of data points required for contact tracing and data analysis purposes. The data points include baseline and demographic information about the case, index case information, screening results, X-ray, IGRA, TST, Active TB test results, TPT eligibility, TPT regimens provided and outcomes.

**This tracker program is not designed to support clinical management nor patient care**. Rather, the program serves as an electronic registry that supports decentralized electronic data capture of case surveillance data down to health facilities.

Household Contact Investigation tracker is closely linked with the [TB Case Surveillance tracker](#tb-cs-design).

## Program Structure

The Household Contact Investigation tracker has been designed to be used together with the TB Case Surveillance tracker. The registration of household contacts in DHIS2 starts in TB Case Surveillance. Once a new episode of bacteriologically confirmed pulmonary TB is notified, the system user is prompted to enter household data for the patient and register household contact in the Household Contact Investigation tracker using the relationship widget. Any household contact with a request for Active TB testing has to be registered in TB Case Surveillance, where diagnostic laboratory test results will be recorded. Depending on the results, the household contact will either be notified as an episode of TB or continue enrollment in the Household Contact Investigation programme.

> **NOTE**
>
> The linkaage between TB Case Surveillance and Household Contact Investigation is only possible when TB Case Surveillance tracker allows registration of presumptive TB cases.

Workflows in countries may vary and both programs should be adapted as needed to local context.

> **IMPORTANT**
>
> Please note that given the generic design of the tracker, this system design guide contains throughout the sections useful information, considerations, and justifications that can be of great importance for the implementation. The document should therefore be thoroughly revised both by the requesting and implementing parties.

### Theoretical Workflow

```mermaid
---
title: TB Household Contact Investigation
---
%%{init: {'mirrorActors': false } }%%

flowchart TB
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
        subgraph BA[Registration, Screening and Testing of a Household Contact]
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

### Program Structure and Design

The design of the tracker follows WHO recommendations for reporting on TB preventive activities among household contacts. The program allows retrospective and close to real-time data entry. User receives feedback messages and recommendations based on entered data. These recommendations are not restrictive and are meant to support data entry and possibly identify gaps in the quality of service delivery. The program is divided into the following stages: Enrollment, Screening and Testing, TPT and TPT Outcome.

  1. Enrollment

     The stage collects the basic TEI attributes. Some attributes show the “GEN - “ prefix. Detailed information about generic metadata is available in the [**Common Metadata Library**](https://docs.dhis2.org/en/topics/metadata/dhis2-who-digital-health-data-toolkit/common-metadata-library/design.html) documentation section.

  2. Screening and Testing

     The stage begins with the "HIV Status" section. Since screening is not required for children under 5 and PLHIV, capturing age and HIV-status data as early as possible simplifies the workflow. If the person is screened, screening and test results for X-Ray, IGRA and TST are recorded in the corresponding sections. If active TB testing is recommended, requested and conducted, the section for recording the results of diagnostic tests is also available in the "Active TB testing" section. There is no automated data exchange between TB Case Surveillance and Household Contact Investigation. Laboratory stage in the TB Case Surveillance will contain detailed diagnostic testing results. Active TB Testing section in the Household Contact Investigation will only contain information whether the results are positive, negative or unknown. If the case has no symptoms and all test results are negative, the user will be notified that TPT is not indicated.

     **The stage is not repeatable**.

  3. TPT

     The stage begins with TPT Eligibility section. If the case is not TPT eligible, the user will be prompted to provide the reason for non-eligibility and close the enrollment. If the case is eligible, the TPT section will be available, prompting the user to enter data whether the case has started TPT, the start date and the regimen. Expected TPT completion date will be generated based on the length of selected regimen and the user will be prompted to schedule the outcome event based on that date.

     **The stage is not repeatable**. In case the implementation plan suggests registering several TPT stages (ie. reassignment of regimen), it is possible to make the stage repeatable and add the following data elements to the TPT stage:
      - TPT end date
      - Reason for ending TPT

     This will alllow users to track changes in the regimens and calculate the expected TPT completion date. It is also possible to edit regimens available for TPT and adjust autocalculations for expected TPT completion dates.

  4. TPT Outcome

     The date of the TPT outcome event is the date when the outcome is recorded. The outcomes "Lost to follow up" and "Died" are separated from the outcome "Stopped". If the TPT outcome is stopped, the user will be prompted to provide a reason for stopping TPT. The eventual delay in outcome (difference in months between expected outcome date and actuall outcome date will be displayed. The user may then close the enrollment.

### Relationship Configuration

The default design of the tracker foresees the registration of household contacts via the relationship widget in the Tracker Capture/Capture app. The relationship is created between a person enrolled in TB Case Surveillance programme and a person enrolled in the Household Contact Investigation programme. The relationship type is biderictional. More information on the configuration of the relationship type is available in the [installation guide](#tb-hh-installation).

### User Groups

The package utilises the same user groups as the TB Case Surveillance package:

TB Admin: can edit/view metadata; no access to data [all program stages]
TB Data capture: can view metadata, can capture data [all program stages]
TB Access: can view metadata, can view data [all program stages]

## Analytics

Raw data collected in the Household Contact Tracing Investigation package provides numerators and denominators for the recommended analytical outputs. A data exchange component included in the package is configured to aggregate data on a quarterly basis and populate the Household Contacts dashboard, included in the TB HMIS module.

### Aggregate Data Exchange

An aggregate data exchange component is an editable object that is included in the package. It is configured to aggregate program indicator data and assign quarterly values to the corresponding data elements and category option combinations of the [Household Contacts data set](#tb-agg-hh-design), which is part of the DHIS2 TB HMIS package.

### Indicators

Intital package includes a set of program indicators that can be divided into two following groups:

- Identification, screening and TPT

  | Indicator | Definition |
  |-|-|
  | Contacts identified (0-4 years, 5+ years) | All enrollments **with a registration date within the reporting periodr** |
  | Contacts screened (0-4 years, 5+ years) | All enrollments thatflagged as screened **with a registration date within the reporting periodr** |
  | Contacts eligible for TPT (0-4 years, 5+ years) | All enrollments flagged as eligible for TPT **with a registration date within the reporting period** |
  | Contacts started on TPT (0-4 years, 5+ years) | All enrollments flagged as started on TPT **with a registration date within the reporting period** |

- TPT outcomes

  | Indicator | Definition |
  |-|-|
  | Contacts completed TPT (0-4 years, 5+ years) | All enrollments that have completed TPT **with a registration date during the period which is 9 months (3 quarters) prior to the reporting period** |
  | Contacts stopped TPT (0-4 years, 5+ years) | All enrollments that have stopped TPT **with a registration date during the period which is 9 months (3 quarters) prior to the reporting period** |

**Reporting and Calculation of TPT Outcomes**

The actual period for the recording of the outcome is dependent on the length of the TPT regimen. In the current module, the longest recommended TPT regimen is 9 months. The period to report the outcomes for an active cohort is set up to 9 months after the end of the period during which the members of the active cohort are registered in the Household Contacts Investigation program. This period may be subject to change depending on country guidelines (eg. longest regimen available).
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
      Members of active cohort with an outcome: Included in report if outcome is available during this period: Outcomes for members of active cohort will be reported during this period
    section Jan 2024
      Final Outcomes Report for active cohort is available

```

### Line Listing

Household Contact Tracing Investigation package includes a dashboard with 3 line lists:

  | Line List | Description |
  |-|-|
  | Household contacts of bacteriologically confirmed TB patients that are yet to be screened for eligibility | All registered contacts that have no registered screening-related data <br> PLHIV cases and children under 5 will also appear on this list even though screening is not required for these groups |
  | Eligible household contacts yet to start on TPT | All registered contacts that are TPT-eligible and have not started on TPT |
  | TPT clients with no outcome recorded | All clients registered and started on TPT that have not received an outcome <br> This line list includes all clients independent of the period when the TPT outcome is due. In order to access a list of clients with no recorded outcome in the current period, it is recommended to schedule the outcome events based on the expected date of TPT completion in Tracker and generate the list of due and overdue events in the Tracker Capture dashboard interface|

The content of the line lists as well as the definitions may be adjusted according to the implementation requirements.

## References

World Health Organization (20 March 2020). WHO consolidated guidelines on tuberculosis: module 1: prevention: tuberculosis preventive treatment. URL: <https://www.who.int/publications/i/item/9789240001503>

World Health Organization (January 2020). “Definitions and reporting framework for tuberculosis – 2013 revision (updated December 2014 and January 2020)”. URL: <http://apps.who.int/iris/bitstream/handle/10665/79199/9789241505345_eng.pdf;jsessionid=0911B3678B5EF8EBD24C834EA882F199?sequence=1>

World Health Organization (2023). “Consolidated guidance on tuberculosis data generation and use. Module 1. Tuberculosis Surveillance”. Geneva: World Health Organization; 2023. Licence: CC BY-NC-SA 3.0 IGO (in press)
