# Chronic Care Management (CCM)  
https://www.cms.gov/Outreach-and-Education/Medicare-Learning-Network-MLN/MLNProducts/Downloads/ChronicCareManagement.pdf

# Public Data Resource  

[Place_of_Service_Code](https://www.cms.gov/Medicare/Coding/place-of-service-codes/Place_of_Service_Code_Set)

[Medicare-Provider-Charge-Data/Physician-and-Other-Supplier](https://www.cms.gov/Research-Statistics-Data-and-Systems/Statistics-Trends-and-Reports/Medicare-Provider-Charge-Data/Physician-and-Other-Supplier)

[CMS DPC API](https://dpc.cms.gov/data)

[Clinical Classifications Software Refined (CCSR)](https://www.hcup-us.ahrq.gov/toolssoftware/ccsr/ccs_refined.jsp)

https://data.cms.gov/provider-data/topics/hospitals  

[MEDPAR Limited Data Set (LDS) - Hospital (National) | CMS](https://www.cms.gov/Research-Statistics-Data-and-Systems/Files-for-Order/LimitedDataSets/MEDPARLDSHospitalNational)

[Data Documentation - MedPAR](https://resdac.org/cms-data/files/medpar/data-documentation)  # deidentified data

[Quality Net](https://qualitynet.cms.gov/inpatient/hac/resources)

[IPPS Final Rule](https://www.cms.gov/medicare/acute-inpatient-pps/fy-2022-ipps-final-rule-home-page)

[HRRP Definition](https://www.law.cornell.edu/cfr/text/42/412.152)
[NTIS Death Maste](https://dmf.ntis.gov/)

# Place of service

https://www.cms.gov/Medicare/Coding/place-of-service-codes/Place_of_Service_Code_Set

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
icd10_ccs = list(
  # https://www.hcup-us.ahrq.gov/toolssoftware/ccsr/prccsr.jsp#download
    pcs = read.csv(file.path("PRCCSR_v2021-1.csv"), stringsAsFactors = F) %>% 
        cmsdata::colnames_tolower() %>% 
        cmsdata::colnames_wiper("\\.", "_") %>% 
        cmsdata::colnames_wiper("^x_|_$", "") %>% 
        dplyr::mutate(
            icd_10_pcs = gsub("^'|'$", "", icd_10_pcs),
            prccsr    = gsub("^'|'$", "", prccsr),
            icd10_type      = "icd10pcs",
            clinical_domain = gsub("^'|'$", "", clinical_domain)
        ) %>%
        dplyr::rename(
          ccs        = prccsr,
          icd10_code = icd_10_pcs,
                      ccs_name = prccsr_description,
                      icd10_name =icd_10_pcs_description) %>% 
        as.data.frame(), 
    # https://www.hcup-us.ahrq.gov/toolssoftware/ccsr/dxccsr.jsp#download
    cm = read.csv(file.path("DXCCSR_v2021-2.csv"), stringsAsFactors = F) %>% 
        cmsdata::colnames_tolower() %>% 
        cmsdata::colnames_wiper("\\.", "_") %>% 
        cmsdata::colnames_wiper("^x_|_$", "") %>% 
        dplyr::mutate(
            icd_10_cm_code = gsub("^'|'$", "", icd_10_cm_code),
            icd_10_cm_code = gsub("(.{3})(.*)", "\\1.\\2", icd_10_cm_code) %>% 
                gsub("\\.$","", .),
            ccs    = gsub("^'|'$", "", ccs_category_1),
            icd10_type      = "icd10cm" 
        ) %>%
        dplyr::rename(icd10_code = icd_10_cm_code,
                      ccs_name = ccs_category_description,
                      icd10_name = icd_10_cm_code_description) %>% 
        as.data.frame()
)  %>% 
    lapply(function(x){
        dplyr::select(x, ccs, ccs_name, icd10_code, icd10_name, icd10_type)
    }) %>% 
    do.call(rbind, . )

planned_admission_ver4 = list(
    # table 1 pcs ccs 
    # Table PA1. Procedure Categories That Are Always Planned in the Planned Admission Algorithm Version 4.0
    pa1 = readxl::read_excel(path = file.path(
        "C:/planned_admission","2019ACO38ValueSet.xlsx"
    ), sheet = "MCC PAA PA1", skip = 1) %>% 
        as.data.frame() %>% 
        cmsdata::colnames_wiper(" ", "_") %>% 
        dplyr::mutate(source = "PA1") %>% 
        dplyr::rename(ccs = AHRQ_ICD10_PROCEDURE_CCS,
                      ccs_name = CCS_Description) %>% 
        merge(
            y = icd10_ccs %>% 
                subset(icd10_type %in% 'icd10pcs') %>% 
                dplyr::select(ccs, icd10_code, icd10_name, icd10_type),
            by = 'ccs',
            all.x=T
        ),
    
    # table 2 cm ccs 
    # Table PA2. Diagnosis Categories That Are Always Planned in the Planned Admission Algorithm Version 4.0
    pa2 = readxl::read_excel(path = file.path("2019ACO38ValueSet.xlsx"), sheet = "MCC PAA PA2", skip = 1) %>% 
        cmsdata::colnames_wiper(" ", "_") %>% 
        dplyr::mutate(# group = "ICD10_Diag_CCS",
            source = "PA2") %>% 
        dplyr::rename(ccs = AHRQ_ICD10_DIAG_CCS,
                      ccs_name = CCS_Description) %>% 
        merge(
            y = icd10_ccs %>% 
                subset(icd10_type %in% 'icd10cm') %>% 
                dplyr::select(ccs, icd10_code, icd10_name, icd10_type),
            by = 'ccs',
            all.x=T 
        ),
    
    # table 3 pcs ccs  
    # Table PA3. Procedure Categories That Are Potentially Planned 
    # in the Planned Admission Algorithm Version 4.0 
    # 
    pa3a = readxl::read_excel(path = file.path("2019ACO38ValueSet.xlsx"), sheet = "MCC PAA PA3", skip = 1, n_max = 63) %>% 
        as.data.frame() %>% 
        cmsdata::colnames_wiper(" ", "_") %>% 
        dplyr::mutate(`Associated_CCS_Procedure_Category_(Individual_ICD-10-PCS_codes_only)` = NULL,
            source = "PA3a") %>% 
        dplyr::rename(ccs = AHRQ_ICD10_PROC_CCS,
                      ccs_name = CCS_Description) %>% 
        merge(
            y = icd10_ccs %>% 
                subset(icd10_type %in% 'icd10pcs') %>% 
                dplyr::select(ccs, icd10_code, icd10_name, icd10_type),
            by = 'ccs',
            all.x=T
        ),
    
    # table 4 ccs diag 
    # Table PA4.  Acute Diagnosis Categories in the Planned Admission Algorithm Version 4.0
    pa4a = readxl::read_excel(path = file.path("2019ACO38ValueSet.xlsx"), sheet = "MCC PAA PA4", skip = 1, n_max = 80) %>% 
        as.data.frame() %>% 
        cmsdata::colnames_wiper(" ", "_") %>% 
        dplyr::mutate(
            `Associated_CCS_Diagnosis_Category_(Individual_ICD-10-CM_codes_only)` = NULL,
            source = "PA4a"
        ) %>% 
        dplyr::rename(
            ccs = `Category/Code`,
            ccs_name = Description
        ) %>% 
        merge(
            y = icd10_ccs %>% 
                subset(icd10_type %in% 'icd10cm') %>% 
                dplyr::select(ccs, icd10_code, icd10_name, icd10_type),
            by = 'ccs',
            all.x=T
        ) ,
    
    
    # table 3 pcs 
    pa3b = readxl::read_excel(path = file.path("2019ACO38ValueSet.xlsx"), sheet = "MCC PAA PA3", skip = 65) %>% 
        as.data.frame() %>% 
        cmsdata::colnames_wiper(" |-|\\(Individual ICD-10-PCS codes only\\)", "_")  %>% 
        dplyr::rename(icd10_code = `ICD_10_PCS`,
                      icd10_name = ICD_10_PCS_Description,
                      associated_ccs_procedure = `Associated_CCS_Procedure_Category__`) %>% 
        dplyr::mutate(# group = "icd10pcs",
            source = "PA3b",
            ccs = gsub('CCS (\\d{1,5}):.*', "\\1", associated_ccs_procedure),
            ccs_name = gsub("CCS.*: (.*)",  "\\1", associated_ccs_procedure),
            icd10_type = "icd10pcs"
        ),
    
    # table 4 cm codes 
    pa4b = readxl::read_excel(path = file.path("2019ACO38ValueSet.xlsx"), sheet = "MCC PAA PA4", skip = 82) %>% 
        as.data.frame() %>% 
        cmsdata::colnames_wiper(" |-|\\(Individual ICD-10-CM codes only\\)", "_")  %>% 
        dplyr::rename(icd10_code = `ICD_10_CM`,
                      icd10_name = ICD_10_CM_Description,
                      associated_ccs = `Associated_CCS_Diagnosis_Category__`) %>% 
        dplyr::mutate(# group = "icd10cm",
            source = "PA4b",
            ccs = gsub('CCS (\\d{1,5}):.*', "\\1", associated_ccs),
            ccs_name = gsub("CCS.*: (.*)",  "\\1", associated_ccs),
            icd10_type = "icd10cm"
        )
) %>% 
    lapply(as.data.frame) %>% 
    lapply(function(x){
        dplyr::select(x, ccs, ccs_name,icd10_code, icd10_name, icd10_type, source)
    }) %>% 
    do.call(rbind.data.frame, .)
```

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

# [NQF Measures](https://www.qualityforum.org/QPS/)
- 2888 Risk-Standardized Acute Admission Rates for Patients with Multiple Chronic Conditions  
- ACO37 - HF
- ACO27 - HbA1c
- ACO38 - comobidities
- ACO09 - COPD/Asthma
- NQF1550 - CJR
- 
# Elixhauser
```
# 2021 update
# https://www.hcup-us.ahrq.gov/toolssoftware/comorbidityicd10/comorbidity_icd10.jsp
elixhauser_icd10_grouping_ver2021_1 <- . <- readLines( file.path(
        "C:/Elixhauser/ElixhauserComorbidity_v2021-1",
        "Comorb_ICD10CM_Format_v2021-1.sas"
    )) %>% {
        start = (2+which(grepl("Proc format lib=library;", .)))
        end = which(grepl(" other = ",.))[1] -2
        (line = .[start:end])
    }  %>% {
        splitAt2 <- function(x, pos) {
            out <- list()
            pos2 <- c(1, pos, length(x)+1)
            for (i in seq_along(pos2[-1])) {
                out[[i]] <- x[pos2[i]:(pos2[i+1]-1)]
            }
            return(out)
        }

        splitAt <- function(x, pos) {
            pos <- c(1L, pos, length(x) + 1L);
            Map(function(x, i, j) x[i:j], list(x), head(pos, -1L), tail(pos, -1L) - 1L)
        }
        splitAt(., which(. %in% ""))
    } %>%
    sapply(function(x) {
        x = subset(x,grepl(".", x))
        out <- data.frame(
            icd10 = gsub('.*"([A-z]+[0-9]+.*?)".*',"\\1",x),
            group = gsub('.*"([A-z]+)".*',  "\\1",x[length(x)]) %>%
                tolower(),
            # desc  = gsub('.*/\\*{1,2}(.{2,})\\*{1,2}/.*',"\\1",x[length(x)]),
            stringsAsFactors = F)
    }, simplify = F) %>% {
        do.call(rbind,.)
    } %>%
    # transform(group = gsub("htn|htncx", tolower("HTN_C (using HTN, HTNCX)"), group)) %>%
    dplyr::mutate(
        icd10std = gsub('(.{3})(.*)',"\\1.\\2",icd10) ,
        icd10std = gsub("\\.$", "", icd10std)
    ) %>% 
    # subset((group) %in% tolower(ahc29$name))  %>% 
    as.data.frame()

```
