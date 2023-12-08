# TB Household Contact Investigation Design { #tb-hh-design }

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
    title Reporting and Calculation of TPT Outcomes
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

