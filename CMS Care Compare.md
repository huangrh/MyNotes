# Public Data Resource  
https://data.cms.gov/provider-data/topics/hospitals  

[MEDPAR Limited Data Set (LDS) - Hospital (National) | CMS]()https://www.cms.gov/Research-Statistics-Data-and-Systems/Files-for-Order/LimitedDataSets/MEDPARLDSHospitalNational

[Data Documentation - MedPAR](https://resdac.org/cms-data/files/medpar/data-documentation)

[Quality Net](https://qualitynet.cms.gov/inpatient/hac/resources)

[IPPS Final Rule](https://www.cms.gov/medicare/acute-inpatient-pps/fy-2022-ipps-final-rule-home-page)

[HRRP Definition](https://www.law.cornell.edu/cfr/text/42/412.152)

# Planned readmission version 4

#### The Planned Readmission Algorithm has three fundamental principles  

1. A few specific, limited types of care are always considered planned
- obstetric delivery,   
- transplant surgery,   
- maintenance chemotherapy/immunotherapy,   
- rehabilitation  
2. Otherwise, a planned readmission is defined as a non-acute readmission for a scheduled procedure  
3. Admissions for acute illness or for complications of care are never planned. The algorithm was developed in 2011 as part of the Hospital-Wide Readmission measure. In 2013, CMS applied the algorithm to its other readmission measures.  


#### Algorithm: In brief, the algorithm identifies a short list of always planned admissions.   
- PA1: The **principal procedure**:  major organ transplant or maintenance chemotherapy; See MCC PAA PA1 : Transplant (bone marrow, kidney, other organ)   
- PA2: The **principal discharge diagnosis** MCC PAA PA2: radiotherapy/chemotherapy, rehabilitation)    
- PA3: A potentially planned procedure (for example, total hip replacement or cholecystectomy; See MCC PAA PA3: **principle procedure.**)   
- PA4: a non-acute **principal discharge diagnosis** code (See MCC PAA PA4 for acute diagnoses).   
- Admissions that include potentially planned procedures that might represent complications of ambulatory care, such as cardiac catheterization, are not considered planned.  

#### CCSR for icd10-cm diag.

https://www.hcup-us.ahrq.gov/toolssoftware/ccsr/dxccsr.jsp#download  
Clinical Classifications Software Refined (CCSR) for ICD-10-CM Diagnoses  
https://www.hcup-us.ahrq.gov/toolssoftware/ccsr/prccsr.jsp#download  

#### Planned readmission code    
https://qualitynet.cms.gov/inpatient/measures/readmission/methodology  
https://qualitynet.cms.gov/files/60943ca9fd340b002259fe16?filename=2021_HWR.xlsx  

```
dplyr::mutate(
    # planned admission
    principle_diag_icd10   = get_first_icd10(x=.$diag_icd10),
    principle_proc_icd10   = get_first_icd10(x=.$proc_icd10),

    planned_admission_pa1  = ifelse(
        principle_proc_icd10 %in% codes$planned_admission_icd10pa1_list,
        T, F),

    planned_admission_pa2  = ifelse(
        principle_diag_icd10 %in% codes$planned_admission_icd10pa2_list,
        T, F),
    planned_admission_pa34 = ifelse(
        principle_proc_icd10 %in% codes$planned_admission_icd10pa3_list &
            !(principle_diag_icd10 %in% codes$planned_admission_icd10pa4_list),
        T, F),
    # Planned admission includes inpatients and does not include Emergency and Observation visits, see measure definition.
    planned_admission      = ifelse(
        (planned_admission_pa1 | planned_admission_pa2 | planned_admission_pa34) &
            grepl('inpatient', pt_class),
        T, F)
)  %>%
```
